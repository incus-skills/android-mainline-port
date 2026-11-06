# Kernel Configuration, Resource Ownership and Device-Tree Values

## Recommended route

1. **Baseline**: Use the latest stable kernel plus patches with explicit provenance (an upstream series under review, or community changes imported by commit). Once merged upstream, switch to the released version and remove the local implementation.
2. **Device tree**: Use the upstream SoC dtsi plus a device-specific board file. Derive every value from evidence from the actual device (runtime device tree, PMIC registers, stock debugfs, vendor source); do not copy values from a reference device.
3. **Built-in versus modules**: Build into the kernel, or include completely in the initramfs, the drivers needed for boot, recovery and the debugging channel, as well as resource providers required by the borrowed firmware display. Treat other drivers as modules by default, following upstream practice, and ship the complete module directory.
4. **Resources left active by firmware**: Every clock, power domain and interconnect bandwidth path still in use must have a genuine consumer declared in the device tree, and that consumer must bind before the kernel reclaims unused resources.
5. **Align configuration with the distribution**: Before the first release, compare the kernel configuration against the distribution's generic kernel configuration and fill in commonly used desktop options.

## Completion criteria

- The actual values of key options in the final `.config` are correct; corresponding `.ko` files are present in the delivered module directory and are actually loaded.
- `devices_deferred` is empty, or every entry can be explained.
- No `sync_state` remains pending indefinitely; no global bypasses such as `*_ignore_unused`, `fw_devlink.sync_state=timeout`, or blacklisting selected modules.
- Repeated cold boots are stable with the complete module set and no blacklist.

## Pitfalls

### A driver is built as `=m`, but the image has no module directory, so functionality silently disappears

Markers: 【Frequent】 【Time-consuming】

- **Symptoms**: Permanent `EPROBE_DEFER`; drivers do not bind; the power and volume buttons stop working; remote-processor services do not appear; `reboot bootloader` stops working, but no error is reported.
- **Cause**: The early RAM image contains only Image + DTB + a small initramfs, with no `/lib/modules`. Later, when a module directory is present, another problem appears: some providers are built in (`=y`) while others are modules (`=m`).
- **Commonly affected drivers (Qualcomm)**: `QCOM_GPI_DMA` (SPI touch); `QRTR`/`QRTR_SMD`/`QCOM_PD_MAPPER` (ADSP services); GPU clock controller (the entire GPU chain waits); `PHY_QCOM_QMP_PCIE` and `RFKILL` (Wi-Fi); `POWER_RESET_QCOM_PON` (power and volume buttons); `RTC_DRV_PM8XXX`; `NVMEM_REBOOT_MODE` and `NVMEM_SPMI_SDAM`; `BATTERY_QCOM_BATTMGR`; `QCOM_CPUCP_MBOX` (no frequency scaling); Type-C switch driver (holds up the entire display driver); `OVERLAY_FS`; `SPI_QCOM_GENI`; `FASTRPC`; `SND_SEQUENCER`.
- **How to confirm**: Before suspecting the device tree, check the actual values in the final `.config`, whether the module directory was delivered, `lsmod`, and `devices_deferred`.
- **How to fix**: During first boot, build all boot and recovery dependencies as `=y`. Establish delivery of a complete, versioned module directory as early as possible. Build the entire dependency chain for root boot, recovery and display into the kernel.
- **Prevention**: Watch for dependency propagation through `A depends on B || !B`: if B is `=m`, A is also forced to `=m` (for example, `RFKILL` can turn the entire wireless chain into modules).
- **Applicability**: General; Qualcomm symbols are from 2026.

### A resource still used by firmware has no consumer and is reclaimed by the kernel

Markers: 【Frequent】 【Time-consuming】

