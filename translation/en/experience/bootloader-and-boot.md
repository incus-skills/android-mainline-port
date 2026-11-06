# Bootloader, First Boot, Debugging Channel and Recovery

## Recommended route (Qualcomm ABL devices)

1. **Read-only evidence acquisition**: On the stock system, capture the runtime device tree at `/sys/firmware/fdt`, image-header versions for each boot partition, `fastboot getvar all`, and the vbmeta chain using `avbtool info_image`.
2. **Reproduce bootloader device-tree processing offline**: Use libufdt (the implementation actually used by ABL) to apply the stock DTBO to the candidate device tree. First use the stock device tree as a positive control.
3. **Inject symbols at packaging time**: Export the symbols actually referenced by the DTBO (plus symbols required by hypervisor runtime overlays, such as `arch_timer`) into a container node with `status = "disabled"`. Keep the kernel DTS in upstream style; do not add these items to the source.
4. **Use timestamp probes for the first device boot**: Boot with `fastboot boot` using an image that only prints timestamped markers, sets `panic=N`, and reboots after a timeout. This proves that "the handoff happened, how long it ran, and that it can return automatically".
5. **Minimal initramfs**: BusyBox; set all block devices read-only and verify each one individually; bring up USB NCM + DHCP + telnet/SSH; exit on a timer.
6. **RAM root**: Package the official distribution root filesystem as SquashFS, transfer it over the USB network using HTTP, use OverlayFS, then `switch_root`.
7. **Persistent boot**: Determine the image format through tests on the actual device (commonly the same three-part structure as stock—boot/init_boot/vendor_boot—plus a self-signed AVB footer); mount the root partition by PARTLABEL.

The format must be tested on every device. Do not copy it from another device, even one using the same SoC.

## Completion criteria

- **RAM stage**: Within about one second after handoff, USB disconnects and reappears under a new identity; commands can be executed in both directions and dmesg retrieved; the device automatically returns to the stock system when the timer expires.
- **Persistent stage**: Boot partitions read back identically, byte for byte; runtime cmdline and kernel version match the manifest; the injected container in the runtime device tree is disabled; `/sys/kernel/debug/devices_deferred` is empty or explainable; no provider remains stuck in `sync_state` indefinitely.
- **First-boot checklist**: `/proc/device-tree/compatible` and model, `systemd-detect-virt`, the Chassis reported by `hostnamectl`, key options in the final `.config`, `devices_deferred`, and `sync_state` status.

## Pitfalls

### Mainline device tree has no `__symbols__`; ABL silently rejects it even though `fastboot boot` returns OKAY

Markers: 【Frequent】 【Time-consuming】

- Symptoms: `fastboot boot` displays `Booting OKAY`, but afterwards there is no USB network and no log; sometimes it looks as though the device "returns to Fastboot after a few seconds".
- Cause: ABL applies the DTBO partition (and hypervisor runtime overlays that are not in any partition) to your device tree. Overlay application requires a symbol table; the mainline device tree does not include one by default. When overlay application fails, ABL neither falls back nor reports an error.
- How to confirm: Capture `/sys/firmware/fdt` from the stock system and compare it with the device tree you packaged; reproduce overlay application offline with libufdt, using the stock device tree as a positive control.
- How to fix: At packaging time, export all symbols referenced by the DTBO (or the union of all base-DTB symbols), pointing them to placeholder nodes inside a disabled container. Also export symbols referenced by hypervisor overlays (such as `arch_timer`).
- Prevention: Complete an offline overlay reproduction before first boot. Use `udevadm monitor` to observe USB events when determining whether handoff occurred; do not rely on the Fastboot return value.
- Applicability: Qualcomm ABL (common on platforms after SM8450); 2026.

### Offline reproduction with dtc's fdtoverlay fails even with the stock device tree

- Symptoms: Offline simulation leads to the absurd conclusion that "the stock device tree also fails".
- Cause: ABL uses libufdt, whose behaviour differs from the implementation in dtc.
- How to fix: Reproduce using the implementation actually used by the target system. Always run a known-good positive control first.
- Applicability: General method.

### Placeholder node is in the wrong place: reserved memory is mapped to physical address 0 and crashes, or becomes a "fake consumer" that prevents resources from being released

Markers: 【Time-consuming】 【Critical】

