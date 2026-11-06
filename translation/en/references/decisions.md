# Key Technical Decisions and Best Practices

For each decision point, record the **default approach**, a set of **rules** (what to do under particular conditions), **why** (the underlying mechanism, so the rule can be reassessed for a new device), **when it does not apply**, and, where needed, **acceptance criteria**.

These rules are abstractions from actual choices and outcomes across multiple projects. When conditions differ, reassess using the criteria in Section 1 and record the reasoning.

## Contents

1. General decision criteria
2. Preparation and boot: backup and root access, device-tree approach, built-in drivers versus modules, initramfs, boot images, early logs, debugging channel, candidate-kernel testing
3. Installation and dual boot: dual-boot approach, partition capacity, root-filesystem source and delivery, installer
4. Display and input: display approach, default refresh rate, screen-off approach, power lifecycle of peripherals and panel, touch algorithms, keyboard-driver form
5. Power: suspend method, power button and screen lock, charging, system time, watchdog
6. Wireless: Wi-Fi regulatory domain, power and firmware, Bluetooth address
7. Remote-processor peripherals: sensors, camera, hardware video decoding, speaker gain
8. Graphics and applications: GPU userspace, sandboxed applications and browser
9. Engineering: kernel baseline, community reuse, packaging and versions, firmware distribution, build environment, first-release threshold, release cadence, when to abandon an approach, UEFI boot layer

---

## 1. General Decision Criteria

Work through these questions for every decision:

1. **How close is it to upstream?** Already merged upstream > patch under review > community work for the same SoC > locally written. For local patches, state how they will be removed once merged upstream.
2. **Can it be rolled back if something goes wrong?** Prefer approaches that do not change bootloader state or touch critical vendor partitions and that can recover automatically after failure.
3. **Is there real-device evidence?** Something demonstrated to work on similar hardware is better than something that merely “claims support”. Someone else's conclusion is only a lead until verified on this device.
4. **Who maintains it?** Upstream- or distribution-maintained solutions are preferable to ones maintained locally.
5. **Can it be replaced?** The hardware layer must not be tied to a distribution, desktop, display manager or fixed account.
6. **Is it the minimal solution?** Do not introduce generic frameworks, permanently running “manager” services or duplicate resource management.
7. **What does the user have to take on?** Performance, power consumption, secure defaults, installation steps and data risk.
8. **Does the licence permit it?**
9. **Does a temporary solution have an exit condition?** A temporary approach may be used to get a result sooner, but work on the long-term solution must begin at the same time and the technical debt must be recorded.

For every important decision, record the options, trade-offs, choice, rationale and circumstances that would trigger reassessment.

---

## 2. Preparation and Boot

### Backup and Root Access

- **Default:** Obtain root access first (Magisk or KernelSU by modifying init_boot). Back up partitions individually within the scope confirmed by the user (usually excluding userdata and super). Save both primary and backup GPTs for every LUN separately. Verify that hashes match on the host and device.
- **Rules:**
  - If considering a minimal RAM probe on the stock kernel to read partitions, do not build one; rooting the stock system directly is faster.
  - If Android cannot or should not be rooted, temporarily boot a rooted recovery with fastboot boot, set block devices read-only, and read from there.
  - If an official OTA can be downloaded again and verified, it can serve as the recovery source for system partitions and reduce the amount that must be backed up. Device-specific data such as persist and calibration data must still be backed up separately.
  - If dual boot requires Android to write boot partitions, choose a root method that modifies only init_boot; re-patch it after Android updates.
- **Why:** There must be a recovery path before persistent writes. USB on the stock kernel often depends on clock controllers for the display, camera and other components being ready first. A minimal probe may therefore fail to obtain USB and provide no new information.
- **Does not apply when:** The vendor provides a recovery tool that can restore every partition without loss, including calibration data. In that case the backup scope can be reduced, but device-specific data must still be backed up.

### Device-Tree Approach

- **Default:** Use the upstream SoC dtsi plus a board-specific file written for the device. Every value must come from evidence collected from the device's stock system. Resources left active by firmware must be declared in the actual consumer nodes that use them.
- **Rules:**
  - If the goal is only a quick demonstration, the stock device tree plus an overlay and the parameters clk_ignore_unused pd_ignore_unused iommu.passthrough=1 may be used. Tell the user that this approach cannot support suspend, power saving or upstreaming.
  - If a fault is suspected to result from resource reclamation, temporarily add those three parameters as a diagnostic. If the problem disappears, that indicates a reclamation issue. Then remove the parameters and declare the resource requirements in the consumers.
  - If removing a debugging parameter causes a black screen (a “load-bearing parameter”), do not carry it into the product. Export clock and power-domain state before and after resource reclamation, then use binary isolation to identify the specific resource.
  - If a community project for the same model follows the stock-device-tree approach, reuse its drivers and userspace, but not its global keep-alive parameters.