- **Symptoms**: The borrowed firmware display goes black or grey after a period of time; GPU GMU timeouts or hangcheck; SPI touch timeouts; in most cases, rebooting does not recover it.
- **Cause**: The kernel disables resources that have no consumers during `late_initcall` (`clk_disable_unused`) and `sync_state` (genpd, interconnect, rpmhpd). The display clocks, power domains and interconnect bandwidth left active by firmware have no declared consumer in the device tree.
- **How to confirm**: Export `/sys/kernel/debug/clk/clk_summary`, `pm_genpd_summary` and `interconnect_summary` once before and once after reclamation; compare which resources were disabled afterwards.
- **How to fix**: Declare the power domains, clocks, interconnect paths and bandwidth on the actual consumer node (for example, `simple-framebuffer`). Build the provider drivers into the kernel and ensure they bind before reclamation.
- **Prevention**: Do not conceal the problem with `clk_ignore_unused`, `pd_ignore_unused`, `fw_devlink.sync_state=timeout` or `iommu.passthrough=1`. These can get an image on screen quickly, but later prevent power saving, suspend and upstream integration. They may be used briefly as diagnostic measures to establish whether resource reclamation is responsible; remove them once confirmed.
- **Applicability**: General, especially evident on Qualcomm; 2026.

### A modular display clock controller causes a “slow-start state”

Markers: 【Time-consuming】

- **Symptoms**: Most boots work normally. On a minority of boots, GPU and touch time out and the whole system becomes slow; it does not recover during that boot, and a single inspection will not detect it.
- **Cause**: When the display clock controller (for example, dispcc) is a module, the borrowed-display driver binds about 15 seconds later, while the display-bus clock is reclaimed after 2–3 seconds. The memory path slows down and does not return to its previous state even after the driver binds.
- **How to confirm**: Compare the simpledrm bind time with the `clk_disable_unused` time; run a small memory-bandwidth test; inspect repeated cold boots.
- **How to fix**: Build the display clock controller into the kernel.
- **Prevention**: Whenever changing resources related to the borrowed display, also check whether the provider is built in and whether it binds before reclamation. Include time and bandwidth budgets in acceptance testing (see `../references/verification.md`).
- **Applicability**: Borrowed-firmware-display route; 2026.

### Partial module blacklists or a pruned module directory are used to “preserve” the boot state, causing reboots with no logs

Markers: 【Critical】

- **Symptoms**: Random whole-device resets; pstore is empty.
- **Cause**: Some providers are not loaded while some `sync_state` operations remain pending indefinitely, leaving the system in an intermediate state that has never been validated.
- **How to fix**: Do not do this. Pruning is for diagnosis only; restore the complete module directory afterwards. Declare resource requirements on the actual consumer.
- **Applicability**: General.

### Dependency loading bypasses a blacklist

- **Symptoms**: An old driver that was blacklisted is still loaded.
- **Cause**: Another module pulls it in through dependency-based `modprobe` (a blacklist only blocks automatic loading by alias); or the blacklist on the command line is truncated.
- **How to fix**: Use `install <module> /bin/false`, or deliver the correct module directory before udev runs. Do not put a long blacklist on the command line.
- **Applicability**: General.

### A device-tree voltage is not on a PMIC driver's supported step, so every regulator in the PMIC disappears

Markers: 【Frequent】 【Critical】

- **Symptoms**: After adding one supply, the display, USB and even the debugging channel all disappear; manual recovery is required.
- **Cause**: A vendor nominal value (for example, 3.300 V or 1.900 V) does not fall on the voltage table used by the mainline regulator driver (for example, an 8 mV step). With `min = max`, there is no valid value, and registration of all regulators in the PMIC fails.
- **How to confirm**: Use the driver's linear range to calculate whether the value is representable.
- **How to fix**: Read the actual value from the PMIC voltage register on the stock system, read-only (for example, 3296 mV or 1896 mV), and set a range that includes it.
- **Prevention**: Mechanically validate every voltage configured with `min = max` during the build.
- **Applicability**: Qualcomm RPMh regulator; general method.

### A power supply that is always on in the stock system is undeclared in mainline, so the peripheral bus is low before any communication

Markers: 【Time-consuming】

- **Symptoms**: A peripheral (for example, a magnetic keyboard bridge) enumerates but transfers no data; all query commands time out; protocol-layer debugging has no effect.
- **Cause**: Some LDOs are always on in the stock device tree, with a nominal consumer in another subsystem (for example, display). Mainline does not declare them, so the bus pull-up has no power.
- **How to confirm**: Read the serial controller's line-state registers and GPIO input levels (note that the pinctrl debug interface shows the output latch, not the input).
- **How to fix**: Take the set difference between all stock `always-on`/`boot-on` supplies and the runtime `regulator_summary`; add those that are missing.
- **Applicability**: General method.

