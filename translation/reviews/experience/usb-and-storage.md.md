# Translation Review: USB, Type-C, External Display, Storage and Installation

## Source and scope

- Source: `experience/usb-and-storage.md`
- Source branch: `main`
- Source blob SHA: `1737f41450dcbe6ebd6880790a4b4e8c072bc9c7`
- Translation: `translation/en/experience/usb-and-storage.md`
- Traversal: Pass 1 — initial structural and linguistic reconstruction
- Review scope: recommended routes, completion criteria, failure records, terminology, explicit pattern relationships and evidence boundaries
- Technical validation: not performed

## Document role and structure

The source is a broad experience record covering USB device/host roles, Type-C and UCSI, external display, UFS, partitioning, dual boot, flashing, root-filesystem delivery and installation tooling. It retains the experience-library structure of recommended route, completion criteria and pitfalls. Unlike several preceding domain records, it has no explicit `Unresolved` section. This is a structural observation only; it does not mean that every topic is complete or that no questions remain.

The recommended route is organized around five operational groupings:
1. USB device mode as the initial debugging channel, followed by host mode and external display.
2. Storage and partition identification using PARTLABEL/GUID, with the storage controller and PHY built in.
3. Dual boot through active-slot image-set rotation rather than treating the two Android A/B slots as independent Linux/Android installations.
4. Root-filesystem delivery by on-device formatting and tar extraction, preserving extended attributes and numeric ownership.
5. A minimal in-memory maintenance image that mounts no partitions and provides a controlled Fastboot fallback.

The completion criteria are user- and device-facing: peripheral read/write and recovery, USB 3 evidence, external display and disconnect recovery, restoration of the debug channel, partition read-back and verification, a real-device Linux → Android → Linux cycle plus cold boot, and a complete clean installation including GNOME initial setup.

## Translation decisions

- “设备模式” and “主机模式” are rendered as “device mode” and “host mode”, preserving USB role distinction.
- NCM is retained as the named USB networking protocol; “USB2 高速” is rendered as “USB 2 high-speed”.
- “控制器角色切换” is rendered as “controller role switching”; UCSI and VBUS remain technical identifiers.
- “存储控制器和 PHY 内建” is rendered as “build the storage controller and PHY into the kernel”, preserving the built-in-versus-module distinction.
- PARTLABEL, GUID, LUN, GPT, A/B, UFS, Fastboot, KernelSU, NCM and command names are retained.
- “活动槽镜像集轮换” is rendered as “active-slot image-set rotation”. This avoids implying that each operating system simply occupies one conventional A/B slot.
- “只截 userdata 尾部给 Linux” is rendered as “allocate only the tail of userdata to Linux”; the source does not specify a numerical partition layout.
- “扩展属性和数字属主” is rendered as “extended attributes and numeric ownership”.
- Sparse-image RAW, FILL and DONT_CARE chunk types are retained as identifiers.
- “写零” is rendered as “write zero” to distinguish a FILL operation from a skipped DONT_CARE region.
- “主备 GPT” is rendered as “primary and backup GPT”.
- “逐分区状态” is retained in quotation marks as “per-partition state”, contrasted with an atomic snapshot.
- “定时回退” is rendered as “timed rollback”; this is a recovery arrangement, not a claim that rollback has been independently verified.
- “读回” is rendered as “read back” where the source requires comparison against written state.
- The final Type-C item is kept separate from the preceding single-port debugging-channel failure. The former gives a broader preparation procedure: establish and verify a second Wi-Fi SSH path, configure timed rollback and consolidate physical interventions.
- Source tags such as 【高频】【致命】【耗时】 are rendered as [Frequent], [Fatal] and [Time-consuming]. Applicability labels and years are retained where supplied.

## Route and acceptance structure

### USB and external display

The source explicitly orders the work: device mode first, as the debugging channel, with USB 2 high-speed NCM preferred; then host mode and external display. Enablement is ordered controller role switching → UCSI → VBUS → enumeration → USB 3 / DisplayPort.

