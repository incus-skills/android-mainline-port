# Pass 1 — Checkpoint 10: Ubuntu Userspace

## 1. Position and scope

- Traversal: **Pass 1 — initial structural and linguistic reconstruction**
- Previous checkpoint: [Pass 1 / Checkpoint 09](pass-1-checkpoint-09.md)
- Completed source: `experience/ubuntu-userspace.md`
- Source branch: `main`
- Source blob SHA: `091d4ba144a77ad20707259a840d6599b2e501da`
- Architectural reference: `references/ubuntu.md`
- English translation: `translation/en/experience/ubuntu-userspace.md`
- Translation review: `translation/reviews/experience/ubuntu-userspace.md.md`
- Chinese descriptive reconstruction: `reconstruction/zh/ubuntu-userspace.md`
- Next source: `experience/build-and-release.md`
- Technical validation: not performed

Pass 1 remains the initial structural and linguistic reconstruction. This checkpoint does not assess the technical validity or current status of the recorded fixes.

## 2. Reconstructed document role

The source explicitly delegates the recommended overall architecture and package split to `references/ubuntu.md`. It says that this file records concrete pitfalls, written against Ubuntu 26.04 + GNOME 49/50.

Its structure is completion criteria followed by pitfalls. It has neither a recommended-route section nor a dedicated unresolved section.

Completion criteria:
- native APT can install, upgrade and remove all project packages without dpkg half-configured state;
- clean-install GNOME Initial Setup has correct scaling and usable touch, and leaves `systemctl --failed` empty;
- cold boot has no service timeout caused by waiting for a device.

## 3. Main reconstructed groups

### Package lifecycle and boot-image generation

Failures concern flash-kernel platform recognition, missing hook prerequisites, dracut host-only selection in QEMU/chroot, generic kernel reinstallation, reused kernel version names and APT auto-removal of project kernels.

A key source rule: package updates can rebuild initramfs, including a keyboard-layout change in Initial Setup. If prerequisites are absent, the project hook must notify and exit 0 without writing a slot or deleting an image set. Installation order must make intermediate initramfs rebuilds harmless.

### Snap, GPU and seeded applications

The `gpu-2404` interface's provider, `mesa-2404`, supplies more than Mesa libraries. A custom provider containing only Mesa can break consumer snaps that rely on other libraries from the original provider. The source recommends basing the provider on the complete original contents, replacing only Mesa, and testing actual consumer loading.

Firefox removal is a separate snap seeding lifecycle problem. Deleting a seed entry leaves a pending task without a file; removal inside the build VM can cause snapd timeout and transaction rollback. The described route waits until first-boot seeding finishes and confirms the deb version exists before purging the snap.

Other cases cover SquashFS kernel features, xattrs/capabilities, QEMU/TCG limitations and an upstream-fixed ARM64 Flutter font fallback issue.

### GNOME Initial Setup and desktop defaults

The wizard's temporary account differs from the persistent user session. System-wide `/etc/xdg/monitors.xml` is read by GDM, Initial Setup and user sessions. Other cases concern invalid keyboard layout, hostname, user/system language, power-button policy, GDM's separate `XDG_CONFIG_HOME`, lock-screen on-screen keyboard and delayed suspend.

The gnome-shell sleep-delay inhibitor lock remains unidentified. The source proposes passive evidence collection and explicitly rejects masking it by lowering `InhibitDelayMaxSec`.

### systemd, networking and cold-boot ordering

The source records:
- a socket/service dependency cycle around `BindToDevice=usb0`;
- a zram generator delay on kernels without zram;
- upgrade migration that removes an enabled symlink;
- a oneshot service repeatedly running without `RemainAfterExit=yes`.

It requires cold-boot tests after changing units, sockets, udev, initramfs or generators. Runtime switching does not reproduce the described socket dependency failure.

### Root filesystem and package boundaries

