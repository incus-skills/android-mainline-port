# Task 003 — Pass 3 review of experience/sensors-camera-media.md

## Purpose and observer position

Perform the Pass 3 technical and semantic translation review of the sensors-camera-media experience document. Compare the Chinese source with its English translation. Preserve source meaning and operational force; do not validate technical claims against current upstream implementations or real devices.

This is a bounded document-level operation within Pass 3, not a new pass and not a technical audit.

## Position and governing criteria

- Pass: Pass 3 — technical and semantic review of the operational English translation set.
- Previous operation: Task 002 — Pass 3 review of `experience/power-and-time.md`; its worker handoff and independent orchestrator evaluation are recorded in `translation/tasks/completed/002-pass3-experience-power-and-time.md` and Pass 3 / Checkpoint 18.
- Current position: Pass 3 / Checkpoint 18; `experience/power-and-time.md` is complete and `experience/sensors-camera-media.md` is next.
- Next-document ordering is corroborated by the source experience-library index and its English counterpart. This is a document-order relationship, not evidence that power-and-time findings determine the sensors/camera/media analysis.
- Pass 3 scope and review dimensions: `reconstruction/zh/continuation/checkpoints/pass-3-checkpoint-01.md`.
- Applicable translation principles and Phase 4 review instructions: `translation/README.md`.
- Controlled terminology: `translation/GLOSSARY.md`.
- Execution boundary and handoff fields: `translation/EXECUTION-SEGMENTATION.md`.

## Exact evidence baseline

Fetch these exact files and verify the revisions before relying on them:

| Role | Repository path | Ref | Blob SHA |
|---|---|---|---|
| Chinese source | `experience/sensors-camera-media.md` | `main` | `98f85dbecb4ec3248a31ac337729784788f0423b` |
| English target | `translation/en/experience/sensors-camera-media.md` | `translation/english-reconstruction` | `dbd1f5cef992ccccf1dbba3bc9ddc22342296343` |
| Existing review (Pass 1) | `translation/reviews/experience/sensors-camera-media.md.md` | `translation/english-reconstruction` | `fb434cf2ac490e8e7968d56049aacbc2f64d0cd5` |
| Pass 3 criteria | `reconstruction/zh/continuation/checkpoints/pass-3-checkpoint-01.md` | `translation/english-reconstruction` | `0870d3ea261a6354649379b96891cda4ca2a0ba6` |
| Current position | `reconstruction/zh/continuation/current-position.md` | `translation/english-reconstruction` | `527c7e5c6a8931d27d9982cbeaa275f7322c8fed` |
| Current checkpoint | `reconstruction/zh/continuation/checkpoints/pass-3-checkpoint-18.md` | `translation/english-reconstruction` | `b7cc2abd09e8fb620e49549f50299d868f4442ed` |
| Completed preceding task | `translation/tasks/completed/002-pass3-experience-power-and-time.md` | `translation/english-reconstruction` | `b248e9bfeeb6658de3909a9a7e33e326fd3bc0786` |
| Glossary | `translation/GLOSSARY.md` | `translation/english-reconstruction` | `7cd456b51223fd2cd3cf408cb14bae7f7d167254` |
| Translation instructions | `translation/README.md` | `translation/english-reconstruction` | `f9bf34ae28e838ae881f9e207797f26c1b327baa` |
| Segmentation arrangement | `translation/EXECUTION-SEGMENTATION.md` | `translation/english-reconstruction` | `412268a0c065a628d65c0c2c1799830f718e92d1` |
| Experience-library index | `experience/README.md` | `main` | `b8b4b9110215280c35316df33d540e292c09356a` |
| English experience-library index | `translation/en/experience/README.md` | `translation/english-reconstruction` | `a7a28190615253246749909b380ddcba17268769` |

The existing review is Pass 1 provenance, not a substitute for comparing the complete source and target afresh. The source document body does not declare direct Markdown links to other domain documents. The Pass 1 review records Pattern D/F/I relationships as cross-document observations; treat these as leads, not as source authority, and do not load additional documents unless a concrete translation claim requires them.

## Scope of work

Review the complete source/target pair against all seven Pass 3 dimensions:

1. Imperative force: requirements, prohibitions, recommendations, defaults, examples and cautions.
2. Conditional scope: applicability, exceptions and limits.
3. Decision logic: conditions, branches, alternatives and owner decisions.
4. Technical tokens: commands, paths, identifiers, filenames, kernel interfaces, configuration keys, error strings, units and version references.
5. Structure: headings, tables, lists, examples and relationships.
6. Cross-document references: direct links and named dependencies, distinguishing source-explicit relationships from review-record or observer-reconstructed correspondences.
7. Uncertainty: preserve ambiguity and missing source information rather than silently resolving it.

Preserve the distinct routes and acceptance conditions for Qualcomm SSC sensors/automatic rotation, Iris/Venus hardware video decoding and camera support. Do not infer a unified multimedia architecture or shared lifecycle/state model.

Pay particular attention to:
- device-local sensor calibration/registry data and its read-only/public-repository boundary;
- the multi-layer rotation acceptance evidence and remote-processor/configuration timing;
- fresh-boot requirements and ADSP sensor-PD state retention;
- upstream driver, signed firmware, module, device-tree and GStreamer prerequisites for video decoding;
- V4L2 operations versus firmware-message sequencing and fast versus slow frame consumption;
- camera support prerequisites, real-scene acceptance and application-level availability;
- the distinction between confirmed and possible causes, especially the unresolved persistent `-EPIPE` condition;
- safety and recovery constraints around video-driver unload/deadlock;
- unresolved new-SoC camera support, autofocus and video recording.

Do not promote a possible cause to a confirmed cause, add procedures absent from the source, or validate technical claims externally.

## Permitted changes

- Modify only `translation/en/experience/sensors-camera-media.md` where a material, source-justified correction is required.
- Append a clearly labelled Pass 3 section to `translation/reviews/experience/sensors-camera-media.md.md`, preserving the existing Pass 1 record.
- Do not modify the Chinese source, Pass 3 criteria, current-position/checkpoint records, glossary, inventory or unrelated files.
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