- Symptoms 1: Handoff by ABL silently hangs; earlycon shows a crash in `persistent_ram_new`.
- Symptoms 2: `sync_state` remains pending for rpmhpd, NoC and GCC; the SoC stays at its highest voltage and never enters idle. This problem can mask other power issues for weeks.
- Cause: The placeholder node is placed at the root or includes `#address-cells`, `#size-cells` or `ranges`, causing overlaid reserved memory to be translated to address 0; alternatively, after overlay application the placeholder node acquires a `compatible` and `status = "ok"`, becoming a device that is never bound. fw_devlink parses its properties as phandles, which happen to resolve to power providers.
- How to fix: Put the placeholder nodes inside a container with `status = "disabled"`, no `compatible` and no address properties. `of/platform` traverses only direct children of `/reserved-memory`.
- Prevention: On the day of first boot, check `state_synced` and the list of providers stuck in `sync_state`. After clearing the pending state, retest the entire power lifecycle.
- Applicability: Qualcomm ABL; 2026.

### Bootloader rewrites the root node's compatible and model, breaking all code that uses them to identify the device

Markers: 【Frequent】

- Symptoms: The PD mapper prints `no support for the platform`; sound-card and remote-processor services do not appear; the display driver's UBWC configuration cannot identify the SoC and there is no `card0`; audio quirks are never selected; the installer reports "device model mismatch".
- Cause: ABL overwrites the runtime device-tree root's compatible, model, msm-id and board-id with values from the DTBO root, turning them into a generic platform name (for example, `qcom,<platform>-mtp`).
- How to confirm: On first boot, compare `cat /proc/device-tree/compatible` with the source.
- How to fix: Restore the root identity early in the kernel (recommended; one fix covers all consumers); or add the rewritten name to each driver's match table; make quirks use an unaffected key (such as the ASoC sound-card name); identify the device using Fastboot product, serial and runtime values.
- Prevention: Never use a property that the bootloader rewrites to select a quirk or determine device identity.
- Applicability: Qualcomm ABL; 2026.

### A hypervisor node makes systemd identify the tablet as a virtual machine; a short press of the power button shuts it down

Markers: 【Frequent】

- Symptoms: `systemd-detect-virt` outputs `vm-other`; Chassis from `hostnamectl` is `vm`; a short press of the power button on the login screen shuts down the system; GNOME Settings has no power-button option; services with `ConditionVirtualization=no` are skipped.
- Cause: ABL injects a `/hypervisor` (Gunyah) node, and systemd uses it to detect virtualization.
- How to fix: Hide the node before systemd performs detection (in initramfs or early in sysinit, bind-mount an empty directory over `/sys/firmware/devicetree/base/hypervisor`), or prevent it from being exposed by the kernel. Also set Chassis to `tablet`.
- Prevention: Make `systemd-detect-virt` and Chassis fixed checks in the first-boot checklist.
- Applicability: Qualcomm platforms after SM8450 with Gunyah; 2026.

### Kernel command line comes from device-tree `/chosen/bootargs`, has a length limit, and is silently truncated beyond it

Markers: 【Frequent】

- Symptoms: Changing the cmdline in the image header has no effect; or a module blacklist at the end of the command line does not work and the supposedly blacklisted driver still loads.
- Cause: ABL reads bootargs from the device tree and appends several hundred bytes of its own parameters in place (possibly including dangerous items such as `block2mtd`). The total length has a limit (commonly about 512 bytes).
- How to fix: At build time, check that the image-header and device-tree cmdlines agree, assert the expected contents, and reject overlong values. Do not put blacklists on the command line.
- Applicability: Qualcomm ABL; 2026.

### `fastboot boot` and cold boot require different image formats

- Symptoms: With a v4 image, `rdinit` is never executed after `fastboot boot` and an early crash occurs; v2 boots in RAM, but flashing it to a partition and cold-booting goes straight to Fastboot.
- Cause: On GKI devices, ABL ignores the ramdisk field in a v4 boot image (the ramdisk is in init_boot/vendor_boot); during cold boot, the vbmeta chain requires boot/vendor_boot to have AVB footers.
- How to fix: Use v2 (kernel + ramdisk + DTB) for RAM testing. For persistent boot, reproduce the stock three-part structure and add self-signed AVB footers (the test key is accepted while unlocked); keep the rollback index as in stock; reproduce the image-header fields and AVB properties from stock.
- Applicability: Qualcomm GKI devices; 2026.

### Serial number exists only in `/proc/bootconfig`; tools reading `/proc/cmdline` all report an identity mismatch

- Symptoms: In-system switching tools and kernel-update hooks report serial mismatch and refuse to run.
- Cause: Depending on the ABL, `androidboot.serialno` may be placed only in bootconfig for a mainline kernel, placed in cmdline, or omitted from both.
- How to fix: Read in this order: cmdline → bootconfig → report an explicit error if neither contains it; add regression tests.
- Applicability: Qualcomm ABL; 2026.