Other cases cover stale APT lists, manual scripts shadowing packages, incomplete root filesystems, incorrect `/dev/null` permissions, NTFS3 filename encoding, misleading storage display, and package dependencies tying basic device functions to GNOME.

A recurring distinction is that basic device packages should not depend on desktop components; desktop adaptation belongs in a separate package. Core GNOME components should not be forked casually; minimal source patches need an upstream-removal condition.

## 4. Explicit cross-document relationships

- The source explicitly points to `../references/ubuntu.md` for the recommended architecture and package split.
- `experience/README.md` classifies this file under Ubuntu packaging, dracut, snap, GNOME Initial Setup and desktop defaults.
- `patterns.md` explicitly lists this document under:
  - **Pattern C:** lifecycle/timing, including sockets bound to interfaces not yet created and repeated oneshot execution;
  - **Pattern D:** residual state, including fresh-install and stale package/build state;
  - **Pattern G:** upstream/distribution fixes, including the snap-store font issue.

The source does not itself link to `patterns.md`; those are reverse references from the pattern index.

## 5. Observer-reconstructed correspondences

- **Package lifecycle as a trigger surface:** unrelated package operations, including Initial Setup keyboard changes, can invoke initramfs hooks; intermediate states matter.
- **Build environment versus target environment:** dracut host-only selection and QEMU/TCG snap tests may not transfer to the target device.
- **Provider versus consumer completeness:** provider dependencies alone do not establish consumer runtime success.
- **Temporary setup identity versus persistent user identity:** Initial Setup, GDM and user sessions differ in account, configuration path and lifecycle.
- **Runtime success versus cold-boot correctness:** socket activation, systemd ordering, udev and initramfs interactions can fail only at cold boot.
- **Device layer versus desktop policy:** package dependencies can couple basic device functions to a particular desktop version.
- **Upstream remediation versus local divergence:** the source prefers an upstream fix where available and requires exit conditions for temporary local patches.
- **Evidence at the actual consumer:** validate the process reading configuration, the snap loading the provider and the final user-facing path.

These correspondences do not establish a source-defined package state machine or unified lifecycle model.

## 6. Unresolved and underspecified boundaries

The source has no dedicated unresolved section, but explicitly states that the particular gnome-shell sleep-delay inhibitor lock has not been identified. It proposes passive collection of `ListInhibitors` and the Shell main-thread wait point, plus wall-clock sleep measurement.

Other limits:
- exact device package names and final package split are delegated to `references/ubuntu.md` and project adaptation;
- most cases lack patch identifiers or current upstream status;
- the file does not provide a complete installation or upgrade procedure;
- stated Ubuntu/GNOME versions and applicability labels are not independently checked in this pass.

## 7. Bounded sufficiency

For initial structural and linguistic reconstruction, the document role, completion criteria, pitfall groups, explicit architecture-reference pointer, reverse Pattern C/D/G references and main evidence boundaries have been recovered.

No technical claims have been validated. Further investigation would require package implementation details, exact unit/package versions, upstream issue or patch evidence, or author clarification concerning the inhibitor lock.

## 8. Position change

Pass 1 remains unchanged in purpose and scope. The document position advances from `experience/usb-and-storage.md` (Checkpoint 09) to `experience/ubuntu-userspace.md` (Checkpoint 10).

The next source in `experience/README.md` is `experience/build-and-release.md`.

Pass number, document position and session checkpoint number remain separate coordinates.

## 9. Re-entry sequence

1. Read `reconstruction/zh/continuation/current-position.md`.
2. Read this checkpoint, then Checkpoint 09 and earlier checkpoints as needed.
3. Read `reconstruction/zh/README.md` and `translation/INVENTORY.md`.
4. Revisit `references/ubuntu.md`, `experience/patterns.md` (Patterns C, D and G), and relevant preceding experience documents as needed.
5. Independently inspect `experience/build-and-release.md`; distinguish its own claims from expectations formed by previous documents.