Completion is not just enumeration. It includes storage read/write, mouse input, unplug/replug recovery, USB 3 speed evidence, external display output with EDID and mode evidence, recovery after disconnect, and restoration of the debugging channel after switching.

The failure records distinguish:
- USB network interface naming collisions;
- large-file NCM transfer stalls;
- UCSI reset retry storms and ADSP unresponsiveness;
- host-mode PHY parameter, property, userspace-control and clock-order issues;
- loss of the sole debugging channel when a single Type-C port changes role;
- external-display switch-chip identity, bitmask/pin representation, HPD reporting, SBU driver build mode and internal-panel takeover prerequisites;
- logical UCSI power state from physical power delivery.

### Storage, partitioning and backup

The source prefers built-in storage controller/PHY support and stable partition identity via PARTLABEL/GUID rather than transient device names. It records that `sdX` names and controller names may change between boots or environments.

Backup records distinguish a per-partition collection from an atomic snapshot. GPT padding, mutable FAT partitions, root permissions and active-partition reads can all affect completeness or consistency. Both host and device hashes are required by the described procedure.

The source also warns that a nominally read-only disk-identification operation may still cause the UFS driver to write device attributes during load. This is recorded as a behaviour to account for, not a claim that the whole operation is read-only.

### Dual boot, slots and flashing

The source's dual-boot model is active-slot image-set rotation:
- both systems boot from the currently active slot;
- the target system's boot-image set is written into that active slot during a switch;
- the other system remains in the Linux partition;
- system-side switching uses `dd`, Android-side switching uses a KernelSU module;
- a service marks the slot successful after boot.

Several failure records show why slot state cannot be reduced to one attribute bit:
- bootloader validation also involves paired partition type GUIDs across LUNs and the boot LUN;
- host-side `fastboot flash` resets retry count;
- Android depends on firmware partitions beyond the boot chain;
- failed image-set rotation can destroy the image set currently in use if the actual active-slot contents are not identified first.

The source distinguishes sparse-image chunk semantics. FILL writes zeros and can make flashing take hours even after Fastboot returns OKAY. DONT_CARE skips writes and therefore leaves old data in place; with a fixed filesystem UUID, old journal contents can be replayed. These are different failure mechanisms and must not be collapsed into a generic “sparse image corruption” category.

The recovery advice is deliberately cautious: obtain bootloader failure evidence before altering slot state, avoid state-changing diagnostics before the mechanism is understood, read back writes, restore original images on mismatch, and do not reboot after an unsuccessful rotation.

### Installation and maintenance image

The recommended root-filesystem path is on-device formatting followed by tar extraction, preserving extended attributes and numeric ownership. If image delivery is unavoidable, only used blocks should be encoded.

The maintenance image is an in-memory recovery/installation environment with storage drivers, root shell and partition tools. It mounts no partitions and returns to Fastboot after a timeout. It is also the described mechanism for writing the partition table and preloading device-local files before first boot.

Installation acceptance requires a clean, documented, real-device procedure through GNOME initial setup. Pitfalls include:
- host/device parameter mismatch causing “preserve home” to format home anyway;
- missing or semantically different tools in the in-memory environment;
- BusyBox tar not preserving extended attributes;
- HTTP service startup before USB networking is ready or host-port collision;
- missing device-local calibration/address data at first boot;
- the need to describe before/after-first-boot differences.

## Explicit cross-document relationships

The source body contains no Markdown links to other domain documents. The collection index `experience/README.md` lists this file under USB/Type-C/external display, UFS, partitioning, dual boot, flashing and installation.

`patterns.md` explicitly names this document in:
- **Pattern A:** bootloader-modified state, including slot state that is not represented by attribute bits alone.
- **Pattern D:** residual state, including sparse-image skipped blocks retaining old data.
- **Pattern I:** recovery-path reliability, including Fastboot background writes and the need to preserve a debugging channel.

