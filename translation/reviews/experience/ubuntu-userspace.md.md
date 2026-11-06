# Translation Review: Ubuntu Userspace

## Source and scope

- Source: `experience/ubuntu-userspace.md`
- Source branch: `main`
- Source blob SHA: `091d4ba144a77ad20707259a840d6599b2e501da`
- Architectural reference: `references/ubuntu.md` (source blob SHA `2f87d2512753af379615e05a7d15ea773fbafc0f`)
- Collection index: `experience/README.md`
- Pattern index: `experience/patterns.md`
- Traversal: Pass 1 — initial structural and linguistic reconstruction
- Technical validation: not performed

## Document role and boundary

The source explicitly delegates the recommended overall architecture and package split to `references/ubuntu.md`. It describes itself as a record of specific pitfalls, written against Ubuntu 26.04 and GNOME 49/50. The translation preserves this division: it does not copy the reference's layered architecture or package table into the experience record.

The document contains completion criteria followed by a long list of concrete pitfalls. Unlike several preceding experience documents, it has no recommended-route section and no dedicated unresolved section. Its completion criteria are concise and system-level:
- native APT lifecycle works for all project packages, with no half-configured dpkg state;
- clean-install GNOME Initial Setup has correct scaling and touch usability, and leaves no failed systemd units;
- cold boot has no service timeout caused by waiting for a device.

This is not a complete Ubuntu porting procedure. It is an experience record whose cases inform the separately documented integration architecture.

## Translation decisions and terminology

- “用户态” is translated as “userspace”, retaining the distinction from kernel/hardware work.
- “推荐的整体架构和包拆分” remains an explicit pointer to the architectural reference rather than being reconstructed as an architecture within this document.
- “踩过的坑” is rendered as “Pitfalls”, consistent with the experience-library format.
- “半配置” is translated as “half-configured”, retaining dpkg's package-state terminology.
- “启动部署工具” is “boot deployment tool”; the source's concrete reference to flash-kernel is retained.
- “启动镜像集” is “boot-image set” or “image set” depending on local context; it is not collapsed into a single boot image.
- “前置文件” is “prerequisite files”; hook behaviour on missing prerequisites is preserved, including exit status 0 and the prohibition on writing a slot or deleting an image set.
- “宿主机” in the snap discussion is “host”; host-built Mesa is distinguished from libraries available inside the strict snap sandbox.
- “消费者 snap” is “consumer snap”; validation must model the consumer's dependency loading, not only the provider snap's own dependencies.
- “首次设置向导” is “Initial Setup” or “the wizard”; it is distinct from the user's persistent session.
- “pretty hostname” is retained as the system hostname concept used for the Bluetooth name.
- “短按” is “short press”; power-button behaviour remains conditional on whether suspend is implemented.
- “睡眠延迟锁” is “sleep delay inhibitor lock”; the exact unreleased lock remains unlocated, as in the source.
- “冷启动” is “cold boot”; runtime switching tests are not treated as substitutes.
- Technical identifiers, package names, unit names, configuration keys, paths, command lines, version names and interface names are retained.
- Tags 【高频】、【耗时】 and 【致命】 are rendered [Frequent], [Time-consuming] and [Fatal].

## Reconstructed content

### Package lifecycle and boot-image generation

The source records a family of failures in which package operations trigger initramfs or boot-image generation:
- flash-kernel does not recognize the target platform;
- a project hook assumes prerequisite mapping files already exist;
- dracut's host-only selection captures QEMU/chroot build-machine hardware;
- a distribution generic kernel is reinstalled and interferes with the project hook;
- reused kernel version names allow old modules to be associated with new builds;
- APT may auto-remove a project kernel package whose name does not match its protected naming convention.

The source's hook-safety rule is especially important: any package update may rebuild initramfs, including a keyboard-layout change in Initial Setup. A hook with missing prerequisites must notify and exit successfully without writing slots or deleting image sets. Installation order must make an initramfs rebuild harmless at every intermediate point.

The ABI ledger and unique ABI increment are preserved as source-specific operational rules, not elevated into a newly defined package policy in this translation.

### Snap, GPU and distribution-seeded applications

The snap cases distinguish host graphics from the graphics userspace exposed inside a strict snap sandbox. The `gpu-2404` interface's provider is `mesa-2404`; simply connecting a custom provider containing Mesa can remove libraries that consumer snaps expect the original provider to supply. The recommended custom provider therefore starts from the complete original provider contents and replaces only Mesa libraries. Validation simulates consumer loading.

The Firefox case is a different seeding lifecycle issue: deleting a seed entry leaves a pending task without its file, while removal inside the build VM can cause snapd timeout and transaction rollback. The described solution defers snap removal until seeding has completed and confirms that the deb version is already installed.

Other snap records concern kernel SquashFS features, xattrs and capabilities, and the limited transferability of QEMU/TCG results to real hardware. The app-store font issue is explicitly attributed to a shipped ARM64 Flutter engine version that upstream has fixed; the source recommends checking upstream before local modification.

### GNOME Initial Setup and desktop defaults

The Initial Setup records distinguish the temporary wizard environment from a persistent user session:
- display configuration in a user's home directory is unavailable to the temporary account;
- system-wide `/etc/xdg/monitors.xml` is read by GDM, the wizard and user sessions;
- the wizard may write an invalid keyboard layout, overwrite hostname, or set only the user's language;
- the power-button action depends on whether suspend is implemented.

