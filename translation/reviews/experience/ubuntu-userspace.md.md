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


---

## Pass 3 — Technical and Semantic Review

### Evidence and observer position

- Chinese source: `experience/ubuntu-userspace.md`, `main`, blob `091d4ba144a77ad20707259a840d6599b2e501da`.
- English target before review: `translation/en/experience/ubuntu-userspace.md`, `translation/english-reconstruction`, blob `baa0b4ee44c5507e3fc5df2ad527da24e6c06355`.
- English target after review: `translation/en/experience/ubuntu-userspace.md`, `translation/english-reconstruction`, blob `47324f6d9e29c4ea19dc911cd6f67f144e4e5954`.
- Pass 1 review retained below this section; its pre-append blob was `c9d57da433c3dbfb92321fa1ef92874b1a2d6a35`.
- Pass: Pass 3 — technical and semantic review of the operational English translation set.
- Position: Pass 3 / Checkpoint 20; the preceding USB/storage review is complete and Ubuntu userspace is the next document.
- Observer question: does the complete English document preserve the Chinese source's meaning and operational force across all seven Pass 3 dimensions?
- Excluded: current upstream correctness, external technical validation, real-device behaviour and project-wide architecture review.

The source explicitly delegates the recommended overall architecture and package split to `../references/ubuntu.md`, and describes this document as recording specific pitfalls only. That boundary is preserved. The architecture reference and English counterpart were checked only to confirm the named reference and its corresponding location; neither was reviewed for technical correctness.

### Observations

The complete source/target comparison covered all seven dimensions.

1. **Imperative force.** Requirements, prohibitions, recommendations, cautions and defaults remain distinct. In particular, the hook's successful exit when prerequisites are missing, its prohibition on writing a slot or deleting an image set, the installation-order constraint, the generic-kernel purge/install order, the consumer-side snap loading check, the seeding completion condition and deb-presence guard, cold-boot verification, and the conditional power-button choices are retained.

2. **Conditional scope.** Applicability qualifiers and conditional branches are retained. Two explicit year qualifiers were absent from the English target: the SquashFS/snap-confine entry was marked `Ubuntu; 2026` in the source, and the zram-generator entry was marked `Ubuntu; 2026`. Both omissions have been corrected. The other applicability labels, including entries with no year in the source, remain as written.

3. **Decision logic.** The target preserves the alternatives and conditions in the source: kernel package naming versus an exact `APT::NeverAutoRemove` entry; the `gpu-2404` provider and the later `gpu-2604` interface; deferred Firefox snap removal after seeding and confirmation that the deb exists; the power-button choice while suspend is incomplete versus after it works; and the source's stated cold-boot and rollback checks.

4. **Technical tokens.** Commands, paths, package and service names, interfaces, configuration keys, error strings, version identifiers and values were checked. Examples include `dpkg --configure -a`, `/etc/flash-kernel/machine`, `hostonly`, `hostonly_cmdline`, `gpu-2404`, `gpu-2604`, `XKBLAYOUT`, `ProtectHome`, `BindToDevice=usb0`, `FreeBind`, `IPAddressAllow`, `IPAddressDeny`, `deb-systemd-helper`, `XDG_CONFIG_HOME`, `iocharset=utf8`, `InhibitDelayMaxSec`, `ListInhibitors`, `Type=oneshot`, `RemainAfterExit` and `StartLimitBurst`. No token correction was justified by the source comparison.

5. **Structure.** The completion criteria and individual pitfall entries retain their order and relationships. Where the source supplies only a cause, resolution or applicability field, the target does not invent a missing symptom, confirmation procedure or prevention rule. The source's specific-pitfall format is not recast as a general packaging or service-lifecycle model.

