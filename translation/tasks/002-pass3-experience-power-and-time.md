# Task 002 — Pass 3 review of experience/power-and-time.md

## Purpose and observer position

Perform the Pass 3 technical and semantic translation review of the power-and-time experience document. Compare the Chinese source with its English translation. Preserve source meaning and operational force; do not validate technical claims against current upstream implementations or real devices.

This is a bounded document-level operation within Pass 3, not a new pass and not a technical audit.

## Position and governing criteria

- Pass: Pass 3 — technical and semantic review of the operational English translation set.
- Previous operation: Task 001 — Pass 3 review of `experience/wireless.md`; worker handoff is recorded in `translation/tasks/completed/001-pass3-experience-wireless.md`.
- Previous completed document before Task 001: `experience/audio.md`, Pass 3 / Checkpoint 16.
- Current-position and Checkpoint 16 still name wireless as the next document. They have not yet been advanced to record Task 001. Do not treat that stale next-document field as evidence that wireless remains pending. Task 001 is completed; this task advances to the next document in the experience-library order.
- Next document: `experience/power-and-time.md`. The sequence is corroborated by the experience-library index and Pass 1 / Checkpoint 06, which records power-and-time immediately after wireless. This is a document-order relationship, not a claim that wireless findings determine the power-and-time analysis.
- Pass 3 scope and review dimensions: `reconstruction/zh/continuation/checkpoints/pass-3-checkpoint-01.md`.
- Applicable translation principles and Phase 4 review instructions: `translation/README.md`.
- Controlled terminology: `translation/GLOSSARY.md`.
- Execution boundary and handoff fields: `translation/EXECUTION-SEGMENTATION.md`.

## Exact evidence baseline

Fetch these exact files and verify the revisions before relying on them:

| Role | Repository path | Ref | Blob SHA |
|---|---|---|---|
| Chinese source | `experience/power-and-time.md` | `main` | `d74ba3b4bc320f2036f185e7614d8cdc1a703c6b` |
| English target | `translation/en/experience/power-and-time.md` | `translation/english-reconstruction` | `5be75ae67a9876c72208a1dce9b7a58c8a130773` |
| Existing review (Pass 1) | `translation/reviews/experience/power-and-time.md.md` | `translation/english-reconstruction` | `a5c0440ceb361a69403d9057de430f4dbee3642f` |
| Direct source dependency: display | `experience/display.md` | `main` | `6cd5016457d4cbab87fae56a4dd5e3c1204bbbdd` |
| Direct English dependency: display | `translation/en/experience/display.md` | `translation/english-reconstruction` | `ed9ec9b19090dd5907e132569545082b94c43d27` |
| Pass 3 criteria | `reconstruction/zh/continuation/checkpoints/pass-3-checkpoint-01.md` | `translation/english-reconstruction` | `0870d3ea261a6354649379b96891cda4ca2a0ba6` |
| Current position (historical ingress state) | `reconstruction/zh/continuation/current-position.md` | `translation/english-reconstruction` | `f28ee4ccac568bb283e13a4c1d40bb65c9076969` |
| Current checkpoint (historical ingress state) | `reconstruction/zh/continuation/checkpoints/pass-3-checkpoint-16.md` | `translation/english-reconstruction` | `55a7fbd7ef2c3ac922282ff6c53e9e31c51a79b0` |
| Completed preceding task | `translation/tasks/completed/001-pass3-experience-wireless.md` | `translation/english-reconstruction` | `cf97ee82a7770c426ed463888123b7be3782e3ad` |
| Glossary | `translation/GLOSSARY.md` | `translation/english-reconstruction` | `7cd456b51223fd2cd3cf408cb14bae7f7d167254` |
| Translation instructions | `translation/README.md` | `translation/english-reconstruction` | `f9bf34ae28e838ae881f9e207797f26c1b327baa` |
| Segmentation arrangement | `translation/EXECUTION-SEGMENTATION.md` | `translation/english-reconstruction` | `412268a0c065a628d65c0c2c1799830f718e92d1` |
| Experience-library index (source order) | `experience/README.md` | `main` | `b8b4b9110215280c35316df33d540e292c09356a` |
| English experience-library index | `translation/en/experience/README.md` | `translation/english-reconstruction` | `a7a28190615253246749909b380ddcba17268769` |
| Pass 1 ordering evidence | `reconstruction/zh/continuation/checkpoints/pass-1-checkpoint-06.md` | `translation/english-reconstruction` | `7a64b1e13611b82c289114ae60c4b81bafab0ed6` |