The document also records GDM's distinct `XDG_CONFIG_HOME`, an on-screen keyboard null-pointer issue on the lock screen, and an unresolved sleep-delay inhibitor lock that can delay suspend. The latter is explicitly not to be hidden by lowering `InhibitDelayMaxSec`; the lock itself has not been identified.

### systemd, networking and boot timing

Several cases show that runtime testing does not expose all boot-order failures:
- a socket bound with `BindToDevice=usb0` waits for an interface that is created by a service that itself waits on the socket's boot target;
- a zram generator configuration can wait 90 seconds on a kernel without zram;
- a service migration can remove an enabled symlink, after which `deb-systemd-helper` treats it as a user disablement;
- a `Type=oneshot` unit without `RemainAfterExit=yes` can run repeatedly as dependencies pull in sysinit, hitting the start limit.

The source calls for cold-boot testing after changes to units, sockets, udev, initramfs and generators, and recommends reproducing the old failure with the previous version as a control.

### Root filesystem, packaging boundaries and user-facing behaviour

The remaining cases include stale build-machine APT lists, manual scripts shadowing packaged versions, incomplete hand-assembled root filesystems, incorrect `/dev/null` permissions, NTFS3 filename encoding, misleading GNOME storage display, and dependencies that couple basic device functions to a specific GNOME version.

A recurring distinction is between device-level functionality and desktop policy: basic device packages should not depend on desktop components; desktop adaptation belongs in a separate package. The final entry discourages forking GNOME core components, preferring minimal distribution-source patches with a recorded upstream-removal condition.

## Explicit cross-document relationships

- The source explicitly points to `../references/ubuntu.md` for the recommended overall architecture and package split.
- `experience/README.md` classifies this document as Ubuntu packaging, dracut, snap, GNOME Initial Setup and desktop defaults.
- `patterns.md` explicitly lists this document under:
  - **Pattern C:** lifecycle/timing failures, including sockets bound to interfaces not yet created and repeated oneshot execution;
  - **Pattern D:** residual-state failures, including fresh-install and stale package/build state;
  - **Pattern G:** upstream/distribution fixes, including the snap-store font issue.

The source does not itself link to `patterns.md`; these are explicit reverse references from the collection's pattern index.

## Observer-reconstructed correspondences

- **Package lifecycle as a trigger surface:** kernel and initramfs hooks can run during apparently unrelated package operations, including Initial Setup keyboard configuration. Intermediate package states therefore matter, not just the final installed state.
- **Build environment versus target environment:** dracut host-only selection and QEMU/TCG snap testing can encode or expose build-environment behaviour that does not transfer to the target device.
- **Provider versus consumer completeness:** a snap graphics provider can satisfy its own dependencies while failing the actual consumer's runtime requirements.
- **Temporary setup identity versus persistent user identity:** GNOME Initial Setup, GDM and the user's session have different accounts, configuration paths and lifecycle points.
- **Runtime success versus cold-boot correctness:** socket activation, systemd dependency ordering, udev and initramfs interactions can fail only during cold boot; live switching is not an adequate substitute.
- **Device layer versus desktop policy:** package dependencies can accidentally make basic hardware/device functions depend on a particular desktop component or version.
- **Upstream remediation versus local divergence:** the app-store font issue illustrates a source-recorded case where updating to an upstream fix is preferred to maintaining a local patch. The final core-component entry similarly requires a removal condition for any temporary patch.
- **Evidence at the actual consumer:** configuration and package presence are insufficient; validate the process that reads the configuration, the snap that loads the provider, and the user-facing path.

These are observational correspondences. The source does not define a unified package state machine or a formal cross-layer lifecycle model.

## Unresolved and underspecified boundaries

The document has no dedicated unresolved section, but it explicitly preserves at least one unresolved cause:
- The specific gnome-shell sleep-delay inhibitor lock that remains held has not been identified. The source proposes passive collection of `ListInhibitors` and the Shell main-thread wait point, plus wall-clock sleep measurement.

Other limits visible in the record:
- The exact device-specific package names and final package split are delegated to `references/ubuntu.md` and project adaptation.
- The source gives concrete fixes but does not provide patch identifiers or current upstream status for most cases.
- It does not provide a complete installation or upgrade sequence; that is outside its stated role as a pitfalls record.
- The stated Ubuntu/GNOME versions and 2026 applicability labels are retained; they are not independently checked in this pass.

These boundaries do not prevent progression in Pass 1. Technical validity and present-day status remain outside this traversal.

## Bounded sufficiency for Pass 1

For initial structural and linguistic reconstruction, the document's role, completion criteria, pitfall categories, explicit reference to the Ubuntu architecture document, reverse pattern references C/D/G, and its key evidence boundaries have been recovered.

No technical claims have been validated. Further investigation would require the referenced architecture and package implementation, specific unit/package versions, upstream issue or patch evidence, or author clarification for the unresolved inhibitor lock.

## Position change

Pass 1 remains the same declared traversal. The document position advances from `experience/usb-and-storage.md` (Checkpoint 09) to `experience/ubuntu-userspace.md` (Checkpoint 10).

The next source in `experience/README.md` is `experience/build-and-release.md`.

Pass number, document position and session checkpoint number remain separate coordinates.
