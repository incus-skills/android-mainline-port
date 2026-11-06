# Pass 4 — Checkpoint 01: Whole-skill English validation begins

## Position

- Traversal: **Pass 4 — whole-skill English validation**.
- Project phase: **Phase 5 — Whole-skill validation**, as defined in `translation/README.md`.
- Purpose: determine whether the English skill tree is coherent and usable as a skill, rather than merely a collection of individually reviewed translations.
- Previous traversal: Pass 3 — technical and semantic review of the operational English translation set.
- Previous traversal closure: Pass 3 / Checkpoint 23; all declared Pass 3 source-based review operations are recorded as accepted.
- Current observer position: project-level validation of navigation, dependencies, workflow usability, consistency and skill-discovery/install metadata. This is not an extension of Pass 3.
- First bounded operation: Task 008 — entrypoint and navigation integrity of `translation/en/SKILL.md`.
- No Pass 4 worker operation has yet been completed or accepted.

## Scope and exit condition

Pass 4 follows the Phase 5 scope in `translation/README.md`:

1. Check that the English `SKILL.md` points only to existing English documents.
2. Check that an agent can follow the documented reading order.
3. Test representative workflows: new-device assessment, project continuation, technical decision and troubleshooting.
4. Check for contradictions, duplicated rules, missing dependencies and accidental source-language leakage.
5. Review installation and skill-discovery metadata for compatibility.

The traversal may be divided into independently bounded tasks. Each task must identify its own evidence boundary and must not imply completion of the other Phase 5 checks.

The Phase 5 exit condition is reached only when the English tree is coherent and usable as a skill, not merely a collection of translated documents. Acceptance of an individual task does not establish Pass 4 or project completeness.

## First operation and rationale

Task 008 is limited to the English entrypoint and navigation contract. `SKILL.md` is the documented entry point: its use routes, process references and file index determine what an agent is instructed to load. Verifying that these references resolve in the pinned English tree is a prerequisite to meaningful representative workflow execution.

Task 008 is read-only with respect to skill content. It may report defects but must not repair them. Any correction requires a separately bounded task after orchestrator assessment.

## Evidence and provenance

- Previous current-position record before this traversal: `reconstruction/zh/continuation/current-position.md`, blob SHA `2f02b450881f5b0a4381eb56132a404ef024fb52`.
- Pass 3 closure: `reconstruction/zh/continuation/checkpoints/pass-3-checkpoint-23.md`, blob SHA `2f356c06db43c84e5112c7a23c4bb51240c55563`.
- Pass 3 scope declaration: `reconstruction/zh/continuation/checkpoints/pass-3-checkpoint-01.md`, blob SHA `0870d3ea261a6354649379b96891cda4ca2a0ba6`.
- Phase definitions and Phase 5 exit condition: `translation/README.md`, blob SHA `f9bf34ae28e838ae881f9e207797f26c1b327baa`.
- Translation inventory: `translation/INVENTORY.md`, blob SHA `0a8e58e9a6f45fd54c718672dc8d7a3a9056f7ee`.
- Proposed execution arrangement: `translation/EXECUTION-SEGMENTATION.md`, blob SHA `05ee569421b89366bbc0993afe62cb22a0644913`.
- English entrypoint: `translation/en/SKILL.md`, blob SHA `f613aa7ef89eb9204c4f9804faf7aebace596a4e`.
- English tree: `translation/en/`, tree SHA `ba8a19206173ccde8fd26b6ae731ca4a0ee8c7cc`.
- Supporting subtree SHAs: `translation/en/references/` = `41f3799c0dd0caa7bdef8e9b4820f45f21ace473`; `translation/en/experience/` = `d8d0db60cb0ef7dafd527f1d5302683bb3fbbb93`.

## Unresolved project-level record discrepancy

The pinned `translation/EXECUTION-SEGMENTATION.md` still contains an “Application to the current position” paragraph that names Pass 3 / Checkpoint 16 / `experience/wireless.md`. This is stale: the canonical current-position record and this checkpoint establish Pass 4 / Checkpoint 01, while Pass 3 closed at Checkpoint 23. The stale paragraph was already present in the older segmentation revision used by Task 007; the newer revision added evidence-boundary and separate-investigation-proposal guidance but did not update that paragraph.

For position, the current-position record and the active checkpoint take precedence. This discrepancy does not change Phase 5 criteria and does not block Task 008, whose task record explicitly establishes its position. It remains an unresolved documentation issue; do not silently treat the segmentation paragraph as current or repair it within Task 008.

## Boundaries and exclusions

- Pass 3 source-based translation review is closed; do not reopen it merely because Phase 5 finds a usability or technical concern.
- Do not modify Chinese source files or English translation targets during orchestration.
- Do not treat source-faithful translation as proof of current technical correctness.
- Current upstream compatibility, real-device behaviour, safety validation and operational execution are not established by this traversal unless separately and explicitly scoped.
- Do not treat the proposed context-segmented execution arrangement as validated merely because tasks and handoffs exist.
- Preserve the distinction between task completion, orchestrator acceptance, Pass 4 closure and overall project completeness.

## Next position

The next supportable hop is Task 008. It validates the entrypoint's references against the pinned English tree without requiring the worker to load every document's contents. The worker must report missing or ambiguous dependencies and stop rather than silently broadening the task.