These are explicit references from the pattern index to this experience document. They are not internal links declared by the source document itself.

## Observer-reconstructed correspondences

- **Control channel as an operational dependency:** USB device mode is deliberately established first as the debugging channel. Host-mode testing can remove that channel on a single-port device, so the source adds a second Wi-Fi SSH path and timed rollback before physical intervention.
- **Logical state versus physical state:** Fastboot OKAY does not prove that background writes have finished; UCSI power status does not prove physical power delivery; bootloader slot attributes do not fully represent slot validity; host-side acceptance of a “preserve home” option does not prove that the device-side script received it.
- **Identity versus enumeration name:** partition and device identity must be recovered from controller, LUN, GPT GUID, label and geometry rather than transient `sdX` names.
- **Write semantics and residual data:** FILL and DONT_CARE have opposite operational consequences—one writes zeros and can be very slow, the other skips writes and can preserve old data. Filesystem metadata and journal regions have their own write requirements.
- **Transaction-like safety boundaries:** slot rotation requires identifying current contents, writing, reading back, restoring on mismatch and withholding reboot. Installation requires verifying device-side effects rather than trusting host-side parameters. This resembles a staged transaction, but the source does not define a formal transaction protocol.
- **Recovery preservation:** the maintenance image, timed rollback, secondary SSH path, no-reboot-on-mismatch rule and avoidance of unattended driver unloads all protect the ability to recover from a failed operation.
- **Device-local versus distributable state:** calibration, addresses and other unique data must not be placed in public images; the installation process must provision them separately before first boot.
- **End-to-end evidence:** acceptance proceeds beyond command success or node presence to read-back, peripheral behaviour, disconnect recovery, real boot cycles and a clean user installation.

These are cross-document and within-document correspondences reconstructed by the observer. The source does not define a unified transaction, recovery or state-machine architecture.

## Unresolved and underspecified boundaries

The source has no explicit `Unresolved` section. This must not be read as evidence that all questions are resolved.

The following boundaries remain visible in the text:
- The exact partition geometry and sizing policy are not specified; only the use of the userdata tail and PARTLABEL/GUID identity are stated.
- The exact implementation and verification semantics of active-slot rotation are not fully specified beyond the described operations and safeguards.
- The source gives no complete acceptance threshold for USB 3 speed, NCM transfer performance or external-display mode selection.
- The external-display section is a compact list of issues, not a full bring-up route.
- The UFS attribute writes are acknowledged, but the exact attributes and their effects are not enumerated.
- Backup consistency is explicitly limited: a “per-partition state” is not an atomic snapshot.
- The maintenance image's timeout and return-to-Fastboot behaviour are described as design requirements; this source does not provide a detailed implementation or test record for that mechanism.
- No source-level resolution status is given for the individual pitfalls beyond their stated resolutions; this review does not infer whether patches or upstream fixes have since changed them.

These are evidence boundaries, not newly asserted defects. They do not prevent progression in Pass 1.

## Bounded sufficiency for Pass 1

For initial structural and linguistic reconstruction, the recommended route, completion criteria, operational failure records, explicit Pattern A/D/I references, and the major identity, write-semantics, recovery and installation boundaries have been recovered. The absence of a dedicated unresolved section is preserved as a structural fact, while gaps in detail remain explicit.

Further investigation would require device-specific partition maps, implementation details for slot rotation and maintenance-image recovery, measured USB/display acceptance data, or author/upstream evidence. No technical claims have been validated in this pass.

## Position change

Pass 1 remains the same declared traversal. The document position advances from `experience/sensors-camera-media.md` (Checkpoint 08) to `experience/usb-and-storage.md` (Checkpoint 09).

The next source should be determined from the current `experience/README.md` ordering. It is `experience/ubuntu-userspace.md`.

Pass number, document position and session checkpoint number remain separate coordinates.