- **Why:** Global keep-alive prevents the kernel from ever turning off resources left active by firmware and provides no IOMMU isolation. A stock device tree is written around downstream drivers, and its overlay may become incompatible after a stock-system update. Correct resource declarations need to be made once; suspend, power saving and upstreaming can then build on them.
- **Does not apply to:** One-off demonstrations or experimental devices.
- **Acceptance:** No global bypass parameters; no sync_state left pending indefinitely; the complete module set remains stable across multiple cold boots.

### Built-in Drivers or Modules

- **Default:** Compile drivers required for boot, recovery and the debugging channel into the kernel or include them fully in the initramfs. Do the same for resource providers required by the borrowed firmware display and drivers in the display-component dependency chain. Include the power-button driver. Use upstream defaults for other drivers as modules, and publish the complete module directory.
- **Rules:**
  - If the early image does not yet contain a module directory, set all boot and recovery dependencies to =y, and establish delivery of a complete, versioned module directory as early as possible.
  - When borrowing the firmware display, resource providers such as display clock controllers must be built in and bound before late_initcall.
  - For a newly tested driver (especially a clock controller or multimedia driver), build it as a module and load it manually after the debugging channel is established. Do not compile several new drivers into the kernel at once.
  - If a driver is in the dependency chain of a component framework (for example, a Type-C switching chip required by display), build it in; otherwise the entire chain waits.
  - If reducing the number of modules, prune by dependency closure, not by the output of lsmod. Run full-scenario regression tests after pruning.
  - If a feature is silently missing, first inspect the final .config, whether the module directory was delivered, and devices_deferred; only then suspect the device tree. Note that A depends on B || !B can also force A to be a module.
- **Why:** Without a module directory, =m is equivalent to not existing, with no error. If a module binds after the resource-reclamation point, the resource has already been turned off. An untested driver may hang a bus early in boot; as a module, a hang affects only one process.
- **Does not apply when:** The distribution's generic kernel already supports the device; follow the distribution's module policy.

### initramfs

- **Default:** Use a custom minimal BusyBox initramfs for first boot and the RAM phase. For the product, use the distribution's generator (Ubuntu uses dracut), set hostonly="no", and implement device-specific early-boot logic as generator modules.
- **Rules:**
  - If early-boot logic is substantial (custom recovery or complex root switching), a custom initramfs may be retained throughout, accepting the maintenance burden.
  - If a custom initramfs verifies the root filesystem, verify only files belonging to the project and provide a process for repackaging after changes. Otherwise distribution updates or on-device hot fixes may cause boot to be rejected.
  - If the distribution generator is slow on the build host, generate the initramfs on the device instead.
  - If the initramfs approaches the available space in the boot partition, check the module and firmware manifests. Do not rely on increasing compression; some packagers ignore that parameter.
  - If package configuration is stuck at half-configured, first inspect the distribution's boot deployment tool (on Debian-family systems, flash-kernel) and the project's hooks.
- **Why:** The distribution generator tracks system updates and has a lower long-term maintenance cost; a custom version offers the greatest control in the early phase.
- **Does not apply when:** The distribution already supports the device's boot method; use its complete workflow directly.

### Boot Image Format

- **Default:** For RAM testing, use header v2 (kernel + ramdisk + DTB). For persistent boot, generate images following the stock layout (GKI devices commonly use the three-part boot/init_boot/vendor_boot arrangement) and sign the AVB footer with a project key. When packaging, check device-tree identity (msm-id/board-id), command-line length and image size.
- **Rules:**
  - For a new device, use timestamped probes to test separately which formats are accepted by fastboot boot and by cold boot. Results from other devices with the same SoC are reference material only.
  - If fastboot boot returns OKAY but the device remains in Fastboot and pstore is empty, first compare device-tree identity and versions that previously booted.
  - If a cold boot goes directly to Fastboot, first inspect the AVB footer required by the vbmeta chain.
  - If an older-generation bootloader accepts only a single boot image, dual boot should rotate only the boot partition.