### `fastboot boot` returns OKAY but remains in Fastboot, with empty pstore: check device-tree identity first

Markers: 【Time-consuming】

- Symptoms: Sending and Booting both return OKAY; there is no USB network; the device remains in Fastboot after tens of seconds; pstore is empty.
- Cause: Before jumping to the kernel, ABL selects and validates the device tree using `qcom,msm-id` and `qcom,board-id` on the device-tree root. If identity does not match, it does not jump, so the kernel leaves no evidence. Packaging tools copy these values from a "stock base device tree"; changing to another stock base device tree can silently produce an image ABL will not accept.
- How to confirm: Decompile the packaged device tree and compare root identity fields item by item with a device tree that has previously booted successfully.
- How to fix: Validate identity during packaging and reject values that do not match the known-good values for this device.
- Prevention: Do not explain this symptom as "the image exceeds the boot partition size". Test with a smaller image to rule that out: the size limit for `fastboot boot` is `max-download-size` (commonly hundreds of MiB), much larger than the boot partition.
- Applicability: Qualcomm ABL; 2026.

### Test a candidate kernel on the installed system without installing the package directly

Markers: 【Critical】

- Symptoms: Directly running `dpkg -i` on a candidate kernel package causes its hooks to rebuild the boot-image set and write it to the active slot. If the candidate is faulty, the device reboots repeatedly and cannot be recovered without someone present.
- How to fix: On the device, extract the module directory with `dpkg-deb -x`, run `depmod`, and generate an initramfs with `dracut --kver` (without writing to a slot). Retrieve the kernel, device tree (verify its identity) and initramfs; create a v2 RAM image; add `softlockup_panic=1 hung_task_panic=1 oops=panic` and `panic=N` to the command line so a hang becomes a reboot back to the installed system. After `fastboot boot`, first set up a timed reboot and only then test. Delete the staged files afterwards. Persistently install through the formal upgrade path only after RAM testing passes.
- Applicability: Devices with self-managed boot images; 2026.

### After Fastboot has been idle for a long time, or while background data writes are still in progress, `fastboot boot` hangs and the device stops responding

Markers: 【Critical】

- Symptoms: The screen remains in Fastboot; `getvar` receives no response; only a long press of the power button works.
- Cause: When flashing a zero-filled image, ABL may return OKAY first and then write the zeros slowly in the background (which can take tens of minutes). Subsequent commands queue or receive no response during this period. The state after a long idle period is also unreliable.
- How to fix: Do not send zero-filled images (see `usb-and-storage.md`). Immediately perform the next step after entering Fastboot; between steps, return to the running system and wait.
- Applicability: Qualcomm ABL; 2026.

### Accessing peripheral registers or clock-debug interfaces before establishing the debugging channel hangs the entire device, with no remote recovery

Markers: 【Frequent】 【Critical】

- Symptoms: After boot there is no USB network and the screen is black; only a long button press recovers the device. The same mistake recurs during later investigation.
- Cause: Initramfs or an early service reads display registers, `clk_summary`, or GPTs from all LUNs before USB is up, touching hardware that is not powered or is protected.
- How to fix: Remove these probes from the default boot path. Perform them only after the channel has been established, when needed.
- Prevention: Statically check at build time that the default boot path contains no `/dev/mem`, `devmem`, or clock-debugfs access.
- Applicability: General; 2026.

### Reading display-controller registers through `/dev/mem` triggers a TrustZone fatal error and resets the SoC

- Symptoms: Read-only evidence acquisition itself causes the device to reboot.
- Cause: A register region removed from a newer-generation DPU is read (for example, part of MDP top), triggering a TZ fatal error.
- How to fix: For the authoritative display-register view, use the stock driver's own debugfs export. Use `/dev/mem` only to read addresses that have been verified.
- Applicability: Qualcomm; 2026.

### Static BusyBox program name or path is changed; services that reference it exit with code 127

Markers: 【Frequent】

- Symptoms: White screen after handoff, debugging channel disappears, and the service log shows `status=127`.
- Cause: The service invokes an applet using its old name.
- How to fix: Keep the BusyBox binary under its original name, and invoke it as `/usr/local/libexec/<project>/busybox <applet>`. Clean up old services before handoff.
- Applicability: General.

### Building a minimal RAM probe on the stock kernel never obtains USB, even after several attempts

