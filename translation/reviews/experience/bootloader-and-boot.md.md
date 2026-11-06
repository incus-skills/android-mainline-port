# Reconstruction and translation review: `experience/bootloader-and-boot.md`

## Source

- Repository: `incus-skills/android-mainline-port`
- Source branch: `main`
- Source path: `experience/bootloader-and-boot.md`
- Source blob SHA: `3b1283ff22c393ca53f02c1b10edc5547389c2b6`
- English artifact: [`translation/en/experience/bootloader-and-boot.md`](../../en/experience/bootloader-and-boot.md)
- Review status: `translated`; semantic and technical review remain outstanding.

## Translation intent and boundary

This is a functional English translation of the source document. It preserves the recommended route, completion criteria, first-boot checklist, individual pitfall records, severity markers, applicability statements, unresolved items, technical identifiers, commands and paths.

No technical recommendation has been corrected, modernised, generalised or independently validated. In particular, version-sensitive statements about Qualcomm ABL, GKI, Gunyah, systemd, kernel configuration and upstream fixes remain source claims.

## Reconstructed document role

This file is a domain-specific operational guide for the boot chain and early system bring-up. It combines three kinds of content:

1. **A recommended progression** from read-only evidence acquisition through offline bootloader reproduction, packaging-time device-tree preparation, timestamped first-boot probes, a minimal initramfs, a RAM-root environment and finally persistent boot.
2. **Acceptance criteria** for the RAM and persistent stages, together with a first-boot checklist.
3. **Accumulated failure experience** describing concrete symptoms, mechanisms, confirmation methods, fixes, prevention and applicability, followed by unresolved problems.

It therefore combines a route through the work with a collection of reusable failure records. The route is not itself a guarantee that a device follows the same image format or bootloader behaviour: the source explicitly requires per-device measurement, even for devices using the same SoC.

## Reconstructed operational structure

### Recommended route

The seven steps form a broad progression:

1. Read-only acquisition of stock runtime device-tree state, boot-image header versions, Fastboot variables and AVB-chain information.
2. Offline reproduction of ABL's device-tree overlay processing using libufdt, with the stock tree as a positive control.
3. Packaging-time export of DTBO and hypervisor-overlay symbols into a disabled container, without adding those placeholders to the upstream-style kernel DTS.
4. Timestamped first-boot probe with a panic timeout and automatic reboot.
5. Minimal initramfs with read-only block devices, a USB networking/debug path and timed exit.
6. RAM-root system using SquashFS, HTTP over USB networking, OverlayFS and `switch_root`.
7. Persistent boot using a format established by tests on the actual device, with root mounted by PARTLABEL.

The progression deliberately separates evidence acquisition and reversible RAM testing from persistent installation.

### Completion and first-boot checks

The RAM acceptance criteria concern handoff observability, USB identity transition, bidirectional command access, dmesg retrieval and automatic return to stock.

Persistent acceptance concerns byte-for-byte boot-partition readback, manifest agreement for runtime cmdline and kernel version, disabled status of the injected container, explainable deferred devices and absence of providers indefinitely stuck in `sync_state`.

The first-boot checklist adds platform identity, virtualization detection, chassis classification and final kernel configuration to the runtime checks.

### Pitfall records

The document contains 19 individual pitfall entries, followed by two unresolved questions. The entries are not all identical in field coverage: some include markers, explicit confirmation or prevention steps, while others give only symptoms, cause, fix and applicability. The translation preserves those source-level differences rather than filling missing fields.

The examples cluster around several recurring concerns:

- ABL transformations and validation of device-tree identity, symbols and boot-image formats.
- Resource ownership and the side effects of placeholder device-tree nodes.
- Runtime identity and systemd's interpretation of injected hypervisor information.
- Command-line, bootconfig and serial-number placement.
- Reversible candidate-kernel testing and Fastboot responsiveness.
- Establishing a debugging channel before accessing hardware or debug interfaces.
- Initramfs-to-root handoff, process lifetime, device naming, firmware and overlapping watchdog protection.
- Recovery-path time bounds and watchdog behaviour.
- Reliable early logging and software reboot-mode handling.

These clusters are descriptive observations across the entries, not a new taxonomy imposed on the source.

## Relationships to other documents

- `experience/README.md` identifies this file as the domain reference for Qualcomm ABL, boot-image formats, first boot, debugging channels, early logs and recovery.
- `experience/patterns.md` points to this file for Category A (bootloader transformations) and Category I (recovery-path reliability), among other cross-domain references.
- `references/process.md` supplies the wider project-stage progression. This file provides detailed boot-chain routes and acceptance criteria relevant to first boot and early bring-up.
- `references/verification.md` supplies the broader evidence and acceptance framework. This file gives concrete boot-specific checks and runtime signals.
- `references/decisions.md` supplies conditional technical decision rules; this file records concrete boot-related mechanisms and device-specific caveats.
- `references/discipline.md` is relevant to technical-debt, recovery and unresolved-problem recording, but this file does not itself define the complete project record structure.
- `experience/usb-and-storage.md` is explicitly referenced for zero-filled images and also shares concerns around boot partitions, storage and installation.