- **Why:** Before jumping to the kernel, the bootloader selects and verifies images using device-tree identity. If this fails, there is no evidence on the kernel side. Different generations of ABL deliver the ramdisk differently.
- **Does not apply to:** Platforms using other bootloaders such as U-Boot or LK; establish the rules for those platforms independently.

### Early Logs

- **Default:** If the stock device tree contains a fixed memory region reserved by firmware, use pstore/ramoops. Otherwise use runtime netconsole, with on-screen earlycon as a supplement.
- **Rules:**
  - If the reserved region is dynamically allocated, do not guess its address; use netconsole instead.
  - If the kernel hangs silently after handoff, find a way to display the crash on screen (for example, earlycon using the borrowed firmware display); this can often identify the cause directly.
  - If pstore contents are corrupted across reboots, do not treat pstore as the only evidence; use a live channel as well.
- **Why:** Writing ramoops to an unsafe address can corrupt data or allow firmware to overwrite it.

### Debugging Channel

- **Default:** During the RAM phase, use USB NCM + telnet/SSH + timeout fallback. In the product, use socket-activated SSH, bound to a fixed USB-subnet address with FreeBind=yes; use IPAddressAllow/IPAddressDeny to accept only the USB subnet. Disabled by default, explicitly enabled when needed, and persistent across reboot.
- **Rules:**
  - To make SSH accept connections only over the USB network interface, use IP filtering, not BindToDevice.
  - If debugging packages or channel-related services change, verify with a cold boot and test the upgrade path from an older version (the enablement symlink may not be recreated during upgrade).
  - Before the first flash to a test device, enable the channel and confirm that a recovery path exists.
  - If there is only one Type-C port and USB host mode must be tested, first establish and verify a backup Wi-Fi SSH channel.
  - If the host does not have serial-port permissions, do not use USB serial (ACM) as the primary channel.
- **Why:** The debugging channel is the prerequisite for unattended work. A socket bound to a network interface that does not yet exist at boot can create a boot dependency deadlock. Without a channel in the production image, any problem requires the user to press buttons.
- **Acceptance:** Verify cold boots with the channel both enabled and disabled; when disabled, it must genuinely be unreachable.

### Candidate-Kernel Testing

- **Default:** Verify by booting in RAM first. Only after it passes should the normal upgrade path be used to install it persistently.
- **Rules:**
  - To test a candidate kernel on an installed system, unpack modules and run depmod on the device; generate the initramfs on the device without writing to a slot; build a v2 RAM image; add softlockup_panic=1 hung_task_panic=1 oops=panic and panic=N to the command line; schedule an automatic reboot immediately after boot; remove staged files after testing.
  - Do not install a candidate kernel package directly. Its hooks write to the active slot, and a faulty candidate may put the device into a reboot loop.
  - Do not unload or rebind a real hardware driver while the device is unattended to replace a driver module at runtime. Stage the new module before reboot and verify it by booting the new image in RAM.
  - Before rebooting, check for tasks in D state. If any exist, use SysRq (s, u, b) to reboot.
- **Why:** A driver-unload path may deadlock. When shutdown hangs, the debugging channel has already been disconnected, leaving the device waiting for someone to press a button.

---

## 3. Installation and Dual Boot

### Dual-Boot Approach

- **Default:** Rotate the active-slot image set. Both systems boot from the active slot. To switch, write the target system's complete image set to the partitions and read it back; do not change slot attributes, touch the other slot or modify vbmeta. Use dd from within the system. Mark the slot successful at boot. Use a KernelSU module on the Android side to switch. Retain the previous Linux image set for rollback.
- **Rules:**
  - If considering putting Linux in slot B, first compare hashes of every partition in both slots. Slot B is often an old firmware chain or empty. Even if usable, do not write slot attributes manually.
  - If considering putting Linux in the recovery partition, first confirm whether the bootloader treats it only as a ramdisk (as is often the case).
  - If neither slot boots, first read the bootloader's recorded failure reason, getvar all and GUID information. Do not experiment with --set-active; recover using the GPT so the bootloader can rebuild slot state.
  - A single-system first implementation is acceptable (slot B remains the stock fallback), but decide the dual-boot design early to avoid later migration.
  - If the host has used fastboot flash to write a boot partition, retry counts may have been reset; a service that marks the slot successful at boot may be needed.
  - If the user wants a boot menu, UEFI or U-Boot is a possible later direction; see Section 9.
- **Why:** A/B slot state is maintained by private bootloader logic and may include swapping partition-type GUIDs and boot LUNs. Changing only attribute bits may be rejected by its validation.
- **Acceptance:** On a real device, complete Linux → Android → Linux once, including a cold boot; compare slot attributes before and after; verify that all firmware partitions required by Android remain intact.