- Symptoms: After booting the stock kernel with a custom ramdisk, USB does not enumerate; repeated attempts produce no new information.
- Cause: The stock kernel's USB depends on clock controllers such as display and camera being ready first; the v4 image may also fail to deliver the ramdisk.
- How to fix: Do not pursue this route. Back up and collect evidence by working from the stock system; start the mainline kernel with a timestamp probe.
- Applicability: Qualcomm GKI devices; 2026.

### Cascading problems during the `switch_root` handoff

- Symptoms: telnet accepts a connection and then immediately disconnects; the port is occupied; the network interface is renamed to `enx*`; firmware requests fail in the new root; after running for a while, the desktop is killed by the old watchdog.
- Cause: Orphaned initramfs processes still hold the port; udev renames the USB network interface; a pre-created `/dev/console` causes the conditional devtmpfs mount to be skipped, leaving no `/dev/ptmx`; firmware exists only in initramfs; the initramfs timer has not been cancelled.
- How to fix: Stop old processes before switching root and rebuild the channel inside the new root; mount devtmpfs unconditionally; use a `.link` file to pin the network-interface name by MAC; put firmware in the root filesystem; overlap old and new protections during handoff, and remove the old protection only after the new one is active.
- Applicability: General when writing a custom initramfs.

### The recovery path itself can hang

Markers: 【Critical】

- Symptoms: When the timer expires, the device does not return to Fastboot or the stock system; a person must press a button.
- Cause: In the expiry path `sleep; sync; reboot`, `sync` has no timeout (`reboot -f` also performs sync); the hardware watchdog has never been tested in practice.
- How to fix: Write a marker first, limit the time spent in `sync`, then use `reboot -f -n`; test the watchdog with a controlled failure-to-feed test and confirm that it really resets the device.
- Prevention: Put a time limit on every step of the recovery path and test the final combination in practice.
- Applicability: General.

### systemd tries to open the hypervisor watchdog by default, with a timeout greater than the hardware limit

- Symptoms: An unexpected reset occurs during an ordinary reboot or shutdown.
- Cause: systemd defaults to `RebootWatchdogSec=10min`, while the Gunyah watchdog limit is about 32 seconds; when `wdctl` cannot read sysfs, it opens the device.
- How to fix: Use a drop-in to set `RuntimeWatchdogSec=off` and `RebootWatchdogSec=off`, enable `WATCHDOG_SYSFS`, and do not add an MMIO watchdog node (this makes SCM abandon the hypervisor path).
- Applicability: Qualcomm Gunyah platforms; 2026.

### Software reboot does not enter Fastboot and is misdiagnosed as "bootloader does not support it"

Markers: 【Time-consuming】

- Symptoms: `reboot bootloader` returns to the ordinary system.
- Cause: The reboot reason is stored in a PMIC SDAM unit. The device-tree definition is 1 byte / 7 bits, but mainline `nvmem-reboot-mode` unconditionally writes 4 bytes, so the NVMEM layer returns `-EINVAL` directly.
- How to fix: Write using the actual width of the unit. Also confirm that `NVMEM_REBOOT_MODE` and `NVMEM_SPMI_SDAM` are built in.
- Prevention: Before declaring that "the bootloader does not support it", first read the generic driver source and the definition of that unit in the device tree.
- Applicability: Qualcomm; 2026 (upstream may already have fixed this; check first).

### With external power connected, the device repeatedly powers on automatically and enters charging mode

- Symptoms: After shutdown, the device repeatedly starts again, or enters your system with `androidboot.mode=charger`.
- Cause: The bootloader automatically powers on when external power is detected.
- How to fix: Make the first initramfs branch recognise charging mode and enter a read-only, low-power wait state; continue only after a long press of the power button. Do not use shutdown as the response (with power still connected, the device will start again).
- Applicability: General.

### Early logs are unreliable

- Symptoms: pstore contents are corrupted or empty; kernel log level has changed; PID 1 exits unexpectedly.
- Cause: The ramoops address is not a firmware-reserved safe region; ABL appends `loglevel=6`; `console=ttynull` causes some init processes to exit; `console=tty0` unregisters bootconsole before initcall; bare `earlycon` does not work under some configurations.
- How to fix: Use the fixed reserved region in the stock device tree for ramoops; if none exists, use runtime netconsole. Compare console parameters in the test environment and on the real device one by one.
- Applicability: General.

## Unresolved

- How to recover unattended from an early hard hang: current approaches depend on a timed reboot or a person pressing a button.
- The exact mechanism behind Fastboot becoming unresponsive after a long idle period.
