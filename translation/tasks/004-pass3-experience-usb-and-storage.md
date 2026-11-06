# Task 004 — Pass 3 review of experience/usb-and-storage.md

## Purpose and observer position

Perform the Pass 3 technical and semantic translation review of the USB and storage experience document. Compare the complete Chinese source with its English translation. Preserve source meaning and operational force; do not validate technical claims against current upstream implementations or real devices.

This is a bounded document-level operation within Pass 3, not a new pass and not a technical audit.

## Position and governing criteria

- Pass: Pass 3 — technical and semantic review of the operational English translation set.
- Previous operation: Task 003 — Pass 3 review of `experience/sensors-camera-media.md`; its worker handoff and independent orchestrator evaluation are recorded in [completed Task 003](https://github.com/incus-skills/android-mainline-port/blob/translation/english-reconstruction/translation/tasks/completed/003-pass3-experience-sensors-camera-media.md) and [Pass 3 / Checkpoint 19](https://github.com/incus-skills/android-mainline-port/blob/translation/english-reconstruction/reconstruction/zh/continuation/checkpoints/pass-3-checkpoint-19.md).
- Current position: Pass 3 / Checkpoint 19; `experience/sensors-camera-media.md` is complete and `experience/usb-and-storage.md` is next.
- Next-document ordering is corroborated by the source experience-library index and its English counterpart. This is a document-order relationship, not evidence that sensors/camera/media findings determine the USB/storage analysis.
- Pass 3 scope and review dimensions: `reconstruction/zh/continuation/checkpoints/pass-3-checkpoint-01.md`.
- Applicable translation principles and Phase 4 review instructions: `translation/README.md`.
- Controlled terminology: `translation/GLOSSARY.md`.
- Execution boundary and handoff fields: `translation/EXECUTION-SEGMENTATION.md`.

## Exact evidence baseline

Fetch these exact files and verify the revisions before relying on them:

| Role | Repository path | Ref | Blob SHA |
|---|---|---|---|
| Chinese source | `experience/usb-and-storage.md` | `main` | `1737f41450dcbe6ebd6880790a4b4e8c072bc9c7` |
| English target | `translation/en/experience/usb-and-storage.md` | `translation/english-reconstruction` | `e756e98882ece48f879aad7434098d0a7706a07d` |
| Existing review (Pass 1) | `translation/reviews/experience/usb-and-storage.md.md` | `translation/english-reconstruction` | `9498835f6aaa108dd732121de46bc40ab9f10255` |
| Pass 3 criteria | `reconstruction/zh/continuation/checkpoints/pass-3-checkpoint-01.md` | `translation/english-reconstruction` | `0870d3ea261a6354649379b96891cda4ca2a0ba6` |
| Current position | `reconstruction/zh/continuation/current-position.md` | `translation/english-reconstruction` | `712e223a974efe17097cbc6d7e15454e90b3b766` |
| Current checkpoint | `reconstruction/zh/continuation/checkpoints/pass-3-checkpoint-19.md` | `translation/english-reconstruction` | `025bf5d3fbdca8c47a7baa5797eccbd6f5f7f7b2` |
| Completed preceding task | `translation/tasks/completed/003-pass3-experience-sensors-camera-media.md` | `translation/english-reconstruction` | `4d3241b55ded0ee9c01df07e80bf6adf0bc1e6b6` |
| Glossary | `translation/GLOSSARY.md` | `translation/english-reconstruction` | `7cd456b51223fd2cd3cf408cb14bae7f7d167254` |
| Translation instructions | `translation/README.md` | `translation/english-reconstruction` | `f9bf34ae28e838ae881f9e207797f26c1b327baa` |
| Segmentation arrangement | `translation/EXECUTION-SEGMENTATION.md` | `translation/english-reconstruction` | `412268a0c065a628d65c0c2c1799830f718e92d1` |
| Experience-library index | `experience/README.md` | `main` | `b8b4b9110215280c35316df33d540e292c09356a` |
| English experience-library index | `translation/en/experience/README.md` | `translation/english-reconstruction` | `a7a28190615253246749909b380ddcba17268769` |

The existing review is Pass 1 provenance, not a substitute for comparing the complete source and target afresh. The Pass 1 review records Pattern A/D/I relationships; treat these as review-record observations, not source authority. Do not load additional domain documents unless a concrete translation claim requires them. Determine whether the source itself declares direct links or named dependencies.

## Scope of work

Review the complete source/target pair against all seven Pass 3 dimensions:

1. Imperative force: requirements, prohibitions, recommendations, defaults, examples and cautions.
2. Conditional scope: applicability, exceptions and limits.
3. Decision logic: conditions, branches, alternatives and owner decisions.
4. Technical tokens: commands, paths, identifiers, filenames, partition labels, GUIDs, image formats, error strings, units and version references.
5. Structure: headings, tables, lists, examples and relationships.
6. Cross-document references: direct links and named dependencies, distinguishing source-explicit relationships from review-record or observer-reconstructed correspondences.
7. Uncertainty: preserve ambiguity and missing source information rather than silently resolving it.

Pay particular attention to preserving distinctions among:
- USB device mode as a debugging channel, host mode, Type-C role switching, UCSI, VBUS, enumeration and USB 3 / DisplayPort enablement;
- storage controller/PHY integration, PARTLABEL/GUID partition identity and the stated userdata allocation boundary;
- active-slot image-set rotation, the bootloader's A/B state, the Linux partition, system-internal `dd` writes, Android-side KernelSU handling and marking a slot successful;
- on-device formatting and tar extraction (including extended attributes and numeric ownership) versus sparse-image delivery;
- sparse-image RAW, FILL and DONT_CARE semantics, including the difference between Fastboot's OKAY response and completed writes;
- GPT type GUID and slot-attribute state, boot LUN, bootloader-owned state transitions and the prohibition on state-changing diagnostic commands before the mechanism is understood;
- per-partition backup state versus an atomic snapshot, and the identity evidence required before restoring a device whose names may change;
- device-local data and pre-first-boot provisioning versus public image contents;
- maintenance-image safety properties, installation-environment tool differences and USB network transfer behaviour;
- logical UCSI power state versus physical supply, external-display connection evidence and the single-Type-C-port recovery/debugging boundary.

Do not combine these into a general storage or peripheral lifecycle model. Preserve each route, condition, failure mechanism, safety warning and acceptance criterion as the source presents it. Do not infer an unresolved section merely because the document has no dedicated one.

## Permitted changes

- Modify only `translation/en/experience/usb-and-storage.md` where a material, source-justified correction is required.
- Append a clearly labelled Pass 3 section to `translation/reviews/experience/usb-and-storage.md.md`, preserving the existing Pass 1 record.
- Do not modify the Chinese source, Pass 3 criteria, current-position/checkpoint records, glossary, inventory, indexes or unrelated files.
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
- Confirm source-explicit references and review-record correspondences remain distinguished.
- Record the exact revisions examined, changed files, checks performed and unresolved matters.
- Keep the Pass 1 review content intact.

## Required handoff

Append the reconstructable handoff to this task record, following the eight fields in `translation/EXECUTION-SEGMENTATION.md`: task identity; observer position; observations; decisions; unresolved matters; validation; change boundary; next dependency.

The handoff must state whether the task was completed, stopped for an adequacy reason, or requires further evidence. Do not move this task to `completed/` until worker execution and handoff are complete. Moving it does not constitute orchestrator acceptance.
