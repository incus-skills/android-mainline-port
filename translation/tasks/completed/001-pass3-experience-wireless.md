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


---

## Worker handoff — Pass 3 execution

### 1. Task identity

- Task: 001 — Pass 3 review of `experience/wireless.md`.
- Pass / checkpoint: Pass 3 / Checkpoint 16; next document after `experience/audio.md`.
- Source: `experience/wireless.md`, `main`, blob `c8ae92e9d20207fd686398412da8df3e49216768`.
- Target baseline: `translation/en/experience/wireless.md`, `translation/english-reconstruction`, blob `b033277ac0b59b81060adc54d2922c191037c27f`.
- Target after correction: blob `8bf6c27e2eb3a88fd0b55c8bd02dd7b10c1ee087`.
- Review after append: `translation/reviews/experience/wireless.md.md`, blob `74d162ff756500c6f0d237aa1f2a8b8267c833dd`.

### 2. Observer position

Determine whether the English wireless experience document preserves the Chinese source's operational meaning and force across the seven Pass 3 dimensions. Excluded external technical validation, current upstream verification and real-device testing.

### 3. Observations

The complete source and target preserve the separate Wi-Fi and Bluetooth routes and acceptance criteria, the distinct failure mechanisms, technical tokens, conditional branches and all three unresolved matters. The source contains no Markdown links or named cross-document dependencies. The existing Pass 1 review's Pattern C/F/G/H associations were treated as leads only; no pattern document was needed to assess a source-to-target claim.

One material semantic omission was found: the baseline target did not state that the vendor SAR power limit prevents transmit power from reaching the approximately 20 dBm failure threshold. The source does state this consequence.

### 4. Decisions

- Corrected that single omission in `translation/en/experience/wireless.md`: the vendor limit is applied at every startup, so transmit power never reaches the threshold; mainline does not apply the limit.
- Appended the Pass 3 review to `translation/reviews/experience/wireless.md.md`, preserving the Pass 1 record.
- No other source-justified target changes were required.
- No external technical claims were checked.

### 5. Unresolved matters

The source's three unresolved matters remain open: Bluetooth `-84 frame reassembly`; the approximately eight-second vendor boot-time radio calibration without a mainline counterpart; and the ACL return-path stall after repeated reconnections, with responsibility unattributed. They were not resolved by inference.

### 6. Validation

- All seven task-specified baseline revisions matched before execution.
- The translation README Phase 4 instructions were read.
- The complete source/target pair was reviewed across all seven criteria.
- Re-fetched the changed target and confirmed the SAR correction is present at blob `8bf6c27e2eb3a88fd0b55c8bd02dd7b10c1ee087`.
- Re-fetched the review and confirmed the Pass 3 section is present and the Pass 1 content remains present at blob `74d162ff756500c6f0d237aa1f2a8b8267c833dd`.
- Re-fetched the source and confirmed its blob remains `c8ae92e9d20207fd686398412da8df3e49216768`.
- No real-device or external technical validation was performed.

### 7. Change boundary

Changed only:
- `translation/en/experience/wireless.md` — one source-justified semantic correction.
- `translation/reviews/experience/wireless.md.md` — appended Pass 3 review.

The Chinese source, Pass 1 review section, criteria, glossary, current-position/checkpoint records and unrelated files were left unchanged.

### 8. Next dependency

No additional evidence or document is required for this bounded operation. Worker execution is complete and handed off for independent orchestrator reconstruction and evaluation. The orchestrator retains responsibility for acceptance, integration and determining the next project operation.

**Worker disposition: COMPLETED.** This records execution completion only, not orchestrator acceptance.