### The serial engine's parent wrapper or GPI DMA is not enabled, so the controller never appears

Markers: 【Frequent】

- **Symptoms**: I2C/SPI/UART child nodes are enabled, but the adapter is never registered; or SPI remains deferred indefinitely and holds up other `sync_state` operations.
- **Cause**: The `qupv3_N` parent-wrapper node is disabled; the `gpi_dma` node is disabled; or the DMA driver is configured as `QCOM_GPI_DMA=m`.
- **How to fix**: Whenever enabling a serial engine, check all three layers together: wrapper node, GPI DMA node and DMA driver built in.
- **Applicability**: Qualcomm; 2026.

### Power votes are permanently lost after OPP returns `-ERANGE`

- **Symptoms**: The whole device silently hangs within tens of milliseconds after the display turns on or resumes.
- **Cause**: To pass mode validation, the clock limit is raised beyond the OPP table. `dev_pm_opp_set_rate` returns `-ERANGE`; after the clock is disabled, the power-domain vote drops to zero and is never restored. Runtime resume enables the clock before restoring the vote.
- **How to fix**: Clamp requested frequency to the top of the OPP table; restore the vote before enabling the clock; do not counteract reclamation with a temporarily held clock reference.
- **Prevention**: Do not treat `-ERANGE` or “OPP not found” as noise.
- **Applicability**: General.

### A new driver is built into the kernel on its first probe, and a failure hangs the whole device

- **Symptoms**: After enabling several multimedia drivers at once, several images all produce a black screen and no USB.
- **Cause**: A clock controller or driver that nobody has exercised before hangs a bus early in boot.
- **How to fix**: Build it as a module for the first test. Load it manually after the debugging channel is established; decide whether to build it in only after it passes.
- **Applicability**: General.

### Reading a debug interface changes the system state

- **Symptoms**: A problem disappears after read-only collection is added, or a new problem appears.
- **Cause**: Reading `clk_summary` wakes some subsystems; reading some sysfs files triggers multiple bus transactions; reading registers in a powered-off module can hang the device.
- **How to fix**: Establish the side effects before observation. Add the same collection code to the control group. For runtime power-related reads, hold a power reference first.
- **Applicability**: General.

### Integer values appear as phandle labels when a device tree is decompiled

- **Symptoms**: A numeric value appears to be an unrelated node name.
- **How to fix**: Verify values only against the raw output of `dtc -I dtb -O dts`; do not use a beautifying tool.
- **Applicability**: General.

### USB gadget cannot recover after UDC reconstruction because of `-ENOENT`

- **Symptoms**: A `kobject_move` failure is reported after USB-role switching or gadget reconstruction.
- **Cause**: The successful path through `device_move` leaks an old glue-dir reference (an upstream issue in the device-model layer).
- **How to fix**: An upstream patch fixes the reference count; first check whether it has already been merged.
- **Applicability**: Mainline 7.x; 2026.

### The kernel configuration lacks common desktop options

- **Symptoms**: Bluetooth LE keyboards and mice do not work (missing `UHID`); memory-pressure protection is absent (missing `PSI` and zram); firewall and WireGuard are unavailable; `.zst` compressed firmware cannot be read (missing `FW_LOADER_COMPRESS`); snap mounting fails (missing SquashFS XZ/LZO/XATTR); ALSA hooks require `snd-seq`.
- **How to fix**: Before the first release, compare item by item with the distribution's generic kernel configuration. Submit each functional change separately so it can be reverted independently.
- **Applicability**: Desktop distributions; 2026.

### Module ABI breakage cannot be detected when `MODVERSIONS` is disabled

- **Symptoms**: Old modules with the same kernel version string are loaded by a new kernel and behave incorrectly.
- **How to fix**: Increment the version (ABI) for every packaged kernel build; never reuse one. See `ubuntu-userspace.md`.
- **Applicability**: General.

## Unresolved

- Why do early debugging parameters (`keep_bootcon`, `initcall_debug`, `ignore_loglevel`) affect panel bring-up timing? Some projects still ship with them; binary-search images have been prepared but not run.