### Partition Capacity

- **Default:** Take space for Linux only from the end of userdata. Use a 60 GiB Linux root (actual system use is approximately 8–15 GB). Put home at the end so it can be expanded easily. Give the user two to four concrete layouts to choose from.
- **Rules:**
  - If the user may reinstall Linux later but wants to keep data, separate root and home.
  - If super is a dynamic partition and Linux cannot read Android's encrypted data, do not take space from either of these.

### Root-Filesystem Source and Delivery

- **Default:** Use the SquashFS layer from the distribution's official ARM64 image as the source, with a fixed URL and hash; package device-specific components. Deliver by running mkfs.ext4 and extracting a tar archive on the device; format the empty partition on the device.
- **Rules:**
  - If assembling the root filesystem manually, switch to the official layer plus packages; manual assembly is likely to omit firmware directories or configuration files.
  - If an image must be distributed, encode only used blocks and calculate the expanded byte count of RAW/FILL/DONT_CARE before flashing.
  - If the target partition contains old data, do not use a sparse image that “skips empty blocks”; old data and logs will remain.
  - Do not layer each release over the old root filesystem; it cannot be reconstructed reliably.
- **Why:** Zero-filled blocks in a sparse image are written slowly in the background by the bootloader. Hundreds of GiB can take hours, during which Fastboot does not respond. Skipped blocks do not clear old contents.

### Installer

- **Default:** Provide an installation method with safety gates in the first release. At minimum, provide a maintenance image (runs only in memory, includes partitioning tools and a root shell, and has no session timeout that returns to Fastboot). Steps: verify device identity → check battery → back up → confirm layout → write and read back → format and extract → pre-populate device-specific data before first boot → write boot images and read them back.
- **Rules:**
  - For features that require device-specific data on first boot (touch calibration, Bluetooth address), pre-populate that data using the maintenance image before first boot.
  - If there is a “preserve data” option, test end to end that the device-side partition is genuinely not formatted.
  - For dual-boot installation, verify every firmware partition Android depends on, not just the boot chain.
  - Test installation-environment tools first on a small sample in the real-device RAM environment. If the same tool causes a problem a second time, replace it.
- **Why:** A manual Fastboot command has no protection if the wrong partition name is entered; users do not have your internal scripts.

---

## 4. Display and Input

### Display Approach

- **Default:** If an upstream display driver supports the SoC and a panel driver is available, take over natively. Otherwise, first borrow the firmware display using simpledrm, while **starting native takeover at the same time**.
- **Rules:**
  - After borrowing the firmware display, do not add product fixes for brightness, screen-off, refresh rate or suspend on top of it; all of these depend on native takeover.
  - Before writing a panel driver, compare the vendor display-driver source and stock-device debugfs snapshots item by item, then proceed to black-box parameter tuning.
  - If a community native implementation exists, import it as a complete set. Check panel commands against the device's stock implementation one by one; take board-specific values from this device.
  - Native-takeover sequence: golden snapshot → panel driver → manually load the display driver as a module → add SMMU passthrough to the display subsystem → take over at the firmware's current refresh rate → screen off/on → other refresh rates (test touch at each rate) → additional features such as night light.
  - If the display is corrupted despite no reported errors, inspect pipeline topology and DSC parameters first; do not perform repeated rounds of single-variable tuning.
- **Why:** simpledrm owns only a block of memory; it does not control the hardware, and GNOME does not provide internal-display functionality through it. Borrowing the display also requires specific protection against resource reclamation. Most of the cost of native takeover lies in checking parameters once; vendor source can shorten this substantially.
- **Acceptance:** Visual inspection by a person plus hardware metrics (frame count, underflows and CRC); dozens of DPMS cycles; multiple cold boots within the time budget.

### Default Refresh Rate

- **Default:** At takeover, use the refresh rate currently set by firmware. Expose other modes only after native display is stable.
- **Rules:**
  - When exposing a new mode, verify touch and DPMS at that rate as well.
  - If a mode is rejected by mode validation because the display-core clock is slightly above its limit, first confirm the actual requirement after DSC compression. Do not raise the clock beyond the OPP table.
  - If a stylus is present, assess its refresh-rate requirements before choosing the default.
- **Why:** Fewer variables make takeover failures easier to attribute. The touch firmware's scan timing is related to display refresh rate.

### Screen-Off Approach

