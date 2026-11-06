# Pass 1 — Checkpoint 09: USB, Type-C, External Display, Storage and Installation

## 1. Position and scope

- Traversal: **Pass 1 — initial structural and linguistic reconstruction**
- Previous checkpoint: [Pass 1 / Checkpoint 08](pass-1-checkpoint-08.md)
- Completed source: `experience/usb-and-storage.md`
- Source branch: `main`
- Source blob SHA: `1737f41450dcbe6ebd6880790a4b4e8c072bc9c7`
- English translation: `translation/en/experience/usb-and-storage.md`
- Translation review: `translation/reviews/experience/usb-and-storage.md.md`
- Chinese descriptive reconstruction: `reconstruction/zh/usb-and-storage.md`
- Next source: `experience/ubuntu-userspace.md`
- Technical validation: not performed

This checkpoint records movement within Pass 1. Its purpose remains initial structural and linguistic reconstruction, not technical validation.

## 2. Reconstructed document role and structure

The source covers USB device/host roles, Type-C and UCSI, external display, UFS, partitioning, dual boot, flashing, root-filesystem delivery and installation tooling. It has recommended-route, completion-criteria and pitfalls sections, but no dedicated unresolved section.

The recommended route has five operational groupings:
1. USB device mode as the initial debugging channel, followed by host mode and external display.
2. Built-in storage controller/PHY and PARTLABEL/GUID-based partition identity.
3. Dual boot by active-slot image-set rotation.
4. Root-filesystem delivery through on-device formatting and tar extraction, preserving extended attributes and numeric ownership.
5. An in-memory maintenance image that mounts no partitions and returns to Fastboot on timeout.

The absence of an unresolved section is a structural observation, not evidence that every issue is resolved.

## 3. Completion and evidence boundaries

- USB: storage read/write, mouse, unplug/replug recovery, USB 3 speed evidence, external display with EDID/mode, disconnect recovery and debugging-channel restoration.
- Partitioning: read-back agreement and `sgdisk --verify`.
- Dual boot: Linux → Android → Linux on real hardware, followed by cold boot and slot-attribute comparison.
- Installation: clean real-device procedure following public documentation, including GNOME initial setup.

These criteria go beyond command success and device-node presence, reaching real peripheral behaviour, read-back and user-facing installation.

## 4. Main operational distinctions

### USB and external display

The explicit enablement order is controller role switching → UCSI → VBUS → enumeration → USB 3 / DisplayPort.

Failure records distinguish NCM naming/transfer problems, UCSI reset storms, host-mode PHY/property/control/clock issues, single-port debugging-channel loss, external-display switch-chip and HPD/SBU issues, and logical UCSI power state versus physical power delivery.

### Storage, partitioning and backup

Partition identity is based on PARTLABEL/GUID rather than transient `sdX` names. Backup is described as a per-partition state, not an atomic snapshot. GPT padding, mutable FAT, root access and active-partition reads constrain completeness and consistency.

The UFS driver may write device attributes on load even during an operation described as read-only identification.

### Dual boot and flashing

Both systems boot from the current active slot. Switching writes the target boot-image set into that slot; the other system remains in the Linux partition. System-side switching uses `dd`; Android-side switching uses a KernelSU module; a service marks the slot successful after boot.

Slot state includes more than boot-partition attribute bits: paired partition type GUIDs across LUNs and the boot LUN also matter. Host Fastboot resets retry count; Android depends on firmware partitions beyond the boot chain; unsafe rotation can delete the image set currently in use.

Sparse-image semantics are distinct:
- FILL writes zeros, potentially slowly in the background after Fastboot has returned OKAY.
- DONT_CARE skips writes, leaving old device data in place and potentially allowing old journal contents to be replayed.

The source's recovery advice includes collecting bootloader failure evidence, avoiding state-changing diagnostics before understanding the mechanism, reading back writes, restoring original images on mismatch and withholding reboot.

### Installation and maintenance image

