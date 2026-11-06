# USB, Type-C, External Display, Storage, Partitioning, Dual Boot, Flashing and Installation

## Recommended Route

**USB:** Implement device mode first (as the debugging channel; USB 2 high-speed NCM is the most reliable), then host mode and external display. Enable in this order: controller role switching → UCSI → VBUS → enumeration → USB 3 / DisplayPort.

**Storage and partitioning:** Build the storage controller and PHY into the kernel. Locate partitions exclusively by PARTLABEL/GUID. Allocate only the tail of userdata to Linux (root partition + home partition).

**Dual boot:** Rotate the active-slot image set. Both systems boot from the currently active slot. When switching, write the target system's boot-image set into the active slot; keep the other system in the Linux partition. Switch from within the system using `dd`, and from Android using a KernelSU module. Mark the slot successful after boot.

**Root filesystem delivery:** Format on the device and extract a tar archive (preserving extended attributes and numeric ownership). If an image must be distributed, encode only used blocks.

**Maintenance image:** A small image that runs only in memory, includes storage drivers, a root shell and partitioning tools, mounts no partitions, and automatically returns to Fastboot on timeout without requiring a session. Use it to write the partition table and preload device-local files before first boot.

## Completion Criteria

- **USB:** USB storage read/write, mouse, recovery after unplug/replug, evidence of USB 3 speed, external monitor output (EDID, mode, recovery after disconnect), and restoration of the debugging channel after switching.
- **Partitioning:** read-back matches after writing, and `sgdisk --verify` passes.
- **Dual boot:** Linux → Android → Linux each completes once on real hardware, followed by a cold boot; compare slot attributes before and after.
- **Installation:** perform the complete procedure on real hardware from a clean start, following the public documentation, including GNOME initial setup.

## Pitfalls

### Sparse Images Encode Empty Regions as "Write Zero", Making Flashing Extremely Slow and Fastboot Unresponsive

Tags: **[Frequent] [Fatal]**

- **Symptoms:** `fastboot flash` quickly returns OKAY, but subsequent commands queue for tens of minutes or the device stops responding; a single partition takes minutes to hours to "flash".
- **Cause:** empty regions in a sparse image are encoded as FILL (write zero). The bootloader returns OKAY first, then slowly writes zeros in the background (for example, tens of MB/s; several hundred GiB can take hours). Fastboot OKAY does not mean writing has finished.
- **Confirmation:** before flashing, total the expanded byte counts for RAW, FILL and DONT_CARE.
- **Resolution:** deliver the root filesystem as tar and run `mkfs.ext4` on the device; format empty data partitions on the device. If an image must be distributed, use the ext4 bitmap to encode only used blocks.
- **Prevention:** inspect installers from comparable projects before designing the installation procedure.
- **Applicability:** General; 2026.

### DONT_CARE in a Sparse Image Does Not Clear Old Data and May Replay Old Journal Records

Tag: **[Fatal]**

- **Symptom:** the filesystem behaves abnormally after flashing over a partition containing old data.
- **Cause:** `img2simg -s` marks empty blocks to be skipped, leaving old contents on the device. If the filesystem UUID is fixed (for example, generated from the release name), journal blocks from a previous installation of the same version may still pass checks; crash recovery can replay the old journal into the new filesystem. Zero-filled blocks inside files, inode tables and the journal area must be written.
- **Resolution:** skip only free blocks identified by the bitmap; still write inode tables and the journal area. Alternatively, format on the device.
- **Applicability:** General.

### Changing Only GPT Attribute Bits to Switch A/B Slots Leaves Both Slots Unbootable

Tags: **[Fatal] [Time-consuming]**

- **Symptoms:** after selecting "reboot to the other system", the device enters Fastboot after about two seconds. Flashing the original factory image, using `fastboot boot` with a known-good image, and even a 4 KiB all-zero file then produce the same error.
- **Cause:** bootloader slot state includes more than boot-partition attribute bits. It also includes swapped type GUIDs for each pair of `_a/_b` partitions across all LUNs and the boot LUN. Changing only the attributes causes the bootloader's GUID validation to reject the state.
- **Confirmation:** first read the bootloader's recorded failure reason (for example, `fastboot oem fbreason`), and collect `getvar all` and GUID information.
- **Resolution:** restore the GPT so the bootloader can rebuild slot state, then use its own `--set-active` operation. For dual boot, use active-slot rotation.
- **Prevention:** change A/B state only through the bootloader's own commands. If valid and invalid images fail in the same way, immediately stop pursuing image-content hypotheses. Until the mechanism is understood, do not use state-changing commands for diagnosis, including `--set-active` under the label of "rollback".
- **Applicability:** Qualcomm ABL; 2026.

### Slot B or the Recovery Partition Cannot Be Used to Host Linux

- **Symptoms:** booting slot B enters an old firmware chain or fails; recovery rejects an image containing a kernel.
- **Cause:** slot B may contain an old factory firmware chain or be all zeros; the super partition can hold only one Android installation; recovery is treated only as a ramdisk.
- **Confirmation:** compare hashes of the partitions in both slots first.
- **Applicability:** Qualcomm.

### Host-side Fastboot Slot Writes Reset the Retry Count; a Custom System That Does Not Mark Success Becomes Unbootable

- **Symptom:** after several boots, the slot is marked unbootable.
- **Cause:** host-side `fastboot flash` resets the retry count to 7; Linux does not mark the boot successful as Android does.
- **Resolution:** write using `dd` from within the system (preserving attributes); use a service to mark the slot successful after boot.
- **Applicability:** Qualcomm A/B.

### Android Fails on Its First Boot After Dual-Boot Installation