- **Default:** Use genuine DPMS screen-off in the native display driver.
- **Rules:** Before native takeover is complete, turning off only the backlight may be used as a transitional measure, but register it as technical debt. Note that it often requires adaptation to a particular desktop version and consumes more power.
- **Why:** Turning off only the backlight leaves the display pipeline running.

### Power Lifecycle of Peripherals and Panel (Touch, Keyboard and Accessories)

- **Default:** Follow standard callbacks (drm_panel_follower, system PM prepare/complete, suspend/resume) and reproduce the power-state sequence used by the stock driver. Do not wait synchronously inside callbacks. If hardware readiness must be awaited, schedule asynchronous work and retain a time budget.
- **Rules:**
  - If a peripheral stops working after the screen turns off, check whether its firmware is downloaded again after the panel reset.
  - If screen-on becomes slower or a peripheral fails after changing refresh rate, check for synchronous waits in panel callbacks.
  - If a peripheral fails after suspend, compare the stock driver's screen-off, screen-on and suspend sequences to find the missing step.
  - If an accessory connected at boot is not recognised, collect evidence separately for “connected at boot” and “connected after boot” and find the earliest failing layer.
  - If a reference implementation uses periodic keep-alive, automatic reset or discards the first interrupt, do not copy those behaviours blindly; inspect the stock lifecycle first.
  - Restarting a service automatically to recover is only a temporary workaround and must be registered as technical debt, not treated as a fix.
- **Why:** A TDDI panel reset can cause touch firmware to be lost. Synchronous waits in callbacks block display commits. Keep-alive and automatic-reset mechanisms can conceal lifecycle errors.
- **Acceptance:** Test real input separately after cold boot, screen off/on, suspend/resume and unplug/replug. Test every refresh rate.

### Touch Algorithm (Raw-Data THP)

- **Default:** Reuse an open-source userspace algorithm implementation. Write only the transport layer in the kernel (firmware download and data frames). Extract device-specific calibration from the stock system and do not put it in the public repository.
- **Rules:** Do not try to run Android's touch HAL directly on Linux; it requires Bionic and a Binder compatibility layer and has a high maintenance cost. Writing a new algorithm from scratch is likewise not worthwhile.

### Keyboard-Accessory Driver Form

- **Default:** Extract the protocol from the vendor driver and implement a transport driver for a standard subsystem (an I2C bridge as an HID transport, or single-wire UART as serdev). Keep the protocol core separate and cover it with unit tests. Follow the stock power lifecycle.
- **Rules:**
  - If the reference implementation is for an accessory on another device, first confirm its protocol and power model (active heartbeat or event-driven). If they differ, reuse only protocol parsing, not keep-alive or recovery strategies.
  - If enumeration succeeds but no keys arrive, inspect the electrical layer first (always-on power and bus levels), then the protocol.
- **Why:** Accessories in the same product category can use completely different protocols on different devices.

---

## 5. Power

### Suspend Method

- **Default:** s2idle (suspend-to-idle): kernel mem_sleep_default=s2idle, systemd SuspendState=freeze.
- **Rules:**
  - If the display still borrows the firmware framebuffer or peripheral power management is incomplete, disable suspend first (sleep.conf.d and set the desktop idle action to “nothing”) and register the debt. Enable it only after completing this sequence: genuine screen-off → peripheral power management → wireless (do not turn off the power domain when PCIe link retention is required) → charging-control handoff → stop sensor streams and battery-gauge polling. Do not enable suspend first and fix components one by one afterward.
  - If logs suggest the device wakes after one or two seconds, verify using wall-clock time or BOOTTIME − MONOTONIC. Under s2idle, add together multiple Timekeeping suspended intervals. Find the source only after confirming that the wake is genuinely early.
  - Once early wake is confirmed, add temporary tracing at the IPCC and glink layers to find the source; stop sensor streams before suspend; test outside the notification period after boot. Do not mask IPCC.
  - If a peripheral does not work after wake, inspect the PM callbacks for that peripheral individually.
  - If the screen is black after wake, inspect the display screen-off/on path.
  - If the device takes more than ten seconds to suspend after the power button is pressed, inspect logind delay locks (ListInhibitors) and identify who holds the inhibitor up to the InhibitDelayMaxSec limit. Do not hide the problem by lowering the limit.
  - If s2idle power consumption is unacceptable and early-wake sources have all been identified, disconnect external power and compare deep suspend with s2idle power consumption; decide from the measurements.