6. **Cross-document references.** The only source-explicit named document dependency in this source is `../references/ubuntu.md`, for overall architecture and package splitting. The source and English experience indexes corroborate the document's classification and its position after `usb-and-storage.md`; this is index/order evidence, not a claim that USB/storage findings determine this document's analysis. The Pass 1 review's pattern correspondences and observer-reconstructed relationships remain review-record observations, not source-authored dependencies. The English relative reference resolves to `translation/en/references/ubuntu.md`.

7. **Uncertainty.** The source explicitly leaves the identity of the gnome-shell sleep-delay inhibitor lock unresolved. The target retains that uncertainty and the stated passive evidence-gathering approach. The source has no dedicated unresolved section; that absence is not treated as proof that every question is resolved. No ambiguity was silently repaired using external technical assumptions.

### Decisions

- Completed the fresh comparison of the complete Chinese source and English target across all seven Pass 3 dimensions.
- Made two material, source-justified corrections in the English target, restoring the `2026` applicability qualifier to the SquashFS/snap-confine and zram-generator entries.
- Made no style-only rewrites and no other target changes.
- Appended this Pass 3 review while preserving the complete Pass 1 review above.
- Did not validate technical claims against upstream implementations or real devices, and did not expand into the architecture reference.

### Unresolved matters

- The specific sleep-delay inhibitor lock held by gnome-shell remains unidentified, as stated by the source.
- The source does not provide a dedicated unresolved section; no additional unresolved status has been inferred from that structural omission.
- Current upstream status, real-device behaviour and technical correctness of the recorded remedies remain unverified by design.
- No additional evidence is required to complete this bounded translation comparison.

### Validation

The task-specified evidence baseline was fetched and each blob SHA matched:

- Chinese source: `091d4ba144a77ad20707259a840d6599b2e501da`.
- English target before review: `baa0b4ee44c5507e3fc5df2ad527da24e6c06355`.
- Pass 1 review before append: `c9d57da433c3dbfb92321fa1ef92874b1a2d6a35`.
- Pass 3 criteria: `0870d3ea261a6354649379b96891cda4ca2a0ba6`.
- Current position: `7082412453ff68ad2184fcda4ba0a7e9ed2b8229`.
- Checkpoint 20: `fa0f3a3231b516aaf35197d095e854b158fc14dd`.
- Completed Task 004: `91b478fb9453ff68bcf4ea09b26d49a7a000bf7b`.
- Glossary: `7cd456b51223fd2cd3cf408cb14bae7f7d167254`.
- Translation instructions: `f9bf34ae28e838ae881f9e207797f26c1b327baa`.
- Segmentation arrangement: `412268a0c065a628d65c0c2c1799830f718e92d1`.
- Source experience index: `b8b4b9110215280c35316df33d540e292c09356a`.
- English experience index: `a7a28190615253246749909b380ddcba17268769`.
- Architecture source (boundary check only): `2f87d2512753af379615e05a7d15ea773fbafc0f`.
- English architecture counterpart (boundary check only): `7e70f8bb2457641984c194bc13a48396d5956ded`.

The source, target, review and governing references matched the task's specified revisions before changes. The target changes are limited to the two applicability qualifiers. The Chinese source was not modified. The Pass 1 review was used as provenance and retained intact.

### Change boundary

Changed:

- `translation/en/experience/ubuntu-userspace.md` — restored two source-specified `2026` applicability qualifiers.
- `translation/reviews/experience/ubuntu-userspace.md.md` — appended this Pass 3 review.

Left unchanged:

- Chinese source `experience/ubuntu-userspace.md`.
- `references/ubuntu.md` and its English counterpart.
- Pass 3 criteria, current-position and checkpoint records, glossary, translation instructions, indexes, inventory and unrelated files.

### Next dependency

No additional file evidence is required for this bounded review. Worker execution and handoff are ready for completion. The orchestrator must independently reconstruct and evaluate the handoff, decide acceptance and determine the next operation. No project-level continuation position has been changed.

**Worker disposition: COMPLETED.** This records worker execution completion only; it does not constitute orchestrator acceptance or technical validation.