- **Symptom:** switching back to Android enters recovery or shows a black screen.
- **Cause:** firmware partitions required by Android (modem, Bluetooth, DSP) are damaged, while the installer checked only the boot chain.
- **Resolution:** have the installer verify all firmware partitions required by Android; use kernel logs from the rawdump as evidence.
- **Applicability:** Qualcomm.

### The Slot-Switching Tool Deletes the Image Set Currently in Use When a Slot Write Fails

- **Cause:** the rotation logic does not first establish which image set is actually in the current active slot.
- **Resolution:** verify hashes of the images actually present in the slot before deciding what to rotate. After writing, read back and compare; if they differ, restore the original images and do not reboot. Ignore interruption signals while writing.
- **Applicability:** Active-slot rotation; 2026.

### The "Preserve Home" Option Actually Formats Home

Tag: **[Fatal]**

- **Cause:** the host accepts the parameter, but the device-side script does not receive it and formats unconditionally.
- **Resolution:** before formatting, verify the volume label and run `e2fsck`; use an end-to-end test to prove that the device side really leaves the partition unformatted.
- **Applicability:** General.

### Device Names Can Change

- **Symptom:** the same device appears under different `sdX` names on different boots; mainline controller names also differ from Android.
- **Resolution:** interpret backup filenames only against the inventory recorded at the time. Before restoring, re-identify using the controller, LUN, GPT GUID, label and geometry; never write back based on the device name alone.
- **Applicability:** UFS; General.

### Backup GPT Locations Are Hard-Coded and No Longer Match After the Bootloader Rewrites Them

- **Resolution:** read the locations of both primary and backup GPTs from their headers.
- **Applicability:** General.

### Backups Are Incomplete or Inconsistent

- **Causes:** GPT table regions contain padding; FAT partitions cannot be frozen; raw blocks cannot be read without root; reads from active partitions are not consistent snapshots.
- **Resolution:** use a temporary rooted recovery or obtain root before backing up; calculate hashes on both host and device. State clearly that the backup represents a "per-partition state", not an atomic snapshot.
- **Applicability:** General.

### Device-Local Data Is Missing After Flashing, Leaving Touch and Bluetooth Unavailable at First Boot

- **Cause:** device-unique data such as calibration and addresses must not be included in public images.
- **Resolution:** add a "preload device-local files before first boot" step to the installation procedure, using the maintenance image to write them. Document the differences before and after first boot.
- **Applicability:** General.

### The HTTP Service in the Maintenance Image Does Not Start or Its Port Conflicts

- **Causes:** the service starts before the USB network link is ready; a commonly used host port is already occupied by another process.
- **Resolution:** start the service only after the link is ready; make the port configurable.
- **Applicability:** General.

### Tools Are Missing or Behave Differently in the Installation Environment

- **Symptoms:** `/dev/disk` is absent; some coreutils implementations reject unregistered UIDs; `install /dev/stdin` fails; BusyBox tar does not preserve extended attributes.
- **Resolution:** run a small-sample test in the real device's in-memory environment first (formatting, verification and ownership changes). Use GNU tar. If the same tool causes a second problem, switch tools.
- **Applicability:** General.

### USB Network Transfer Stalls Partway Through a Large File

- **Resolution:** transfer in chunks (for example, 64 MiB each); have the server read from the same verified file by offset. Stop immediately if any chunk fails verification.
- **Applicability:** NCM; 2026.

### An External USB Network Adapter Takes the Debugging Interface Name

- **Cause:** the external adapter is also named `usb0`.
- **Resolution:** fix the interface name and MAC in configfs, and identify it with a `.link` file.
- **Applicability:** General.

### A UCSI Reset Command Storm Leaves the ADSP Unresponsive

- **Symptom:** a UCSI reset occasionally times out, after which the ADSP does not respond for the remainder of the boot.
- **Cause:** after `EPROBE_DEFER`, the reset command is resent every 20 ms, hundreds of times.
- **Resolution:** send the reset command only once.
- **Applicability:** Qualcomm UCSI; 2026.

### USB Host Mode Is Unstable or Freezes the Entire Device

- **Causes:** the USB 3 PHY parameter table does not match the device generation (the vendor's complete table must be ported); the `maximum-speed` property causes an out-of-range condition and is rewritten by the bootloader; `allow_userspace_control` is not set; the pipe clock is enabled too early.
- **Resolution:** address each item above. During driver development, build drivers that could hang the bus as modules.
- **Applicability:** Qualcomm DWC3 + QMP.

### A Single Type-C Port Disconnects the Debugging Channel When Switched to Host Mode

- **Resolution:** before switching, configure a timed rollback on the device and read back to confirm that it is active.
- **Applicability:** General.

### External Display Link Issues

- The switch-chip name in the device tree does not match the actual chip (use the I2C ID read-back as evidence); an interface expects a bitmask but is given a pin number; HPD is reported connected even when direction or switching is wrong; the SBU switch-chip driver must be built into the kernel; external display usually requires the internal panel to have already been taken over by the native driver.
- UCSI's reported power state is a logical state and does not prove physical power delivery.
- **Applicability:** Qualcomm DP AltMode; 2026.

### The Driver Writes Device Attributes Even When the Disk Is Only Being Identified Read-Only

- **Cause:** the UFS driver sets some device attributes when it loads.
- **Resolution:** account for this behaviour and, where necessary, confirm the original values on the factory system.
- **Applicability:** UFS; 2026.

### With a Single Type-C Port, Prepare a Second Channel Before Testing Host Mode

- **Procedure:** before testing OTG, VBUS or peripherals, establish a backup Wi-Fi SSH channel on the device and verify its identity; configure a timed rollback. Group cable changes and peripheral connections into a single physical intervention.
- **Applicability:** General; 2026.
