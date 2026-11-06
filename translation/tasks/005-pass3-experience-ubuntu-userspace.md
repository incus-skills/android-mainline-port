# Task 005 — Pass 3 review of experience/ubuntu-userspace.md

## Purpose and observer position

Perform the Pass 3 technical and semantic translation review of the Ubuntu userspace experience document. Compare the complete Chinese source with its English translation. Preserve source meaning and operational force; do not validate technical claims against current upstream implementations or real devices.

This is a bounded document-level operation within Pass 3, not a new pass and not a technical audit.

## Position and governing criteria

- Pass: Pass 3 — technical and semantic review of the operational English translation set.
- Previous operation: Task 004 — Pass 3 review of `experience/usb-and-storage.md`; its accepted worker handoff and independent evaluation are recorded in [completed Task 004](https://github.com/incus-skills/android-mainline-port/blob/translation/english-reconstruction/translation/tasks/completed/004-pass3-experience-usb-and-storage.md) and [Pass 3 / Checkpoint 20](https://github.com/incus-skills/android-mainline-port/blob/translation/english-reconstruction/reconstruction/zh/continuation/checkpoints/pass-3-checkpoint-20.md).
- Current position: Pass 3 / Checkpoint 20; `experience/usb-and-storage.md` is complete and `experience/ubuntu-userspace.md` is next.
- Next-document ordering is corroborated by the source experience-library index and its English counterpart. This is a document-order relationship, not evidence that USB/storage findings determine the Ubuntu userspace analysis.
- Pass 3 scope and review dimensions: `reconstruction/zh/continuation/checkpoints/pass-3-checkpoint-01.md`.
- Applicable translation principles and Phase 4 review instructions: `translation/README.md`.
- Controlled terminology: `translation/GLOSSARY.md`.
- Execution boundary and handoff fields: `translation/EXECUTION-SEGMENTATION.md`.

The Chinese source explicitly says the recommended overall architecture and package split are described in `../references/ubuntu.md`, and that this experience document records specific pitfalls only. Preserve that boundary and the reference. The corresponding source and English architecture-reference files are included below only to verify the named reference/counterpart and its boundary; do not turn this task into a review or technical audit of the architecture reference.

## Exact evidence baseline

Fetch these exact files and verify the revisions before relying on them:

| Role | Repository path | Ref | Blob SHA |
|---|---|---|---|
| Chinese source | `experience/ubuntu-userspace.md` | `main` | `091d4ba144a77ad20707259a840d6599b2e501da` |
| English target | `translation/en/experience/ubuntu-userspace.md` | `translation/english-reconstruction` | `baa0b4ee44c5507e3fc5df2ad527da24e6c06355` |
| Existing review (Pass 1) | `translation/reviews/experience/ubuntu-userspace.md.md` | `translation/english-reconstruction` | `c9d57da433c3dbfb92321fa1ef92874b1a2d6a35` |
| Pass 3 criteria | `reconstruction/zh/continuation/checkpoints/pass-3-checkpoint-01.md` | `translation/english-reconstruction` | `0870d3ea261a6354649379b96891cda4ca2a0ba6` |
| Current position | `reconstruction/zh/continuation/current-position.md` | `translation/english-reconstruction` | `7082412453ff68ad2184fcda4ba0a7e9ed2b8229` |
| Current checkpoint | `reconstruction/zh/continuation/checkpoints/pass-3-checkpoint-20.md` | `translation/english-reconstruction` | `fa0f3a3231b516aaf35197d095e854b158fc14dd` |
| Completed preceding task | `translation/tasks/completed/004-pass3-experience-usb-and-storage.md` | `translation/english-reconstruction` | `91b478fb9453ff68bcf4ea09b26d49a7a000bf7b` |
| Glossary | `translation/GLOSSARY.md` | `translation/english-reconstruction` | `7cd456b51223fd2cd3cf408cb14bae7f7d167254` |
| Translation instructions | `translation/README.md` | `translation/english-reconstruction` | `f9bf34ae28e838ae881f9e207797f26c1b327baa` |
| Segmentation arrangement | `translation/EXECUTION-SEGMENTATION.md` | `translation/english-reconstruction` | `412268a0c065a628d65c0c2c1799830f718e92d1` |
| Source experience-library index | `experience/README.md` | `main` | `b8b4b9110215280c35316df33d540e292c09356a` |
| English experience-library index | `translation/en/experience/README.md` | `translation/english-reconstruction` | `a7a28190615253246749909b380ddcba17268769` |
| Explicitly referenced architecture source (boundary context only) | `references/ubuntu.md` | `main` | `2f87d2512753af379615e05a7d15ea773fbafc0f` |
| English counterpart of architecture reference (boundary context only) | `translation/en/references/ubuntu.md` | `translation/english-reconstruction` | `7e70f8bb2457641984c194bc13a48396d5956ded` |

The existing review is Pass 1 provenance, not a substitute for comparing the complete source and target afresh. It records explicit and observer-reconstructed relationships separately; retain that distinction. Do not load additional domain documents unless a concrete translation claim requires them.

## Scope of work

Review the complete source/target pair against all seven Pass 3 dimensions:

1. Imperative force: requirements, prohibitions, recommendations, defaults, examples and cautions.
2. Conditional scope: applicability, exceptions and limits.
3. Decision logic: conditions, branches, alternatives and owner decisions.
4. Technical tokens: commands, paths, identifiers, package names, service names, interfaces, error strings, versions and configuration values.
5. Structure: headings, lists, examples and relationships.
6. Cross-document references: direct links and named dependencies, distinguishing source-explicit relationships from review-record or observer-reconstructed correspondences.
7. Uncertainty: preserve ambiguity and missing source information rather than silently resolving it.

Pay particular attention to preserving distinctions among:
- the document's specific-pitfall role and its explicit delegation of overall architecture/package split to `references/ubuntu.md`;
- package installation/configuration state, kernel and initramfs hooks, prerequisite-file availability, and the conditions under which package updates trigger initramfs regeneration;
- build-host/QEMU/chroot hardware discovery under dracut `hostonly`, target-device initramfs content, generic-kernel package reinstallation, kernel version naming and module identity;
- APT dependency/autoremove behaviour and the distinction between the project's kernel package and packages that must remain installed;
- snap GPU provider/consumer boundaries, the `gpu-2404` / `gpu-2604` interfaces, Mesa library replacement, and other libraries expected from the provider;
- image seeding state, preseeded snap tasks, first-boot service timing and the conditions for removing or replacing a seeded application;
- QEMU/TCG snap test limitations versus real-device acceptance, preserving the source's stated evidence boundary;
- GNOME Initial Setup scaling, keyboard-layout writes, hostname and locale behaviour, power-button defaults, and first-run versus later-session context;
- user D-Bus availability in services, network-interface readiness, zram generator behaviour, service enablement across upgrades, and cold-boot timing;
- stale APT lists, displayed filesystem capacity versus actual allocation, executable path shadowing, and missing root-filesystem content;
- GDM/session-specific dconf state, lock-screen on-screen keyboard behaviour, NTFS3 filename encoding, delayed key response, and font fallback;
- package dependency boundaries between device-level functionality and a particular desktop release;
- repeated `Type=oneshot` execution and start-rate limits, and the source's warning against forking core GNOME components to change behaviour.

Do not combine these into a general packaging, desktop or service lifecycle model. Preserve each pitfall, condition, cause, remedy, applicability and acceptance criterion as the source presents it. Do not infer that a source omission has been resolved merely because the source has no dedicated unresolved section.

## Permitted changes

- Modify only `translation/en/experience/ubuntu-userspace.md` where a material, source-justified correction is required.
- Append a clearly labelled Pass 3 section to `translation/reviews/experience/ubuntu-userspace.md.md`, preserving the existing Pass 1 record.
- Do not modify the Chinese source, architecture reference, Pass 3 criteria, current-position/checkpoint records, glossary, inventory, indexes or unrelated files.
- Do not rewrite for style alone.
- If no translation correction is justified, leave the target unchanged and record that result.

## Stop conditions

Stop and report rather than broaden the task if:
- the source, target or governing criteria revision differs from the stated baseline in a material way;
- a required source/target passage cannot be obtained;
- the translation decision depends on external technical correctness or real-device evidence;
- a cross-document relationship cannot be distinguished from a source-explicit link or review-record observation;
- the task cannot be completed within the stated scope.

Do not resolve external technical questions or silently expand the task.

## Validation

- Re-read the complete changed target and review record.
- Confirm the Chinese source is unchanged.
- Confirm every proposed target change is supported by the Chinese source.
- Confirm the explicit `references/ubuntu.md` boundary and other source-explicit references remain accurately represented.
- Confirm source-explicit references and review-record correspondences remain distinguished.
- Record the exact revisions examined, changed files, checks performed and unresolved matters.
- Keep the Pass 1 review content intact.

## Required handoff

Append the reconstructable handoff to this task record, following the eight fields in `translation/EXECUTION-SEGMENTATION.md`: task identity; observer position; observations; decisions; unresolved matters; validation; change boundary; next dependency.

The handoff must state whether the task was completed, stopped for an adequacy reason, or requires further evidence. Do not move this task to `completed/` until worker execution and handoff are complete. Moving it does not constitute orchestrator acceptance.


---

## Worker handoff — Pass 3 execution

### 1. Task identity

- Task: 005 — Pass 3 review of `experience/ubuntu-userspace.md`.
- Pass: Pass 3 — technical and semantic review of the operational English translation set.
- Position: Pass 3 / Checkpoint 20; Task 004's USB/storage review is complete; Ubuntu userspace is the selected next document.
- Chinese source: `experience/ubuntu-userspace.md`, `main`, blob `091d4ba144a77ad20707259a840d6599b2e501da`.
- English target before review: `translation/en/experience/ubuntu-userspace.md`, `translation/english-reconstruction`, blob `baa0b4ee44c5507e3fc5df2ad527da24e6c06355`.
- English target after review: `translation/en/experience/ubuntu-userspace.md`, blob `47324f6d9e29c4ea19dc911cd6f67f144e4e5954`.
- Pass 1 review before append: `translation/reviews/experience/ubuntu-userspace.md.md`, blob `c9d57da433c3dbfb92321fa1ef92874b1a2d6a35`.
- Review after Pass 3 append: `translation/reviews/experience/ubuntu-userspace.md.md`, blob `181d7c2de13af82f24c5697c316a9a713e6f7f6d`.

### 2. Observer position

Determine whether the complete English Ubuntu userspace experience document preserves the Chinese source's meaning and operational force across the seven Pass 3 dimensions. This is one bounded source-to-target translation review, not a new pass or technical audit. Excluded current upstream correctness, external technical validation, real-device behaviour and project-wide architecture review.

### 3. Observations

- Compared the complete source and target across imperative force, conditional scope, decision logic, technical tokens, structure, cross-document references and uncertainty.
- The target preserves the source's specific-pitfall role and its explicit delegation of overall architecture and package splitting to `../references/ubuntu.md`.
- Requirements and safeguards remain distinct: hook exit status 0 when prerequisites are absent; no slot write or image-set deletion; harmless initramfs rebuilds at every installation stage; generic-kernel package ordering; consumer-side snap dependency loading; snap seeding completion and deb-presence checks; cold-boot checks; and conditional power-button behaviour.
- Decision branches remain distinct for APT kernel protection, the `gpu-2404` to `gpu-2604` interface transition, Firefox snap removal, suspend state and upgrade-path service enablement.
- Technical tokens, commands, paths, package names, service names, interface names, configuration keys, error strings and version identifiers were checked and did not require correction.
- The source and English experience indexes corroborate the ordering after `usb-and-storage.md`. This is document-order evidence, not a dependency between the two documents.
- The source-explicit named reference is `../references/ubuntu.md`. The Pass 1 review's pattern associations and observer-reconstructed correspondences remain review-record observations, not source-authored dependencies.
- The source explicitly leaves the identity of the gnome-shell sleep-delay inhibitor lock unresolved. The target retains that uncertainty. The source has no dedicated unresolved section; this absence was not treated as proof that all questions are resolved.

### 4. Decisions

- Completed the full seven-dimension source-to-English comparison.
- Corrected exactly two material omissions in the English target: restored the source's `2026` applicability qualifier for the SquashFS/snap-confine entry and for the zram-generator entry.
- Made no style-only rewrites and no other target changes.
- Appended a Pass 3 section to the review record, preserving the complete Pass 1 record.
- Did not review the architecture reference beyond confirming the named source/counterpart and reference boundary.
- Did not perform external technical validation or real-device testing.

### 5. Unresolved matters

- The specific gnome-shell sleep-delay inhibitor lock remains unidentified, as stated in the source.
- Current upstream status, real-device behaviour and technical correctness of the recorded remedies remain unverified by design.
- No further evidence is required to complete this bounded translation review.

### 6. Validation

Verified all 14 task-specified evidence baselines before relying on them; every fetched blob SHA matched:

- Chinese source: `091d4ba144a77ad20707259a840d6599b2e501da`.
- English target: `baa0b4ee44c5507e3fc5df2ad527da24e6c06355`.
- Pass 1 review: `c9d57da433c3dbfb92321fa1ef92874b1a2d6a35`.
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

Post-change validation:

- Re-fetched the Chinese source; it remains at `091d4ba144a77ad20707259a840d6599b2e501da`.
- Re-fetched the English target; it is now `47324f6d9e29c4ea19dc911cd6f67f144e4e5954`.
- Compared the complete target against the original target blob after applying the two intended replacements; the resulting text matches exactly, confirming no other target changes.
- Re-fetched the review record; it is now `181d7c2de13af82f24c5697c316a9a713e6f7f6d`.
- Compared the updated review with the original Pass 1 review blob; the complete original content remains an exact prefix and the Pass 3 section is appended.
- Confirmed both restored `2026` qualifiers in the updated target.
- No source file or excluded project-control file was modified.

### 7. Change boundary

Changed only:

- `translation/en/experience/ubuntu-userspace.md` — two source-justified applicability-year restorations.
- `translation/reviews/experience/ubuntu-userspace.md.md` — appended Pass 3 review, preserving Pass 1.
- `translation/tasks/005-pass3-experience-ubuntu-userspace.md` — appended this eight-field worker handoff.

Left unchanged:

- Chinese source `experience/ubuntu-userspace.md`.
- `references/ubuntu.md` and its English counterpart.
- Pass 3 criteria, current-position/checkpoint records, glossary, translation instructions, indexes, inventory and unrelated files.

### 8. Next dependency

No additional file evidence is required for this bounded translation review. Worker execution and handoff are complete. The orchestrator must independently reconstruct and evaluate this handoff, determine acceptance and select the next operation. No project-level continuation position was changed.

**Worker disposition: COMPLETED.** This records worker execution completion only; it does not constitute orchestrator acceptance or technical validation.