The source establishes these references, but does not define a complete directed dependency graph or prescribe that every listed reference must be read before using this file.

## Terminology and translation decisions

| Source expression | English rendering | Reason |
|---|---|---|
| 只读取证 | read-only evidence acquisition | Preserves the non-mutating constraint on the initial stock-system investigation. |
| 正对照 | positive control | Uses the experimental-control term for a known-good stock device-tree overlay test. |
| 打包期注入符号表 | inject symbols at packaging time | Keeps the operation at packaging time and distinct from changing kernel DTS source. |
| 时标探针 | timestamp probe | Describes the early boot image's timestamped observability purpose. |
| RAM 根 | RAM root / RAM-root system | Preserves the temporary root environment distinct from persistent installation. |
| 持久启动 | persistent boot | Distinguishes the installed boot path from reversible RAM testing. |
| 调试通道 | debugging channel | Covers the USB networking and remote command path used for observation and recovery. |
| 占位节点 | placeholder node | Retains the node's special packaging/overlay role without implying a real device. |
| 假消费者 | fake consumer | Preserves the source's warning about a placeholder being interpreted as a consumer and affecting provider links. |
| 交接 | handoff | Used for the bootloader-to-kernel and initramfs-to-new-root transitions, with the specific transition clear from context. |
| 适用范围 | Applicability | Retains the explicit device/platform/method boundary. |
| 未解决 | Unresolved | Keeps open questions distinct from confirmed reusable pitfalls. |

Commands, kernel symbols, device-tree properties, paths, service names, image formats and configuration identifiers are retained verbatim.

## First-pass observations

1. **The first objective is observability and recoverability, not immediate persistent boot.** The route moves from read-only evidence to offline reproduction, then a timestamped probe and minimal RAM environment before persistent installation.
2. **Bootloader success signals are treated as insufficient evidence.** Several entries distinguish Fastboot's OKAY response from actual handoff, device-tree acceptance, kernel execution or a functioning debugging channel.
3. **The stock device is both evidence source and positive control.** Its runtime device tree, boot configuration, image structure and known-good overlay behaviour are used to identify transformations and validate offline reproduction.
4. **Packaging is an explicit intervention boundary.** The symbol-table workaround is placed in packaging rather than upstream-style kernel DTS source. Device identity is also checked at packaging time where ABL validates it before kernel execution.
5. **Reversibility is built into the route.** Timed reboot, read-only block devices, RAM testing, delayed persistent installation and a working debug channel limit the cost of early failures.
6. **The debugging channel is a prerequisite for risky observation.** The source warns that reading registers, clock debug interfaces or storage metadata too early can hang the device before remote recovery is available.
7. **The initramfs-to-root transition is a lifecycle boundary of its own.** Processes, ports, network naming, device nodes, firmware and watchdog timers must be transferred or replaced deliberately; simply changing the root filesystem is insufficient.
8. **Recovery mechanisms require independent testing.** Timeouts, `sync`, reboot flags and watchdogs can themselves block or fail. A recovery path that has not been tested in its final combination is not established as reliable.
9. **The document distinguishes known pitfalls from unresolved mechanisms.** The two unresolved items concern unattended recovery from early hard hangs and the exact mechanism of Fastboot becoming unresponsive after long idle. The translation leaves both open.

## Ambiguities and limits preserved

- The recommended persistent image structure is described as common, not universal. The source explicitly requires per-device testing.
- The symbol-export instruction allows either the symbols referenced by DTBO or the union of base-DTB symbols. The translation preserves both alternatives.
- The exact overlap and implementation details of bootloader-injected hypervisor overlays are not fully specified here.
- Several entries give a recommended fix without a complete test protocol or acceptance threshold. No additional criteria have been invented.
- The source refers to a "disabled container" and specifies its properties across entries; it does not define a general-purpose schema for all possible placeholder nodes.
- Some applicability statements are broad ("general"), while others are tied to Qualcomm ABL, GKI, Gunyah or a stated year. The translation does not infer narrower or broader scope.
- The source flags one software reboot-mode issue as potentially already fixed upstream. This status has not been checked.
- The exact mechanism behind long-idle Fastboot unresponsiveness remains explicitly unresolved.
- Unattended recovery from an early hard hang remains explicitly unresolved; the source says current approaches depend on timed reboot or human intervention.

## Review boundary

This record reconstructs the source's recommended route, acceptance criteria, first-boot checks, pitfall structure and cross-document relationships. It does not establish that:
- the bootloader, kernel, device-tree, AVB, systemd or watchdog procedures are technically correct for a current device;
- any command or recovery procedure is safe to execute on a particular device;
- the listed ABL/GKI/Gunyah behaviours apply to every Qualcomm platform;
- the stated upstream status remains current;
- the unresolved recovery problems have a satisfactory solution.

Those questions require separate semantic/technical review and, where appropriate, controlled real-device validation. The Chinese source remains authoritative for this translation pass.