- **Why:** On Qualcomm platforms, messages from remote processors over IPCC/glink can fully wake deep suspend without appearing in the wakeup record. Under s2idle, a non-wakeup interrupt wakes the CPU briefly and it returns to sleep. The monotonic clock does not advance during suspend, and one s2idle session can be split into many intervals.
- **Does not apply to:** Platforms without these remote-processor messages and with mature deep-suspend support; choose based on power measurements.
- **How to test:** First use pm_test incrementally (freezer → devices → platform → processors → core), then perform real suspend. Automate with RTC wakeup. For an automatic recovery timer, use OnCalendar + WakeSystem=yes (OnBootSec does not count suspended time). Test physical wake sources (power button, keyboard and opening the cover) separately.
- **Acceptance:** Successful-suspend count increases; wall-clock duration is correct; boot ID is unchanged; display, touch, keyboard, Wi-Fi, Bluetooth and audio all work after wake; at least 10 consecutive cycles.

### Power Button and Screen Lock

- **Default:** The power button has exactly one handler (choose either logind or the desktop and set the other to ignore it). Before suspend is complete, a short press does nothing or retains the shutdown dialog. After suspend support is complete, a short press locks the screen and suspends.
- **Rules:**
  - If a short press of the power button shuts the device down directly, first check whether the system has identified the device as a virtual machine.
  - If the power-button option is absent from Settings, GNOME may be hiding it for a tablet chassis. Setting Chassis to convertible may expose it, but check side effects on suspend delay and Bluetooth device class.
  - If power-button behaviour is needed that the desktop does not provide (for example, lock only), apply a minimal patch to the distribution source package and register an exit condition. Do not fork a core component or add a permanently running polling service.
- **Why:** Two handlers responding to the same power-button event conflict. The desktop's chassis classification can change many defaults.

### Charging

- **Default:** Read-only first (battery gauge, charging state, input voltage and current). For writes, use a whitelist and limits; refuse writes for unknown power sources. Hand control back before suspend. Reuse a community implementation for proprietary fast charging.
- **Rules:**
  - If the generic battmgr reports incorrect values on a new SoC, first inspect the data layout and current-sign quirk. If the protocol is still incompatible, write a read-only client for the vendor-private channel.
  - If the original fast charger draws very little current at 5 V, this indicates that authentication and high-voltage negotiation are required. Reuse a community implementation; do not keep experimenting at 5 V.
  - Before writing charging parameters, use a whitelist and limits and refuse unknown sources. Return to a conservative state before suspend; if handoff fails, refuse suspend.
  - To wait for a target battery level before testing, use a wall charger and read updated values on a schedule. Slow charging from a computer USB port under Android may stop automatically after several hours.
  - If net charging is unavailable under Linux, document this clearly and set a battery threshold before testing (for example, at least 40% before leaving Android).
- **Why:** Fast-charge negotiation often requires host participation. A status flag can be forced to a value without changing the actual current.
- **Acceptance:** Judge only the battery-level and voltage trends and current over 15–30 minutes, not status flags.

### System Time

- **Default:** Transitional approach: restore the last saved time at boot (fake-hwclock, a oneshot service with RemainAfterExit=yes) and use chrony. Root solution: store an offset in the PMIC's SDAM.
- **Rules:**
  - Before using an SDAM offset, verify read-only on the stock system that the target cell is not already occupied.
  - If time still does not synchronise after networking is available, check whether the router hijacks NTP or chrony requires NTS authentication. Where needed, add nocerttimecheck and authselectmode ignore, and explain the trade-off.
  - If considering enabling RTC writes, first perform a separate RAM experiment; if the bus rejects writes, revert the change.
- **Why:** The Qualcomm PMIC RTC is usually read-only from Linux; forced writes may be rejected by arbitration.

### Watchdog

- **Default:** First perform a controlled missed-feed test and confirm that the hardware watchdog really resets the device. Do not rely on it for recovery until verified.
- **Rules:** systemd enables the watchdog by default. Ensure the timeout does not exceed the hardware limit, or disable the runtime and reboot watchdogs.
- **Why:** A watchdog provided by a hypervisor may show an unchanged counter while being fed, and may not reset after missed feeds.

---

## 6. Wireless

### Wi-Fi Regulatory Domain, Power and Firmware