The preferred root-filesystem delivery is on-device formatting plus tar extraction. The maintenance image provides an in-memory environment with storage tools, no mounted partitions and timed Fastboot fallback. It is also used to write the partition table and preload device-local files.

The clean-install procedure must verify device-side effects, not merely host-side parameter acceptance. Tool availability and semantics in the actual in-memory environment must be tested.

## 5. Explicit cross-document relationships

The source body contains no Markdown links to other domain documents. The collection index lists this document for USB/Type-C/external display, UFS, partitioning, dual boot, flashing and installation.

`patterns.md` explicitly references this document in:
- **Pattern A:** bootloader-modified state, including slot state beyond attribute bits.
- **Pattern D:** residual state, including sparse-image skipped blocks retaining old data.
- **Pattern I:** recovery-path reliability, including Fastboot background writes and preserving a debugging channel.

## 6. Observer-reconstructed correspondences

- **Debug channel as an operational dependency:** device mode is established first; host-mode testing on a single-port device can remove the channel, motivating a second Wi-Fi SSH route and timed rollback.
- **Logical versus physical state:** Fastboot OKAY, UCSI power status, slot attributes and host-side installer parameters each provide only partial evidence of the underlying operation.
- **Identity versus enumeration name:** controller, LUN, GPT GUID, label and geometry are more stable identity evidence than transient device names.
- **Write semantics and residual data:** FILL writes zeros; DONT_CARE preserves skipped contents. Their consequences must not be conflated.
- **Transaction-like safeguards:** slot rotation uses identify → write → read back → restore on mismatch → do not reboot. This is an observer correspondence, not a source-defined transaction protocol.
- **Recovery preservation:** maintenance image, timed rollback, secondary SSH, no reboot on mismatch and restrictions on unattended driver unload all preserve recovery options.
- **Device-local versus distributable data:** calibration and addresses are excluded from public images and provisioned separately before first boot.
- **End-to-end evidence:** acceptance extends from command output and enumeration to read-back, peripheral behaviour, disconnect recovery, boot cycles and clean installation.

These correspondences do not establish a unified transaction, recovery or state-machine architecture in the source.

## 7. Underspecified boundaries

The source has no dedicated unresolved section. Remaining boundaries include:
- exact partition geometry and sizing policy;
- full implementation and verification semantics for active-slot rotation;
- numeric acceptance thresholds for USB 3 speed, NCM transfer and external-display mode;
- a complete external-display bring-up route;
- exact UFS attributes written during driver load and their effects;
- the explicitly non-atomic nature of per-partition backups;
- implementation and test evidence for maintenance-image timeout/Fastboot fallback;
- subsequent patch or upstream-fix status for individual pitfalls.

These are evidence boundaries, not inferred defects. They do not block the next Pass 1 document.

## 8. Bounded sufficiency for Pass 1

For initial structural and linguistic reconstruction, the recommended route, completion criteria, failure records, explicit Pattern A/D/I references and the main identity, write-semantics, recovery and installation boundaries have been recovered. The absence of a dedicated unresolved section is preserved as a structural fact.

Further investigation would require device partition maps, implementation details for slot rotation and maintenance-image recovery, measured USB/display acceptance data or author/upstream evidence. No technical claims have been validated.

## 9. Position change

Pass 1 remains the same declared traversal. The document position advances from `experience/sensors-camera-media.md` (Checkpoint 08) to `experience/usb-and-storage.md` (Checkpoint 09).

The next source in the experience-library index is `experience/ubuntu-userspace.md`.

Pass number, document position and session checkpoint number remain separate coordinates.

## 10. Re-entry sequence

1. Read `reconstruction/zh/continuation/current-position.md`.
2. Read this checkpoint, then Checkpoint 08 and earlier checkpoints as needed.
3. Read `reconstruction/zh/README.md` and `translation/INVENTORY.md`.
4. Revisit `patterns.md`, especially Patterns A, D and I, where their explicit references are relevant.
5. Independently inspect `experience/ubuntu-userspace.md`; distinguish its own claims from expectations formed by previous documents.
