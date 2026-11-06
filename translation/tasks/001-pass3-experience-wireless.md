# Task 001 — Pass 3 review of experience/wireless.md

## Purpose and observer position

Perform the Pass 3 technical and semantic translation review of the wireless experience document. Compare the Chinese source with its English translation. Preserve source meaning and operational force; do not validate the technical claims against current upstream implementations or real devices.

This is a bounded document-level operation within Pass 3, not a new pass and not a technical audit.

## Position and governing criteria

- Pass: Pass 3 — technical and semantic review of the operational English translation set.
- Current project position: Pass 3 / Checkpoint 16; the preceding completed document is `experience/audio.md`; this is the next document.
- Pass 3 scope and review dimensions: `reconstruction/zh/continuation/checkpoints/pass-3-checkpoint-01.md`.
- Applicable translation principles and Phase 4 review instructions: `translation/README.md`.
- Controlled terminology: `translation/GLOSSARY.md`.
- Execution boundary and handoff fields: `translation/EXECUTION-SEGMENTATION.md`.

## Exact evidence baseline

Fetch these exact files and verify the revisions before relying on them:

| Role | Repository path | Ref | Blob SHA |
|---|---|---|---|
| Chinese source | `experience/wireless.md` | `main` | `c8ae92e9d20207fd686398412da8df3e49216768` |
| English target | `translation/en/experience/wireless.md` | `translation/english-reconstruction` | `b033277ac0b59b81060adc54d2922c191037c27f` |
| Existing review (Pass 1) | `translation/reviews/experience/wireless.md.md` | `translation/english-reconstruction` | `f0ac2791ea27b600678b2ed5b87f9cefc2eaae1d` |
| Pass 3 criteria | `reconstruction/zh/continuation/checkpoints/pass-3-checkpoint-01.md` | `translation/english-reconstruction` | `0870d3ea261a6354649379b96891cda4ca2a0ba6` |
| Current position | `reconstruction/zh/continuation/current-position.md` | `translation/english-reconstruction` | `f28ee4ccac568bb283e13a4c1d40bb65c9076969` |
| Current checkpoint | `reconstruction/zh/continuation/checkpoints/pass-3-checkpoint-16.md` | `translation/english-reconstruction` | `55a7fbd7ef2c3ac922282ff6c53e9e31c51a79b0` |
| Glossary | `translation/GLOSSARY.md` | `translation/english-reconstruction` | `7cd456b51223fd2cd3cf408cb14bae7f7d167254` |

The existing review is Pass 1 provenance, not a substitute for comparing the complete source and target afresh.

## Scope of work

Review the complete source/target pair against all seven Pass 3 dimensions:

1. Imperative force: requirements, prohibitions, recommendations, defaults, examples and cautions.
2. Conditional scope: applicability, exceptions and limits.
3. Decision logic: conditions, branches, alternatives and owner decisions.
4. Technical tokens: commands, paths, identifiers, filenames, kernel interfaces, configuration keys, error strings, units and version references.
5. Structure: headings, tables, lists, examples and relationships.
6. Cross-document references: direct links and named dependencies, distinguishing source-explicit relationships from review-record or observer-reconstructed correspondences.
7. Uncertainty: preserve ambiguity and missing source information rather than silently resolving it.

The source contains separate Wi-Fi and Bluetooth routes and distinct completion criteria. Preserve their differences; do not infer a unified wireless architecture or acceptance model unless the source establishes one. Check the failure records and unresolved items without imposing a uniform structure the source does not have.

The existing Pass 1 review records collection-level relationships to patterns C, F, G and H. Treat those as leads to verify, not as source authority. The source itself may not establish direct cross-document links. Do not make the patterns documents mandatory inputs unless the source/target comparison shows that they are needed to assess a concrete translation claim. If you consult them, record the exact revisions and distinguish their evidential role.

## Permitted changes

- Modify only `translation/en/experience/wireless.md` where a material, source-justified correction is required.
- Append a clearly labelled Pass 3 section to `translation/reviews/experience/wireless.md.md`, preserving the existing Pass 1 record.
- Do not modify the Chinese source, the Pass 3 criteria, the current-position/checkpoint records, the glossary or unrelated files.
- Do not rewrite for style alone.
- If no translation correction is justified, leave the target unchanged and record that result.

## Stop conditions

Stop and report rather than broaden the task if:
- the source, target or governing criteria revision differs from the stated baseline in a material way;
- a required source/target passage or dependency cannot be obtained;
- the translation decision depends on external technical correctness or real-device evidence;
- a cross-document relationship cannot be distinguished from an observer inference;
- the task cannot be completed within the stated scope.

Do not resolve external technical questions or silently expand the review.

## Validation

- Re-read the complete changed target and review record.
- Confirm the Chinese source is unchanged.
- Confirm every proposed target change is supported by the Chinese source.
- Record the exact revisions examined, changed files, checks performed and any unresolved matters.
- Keep the Pass 1 review content intact.

## Required handoff

Append the reconstructable handoff to this task record, following the eight fields in `translation/EXECUTION-SEGMENTATION.md`: task identity; observer position; observations; decisions; unresolved matters; validation; change boundary; next dependency.

The handoff must state whether the task was completed, stopped for an adequacy reason, or requires further evidence. Do not move this task to `completed/` until the worker's execution and handoff are complete. Moving it does not constitute orchestrator acceptance.