- **Default:** Fix the country code to match the stock system's default and do not change it at runtime. Apply the SAR power limit at boot as the stock system does. Use the complete firmware and board-data set from the same stock version.
- **Rules:**
  - If no 5 GHz networks can be found, inspect the driver's band-range calculation (some country rules remove 5 GHz) and board matching logs.
  - Before experimenting with only a replacement board file, confirm whether the driver prefers board-2.bin. Replace firmware as a complete version-matched set.
  - If connection succeeds but drops after a few seconds, inspect whether transmit power exceeds the stock limit. Compare the commands sent by the stock driver at startup before changing firmware.
  - Do not automatically change the country code based on timezone or user location. Changing it at runtime may cause SMMU faults, firmware crashes and driver deadlocks, with little benefit.
  - Do not reload the driver during the same boot; reboot before testing again.
- **Why:** The country code affects bands, power and firmware state. The runtime-change path is among the least tested in the mainline driver. The stock implementation avoids high-power problems by applying a SAR limit.
- **Acceptance:** Connect at 5 GHz / 80 MHz and remain connected; DHCP, ping and throughput work; connectivity recovers after suspend/resume.

### Bluetooth

- **Default:** Read the address from device-specific data and write it through MGMT on first boot; do not hard-code it in the device tree. Capture btsnoop on both ends during testing.
- **Rules:**
  - A public image must not contain the device's own address. Write it at first boot using a helper program, and do not change it if already configured.
  - If a manual command succeeds but the service fails, compare execution environments (standard input/output, permissions and namespaces) before using strace.
  - If reconnection fails, capture packets at both ends and confirm that the test peer's kernel has no known Bluetooth defect.
- **Acceptance:** Pairing, transfer, audio or input, reconnection, cold boot and coexistence with Wi-Fi.

---

## 7. Remote-Processor Peripherals

### Sensors