The existing review is Pass 1 provenance, not a substitute for comparing the complete source and target afresh. The display files are included because the power-and-time source explicitly points to `display.md` for the unresolved black-screen-after-resume issue. Do not load other documents unless a concrete translation dependency requires them.

## Scope of work

Review the complete source/target pair against all seven Pass 3 dimensions:

1. Imperative force: requirements, prohibitions, recommendations, defaults, examples and cautions.
2. Conditional scope: applicability, exceptions and limits.
3. Decision logic: conditions, branches, alternatives and owner decisions.
4. Technical tokens: commands, paths, identifiers, filenames, kernel interfaces, configuration keys, error strings, units and version references.
5. Structure: headings, tables, lists, examples and relationships.
6. Cross-document references: direct links and named dependencies, distinguishing source-explicit relationships from review-record or observer-reconstructed correspondences.
7. Uncertainty: preserve ambiguity and missing source information rather than silently resolving it.

The source names watchdog and thermal management in its title, but its body coverage is uneven. Do not silently fill those coverage gaps or infer that the title establishes procedures or acceptance criteria absent from the body.

Preserve distinctions among battery level, current, voltage, charge trend, charging flags, charger classification, kernel sysfs and UPower observations. Preserve the separate mechanisms and evidence boundaries for charging, private fast-charge authentication, PMIC writes, RTC/SDAM, NTP/chrony, deep suspend, s2idle, clock measurement, charging-control handoff and fuel-gauge polling. Do not infer a unified power-management state machine.

The source explicitly references `display.md` for the unresolved black-screen-after-resume issue. Verify that the English reference preserves the intended target. The Pass 1 review also records pattern-level and kernel/resource correspondences; treat those as leads, not source authority. Do not make additional documents mandatory inputs unless needed to assess a concrete translation claim.

## Permitted changes

- Modify only `translation/en/experience/power-and-time.md` where a material, source-justified correction is required.
- Append a clearly labelled Pass 3 section to `translation/reviews/experience/power-and-time.md.md`, preserving the existing Pass 1 record.
- Do not modify the Chinese source, display dependency, Pass 3 criteria, current-position/checkpoint records, glossary, inventory or unrelated files.
- Do not rewrite for style alone.
- If no translation correction is justified, leave the target unchanged and record that result.

## Stop conditions

Stop and report rather than broaden the task if:
- the source, target or governing criteria revision differs from the stated baseline in a material way;
- a required source/target passage or dependency cannot be obtained;
- the translation decision depends on external technical correctness or real-device evidence;
- a cross-document relationship cannot be distinguished from an observer inference;
- the task cannot be completed within the stated scope.

Do not resolve external technical questions or silently expand the task.

## Validation

- Re-read the complete changed target and review record.
- Confirm the Chinese source and display dependency are unchanged.
- Confirm every proposed target change is supported by the Chinese source.
- Confirm the direct `display.md` reference resolves to the intended English document.
- Record the exact revisions examined, changed files, checks performed and unresolved matters.
- Keep the Pass 1 review content intact.

## Required handoff

Append the reconstructable handoff to this task record, following the eight fields in `translation/EXECUTION-SEGMENTATION.md`: task identity; observer position; observations; decisions; unresolved matters; validation; change boundary; next dependency.

The handoff must state whether the task was completed, stopped for an adequacy reason, or requires further evidence. Do not move this task to `completed/` until worker execution and handoff are complete. Moving it does not constitute orchestrator acceptance.


---

## Worker handoff — Pass 3 execution

### 1. Task identity