- **Default:** First identify which remote processor hosts the sensors (SLPI or the ADSP's sensor_pd). Use the open-source hexagonrpc + libssc + iio-sensor-proxy build with SSC support. Import device-specific configuration read-only from Android; do not put it in the public repository.
- **Rules:**
  - Before borrowing a sensor approach from another device, confirm that the remote processor is the same. Patches may be reusable; the startup method may not be.
  - If the remote processor starts automatically before configuration can be read, change to a one-time userspace start after the configuration becomes readable.
  - Test each group on a fresh boot. When importing device-specific configuration, reject stale copies.
- **Why:** The sensor power domain reads its configuration only once each time the remote processor starts.

### Camera

- **Default:** First check whether an upstream or community camera subsystem exists for the SoC. If it does, import it as a complete set and use libcamera software ISP. If it does not, mark the camera unsupported.
- **Rules:**
  - If a test script can capture frames, continue validating the application entry path (camera service, PipeWire and application) before declaring it usable.
  - If GPU rendering has a defect with a particular pixel format, temporarily use another format as a workaround, register the technical debt, and switch back once the GPU is fixed.
  - If actuator or sensor voltage lacks device-specific evidence, do not enable that component or guess a voltage.
  - Images captured during testing are user-private data; delete them after use.

### Hardware Video Decoding

- **Default:** Use the upstream driver, device-specific signed firmware and a modular driver. Add missing GStreamer plugins from native APT packages.
- **Rules:** For acceptance, test every format with both fast and slow consumption, and compare frame by frame with software decoding. When borrowing platform data from a neighbouring SoC, compare it with the vendor's message sequence.

### Speaker Gain

- **Default:** Use conservative gain for the first sound. Decide product gain separately. Increasing gain without speaker protection risks damage; provide the value chain and let the user decide the risk.
- **Rules:**
  - If gain is in doubt, test the vendor default or safe upper limit directly; do not increase it a little at a time.
  - If comparison with the stock system reveals only one difference, test that difference first with a single-variable experiment. Do not defer it merely because it “looks like it affects only data”.
  - Accept UCM on a fresh system with no saved ALSA state.

---

## 8. Graphics and Applications

### GPU Userspace

- **Default:** If the distribution's Mesa recognises the GPU (using the complete chip ID), use the distribution version. Otherwise, build a fixed version into a separate prefix, enable it only after the GPU driver loads, and revert to the distribution version simply by uninstalling it.
- **Rules:**
  - If Mesa has a defect on a new GPU, first inspect upstream stable and main; backport the relevant commit and register the technical debt. If upstream has not fixed it, write a minimal correction and prepare it for submission.
  - If GTK video has been tested with only one renderer, test both GL and Vulkan.
- **Further details:** ../experience/gpu.md.

### Sandboxed Applications and Browser

- **Default:** Use the deb version of the browser. For GPU support in snap applications, use the provider based on the complete original mesa-2404 content, replacing only the Mesa libraries, and connect it automatically on first boot.
- **Rules:**
  - If the official image already pre-installs a snap, do not remove it during the build. Remove it natively only after first-boot seeding has completed.
  - If an application bundled as a snap has a problem, first run snap refresh to see whether upstream has already fixed it.
  - Put a time limit on snap- and AppArmor-related problems. Defer them if they are not required for the first release.
- **Further details:** ubuntu.md.

---

## 9. Engineering

### Kernel Baseline

- **Default:** Latest stable plus patches with clearly identified sources. Remove local implementations when upstream covers the feature.
- **Rules:**
  - If a community branch is ahead, select commits from it; do not use the entire community branch as the baseline, as migration costs will continue to accumulate.
  - If the LTS kernel lacks support for the SoC, use the latest stable kernel rather than forcing a backport.

### Community Reuse

- **Default:** Choose one primary tracking source for each component. First compare the foundations of the two approaches: device-tree strategy, resource management, init and installation method.
- **Rules:**
  - If the foundations are the same, switch directly to the community baseline.
  - If the foundations differ, import complete functional series (preserving author, source commit and licence). Continue to take board-specific values from this device and split local adaptation into small commits. Do not re-derive every line.
  - A community claim of “supported” counts as a pass only after differences and integration have been verified on this device.
  - Accept community contributions wherever possible and fix minor issues locally. Do not accept content unrelated to the project scope or carrying legal or ethical risks.

### Packaging and Versions

- **Default:** Split packages by layer (see ubuntu.md); increment the kernel ABI on every kernel package build; package version numbers must increase monotonically.
- **Rules:**
  - If a basic device feature depends on a particular desktop-component version, separate it; package desktop adaptation independently.
  - If a release version is lower than an internal version already installed on the test device, increase the version. Use suffixes such as ~dev for internal builds.
  - If a published tag needs correction, publish a new version; do not move the tag.

### Firmware Distribution

- **Default:** The user decides. If firmware is distributed with the project, include a per-file manifest (source, path and hash) and state: “Remove upon objection from the rights holder.” Device-unique data must never be included.
- **Rules:** If users are expected to extract firmware themselves, first verify that the extraction toolchain is complete and usable; otherwise the installation barrier is too high.

### Build Environment

- **Default:** See build-environment.md.
- **Rules:**
  - If a step takes more than five minutes, time its sub-steps first to identify the bottleneck before deciding what to optimise.
  - If building an ARM64 root filesystem on x86 using full-system emulation, switch to native ARM64 (for example, an ARM64 CI runner).
  - For routine iteration, install packages on the device rather than rebuilding the entire root filesystem.

### First-Release Threshold

- **Default:** Someone else must be able to build, install and use the result by following the documentation. The support table must list only features that have passed on a real device. Not every peripheral has to be complete.

### Release Cadence

- **Default:** Define each release's scope and acceptance method first → develop → test in a concentrated pass (put tasks requiring user participation last) → release → clean up → begin the next version.
- **Rules:** If candidate numbers keep increasing, branches remain unmerged or no release has happened for a long time, stop and define the scope for a release.

### When to Abandon an Approach

- **Default:** Abandon an approach when the same kind of failure occurs a second time without new information; when one experiment proves it cannot work; when maintenance cost clearly exceeds benefit; or when the user asks to stop. Record the abandonment and the conditions under which it applies.
- **Rules:**
  - Before declaring something “infeasible”, first read the relevant generic-driver source and confirm that the issue is not a specific parameter (for example, write width) being wrong.
  - The following approaches have proved uneconomical or unworkable across multiple projects. Do not repeat them unless new evidence appears: switching slots by changing attribute bits alone; installing Linux in slot B; using recovery as the installation target; building a RAM probe on the stock kernel; using USB serial as the primary channel; using a network block device as root; building a root filesystem in an unprivileged user namespace without root; removing pre-installed snaps during the build; changing the Wi-Fi country code at runtime; enabling PMIC RTC writes; compiling multimedia drivers into the kernel all at once; investigating snap/AppArmor indefinitely without a time limit.

### UEFI Boot Layer

- **Default:** An optional direction for a later phase; do not include it in the first release.
- **Rules:** When evaluating it, first establish four things: whether the firmware embeds vendor components that cannot be redistributed; whether it replaces boot (and will therefore be overwritten by Android updates); how slot-success marking is handled; and how hardware state (display, SMMU and clocks) differs when exiting UEFI from the state when exiting ABL. All conclusions previously based on the ABL handoff state must be retested. If the conclusion is unclear, perform only one read-only RAM validation.