- Task: 002 — Pass 3 review of `experience/power-and-time.md`.
- Pass: Pass 3 — technical and semantic review of the operational English translation set.
- Source: `experience/power-and-time.md`, `main`, blob `d74ba3b4bc320f2036f185e7614d8cdc1a703c6b`.
- English target baseline: `translation/en/experience/power-and-time.md`, blob `5be75ae67a9876c72208a1dce9b7a58c8a130773`.
- Existing review baseline: `translation/reviews/experience/power-and-time.md.md`, blob `a5c0440ceb361a69403d9057de430f4dbee3642f`.
- Current position observed: Pass 3 / Checkpoint 17; next document `experience/power-and-time.md`.

### 2. Observer position

Determine whether the complete English power-and-time experience document preserves the Chinese source's meaning and operational force across the seven Pass 3 dimensions. Excluded external technical correctness, current upstream verification, real-device behaviour and project-wide integration.

### 3. Observations

The complete source and target preserve the distinct battery, charging, private fast-charge, PMIC/RTC, time synchronisation, deep suspend, s2idle, clock-measurement, charging-handoff and fuel-gauge-polling mechanisms. The source does not establish a unified power-management state machine.

One material translation discrepancy was found: the source's `每秒十几次` describes write rejections more than ten times per second (in the teens), whereas the target said “dozens per second”. The latter overstates the source.

The target otherwise preserves the source's uneven coverage: watchdog is named in the title but has no dedicated body procedure or acceptance criterion; thermal management is limited to using vendor temperature zones and thresholds. The three unresolved issues remain unresolved.

The source explicitly points to `display.md` for the black-screen-after-wake issue. The target retains that reference, and the English display counterpart exists at the expected relative location.

### 4. Decisions

- Corrected “dozens per second” to “more than ten times per second”.
- Appended a Pass 3 review section to `translation/reviews/experience/power-and-time.md.md`, retaining the complete Pass 1 review.
- Made no other target changes; no style-only rewrite was justified.
- Did not add watchdog or thermal procedures, a unified power state model, or external technical claims.

### 5. Unresolved matters

The source's three unresolved issues remain open:
- Linux net charging and fast charging require private authentication and negotiation; the first release usually cannot close the gap.
- The cause of the RTC returning to 1970 after an abnormal reset is unknown.
- The display remains black after suspend/resume; the source refers to `display.md`.

The source also leaves average-power and remaining-time readings unexplained for some fuel gauges, and provides no dedicated watchdog procedure or thermal acceptance criteria. These are preserved as source-coverage boundaries, not resolved by inference.

### 6. Validation

- All task-specified source, target, review, display dependency, Pass 3 criteria, glossary, translation instructions, completed Task 001, experience indexes and Pass 1 ordering evidence were fetched.
- All specified revisions matched except the historical current-position SHA. The current-position record has since advanced to Checkpoint 17 and explicitly names power-and-time as next; this agrees with the task's selected document. Checkpoint 17 was read and corroborates Task 001 completion and this next document.
- Complete Chinese source and English target were compared across all seven Pass 3 dimensions.
- Complete source and English display dependency were checked; the English reference resolves to the intended counterpart.
- The changed target and appended review were re-fetched after writing. The Pass 1 review section was retained.
- The Chinese source and display dependency were re-fetched and confirmed unchanged at their specified revisions.
- No external technical or real-device validation was performed.

### 7. Change boundary

Changed only:
- `translation/en/experience/power-and-time.md` — one numerical-language correction.
- `translation/reviews/experience/power-and-time.md.md` — appended the Pass 3 review.

The Chinese source, display dependency, Pass 3 criteria, current-position/checkpoint records, glossary, inventory and unrelated files were left untouched. The existing Pass 1 review content was preserved.

### 8. Next dependency

No additional evidence or document is required for this bounded translation review. Worker execution and handoff are complete. The orchestrator retains responsibility for independently reconstructing and evaluating the handoff, accepting or rejecting the changes, integrating them, and determining the next operation.

**Worker disposition: COMPLETED.** This records worker execution completion only, not orchestrator acceptance.
