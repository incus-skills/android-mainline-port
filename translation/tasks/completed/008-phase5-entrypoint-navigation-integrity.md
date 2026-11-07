# Task 008 — Phase 5 entrypoint and navigation integrity

## Purpose and observer position

Perform the first bounded operation in Pass 4 — whole-skill English validation. Determine whether the English skill entrypoint's documented navigation and file references resolve against the pinned English tree, and whether the entrypoint's stated reading routes are structurally followable.

This is a read-only structural validation. It is not another Pass 3 translation review, a semantic rewrite, a technical audit, or an execution of representative workflows.

## Position and governing criteria

- Traversal: **Pass 4 — whole-skill English validation**.
- Project phase: **Phase 5 — Whole-skill validation**, as defined in `translation/README.md`.
- Current position: Pass 4 / Checkpoint 01.
- Previous traversal: Pass 3, closed at Checkpoint 23. Pass 3 source-based review completion does not establish Phase 5 usability.
- Pass 4 declaration: `reconstruction/zh/continuation/checkpoints/pass-4-checkpoint-01.md`, blob SHA `598722e61e115068af5c90d90dbdf8224dd7c764`.
- Current-position record before this task was prepared: `reconstruction/zh/continuation/current-position.md`, blob SHA `2f02b450881f5b0a4381eb56132a404ef024fb52`.
- Pass 3 closure: `reconstruction/zh/continuation/checkpoints/pass-3-checkpoint-23.md`, blob SHA `2f356c06db43c84e5112c7a23c4bb51240c55563`.
- Phase definitions and Phase 5 criteria: `translation/README.md`, blob SHA `f9bf34ae28e838ae881f9e207797f26c1b327baa`.
- Translation inventory: `translation/INVENTORY.md`, blob SHA `0a8e58e9a6f45fd54c718672dc8d7a3a9056f7ee`.
- Execution boundary and eight handoff fields: `translation/EXECUTION-SEGMENTATION.md`, blob SHA `05ee569421b89366bbc0993afe62cb22a0644913`.
- English entrypoint: `translation/en/SKILL.md`, blob SHA `f613aa7ef89eb9204c4f9804faf7aebace596a4e`.
- Pinned English tree: `translation/en/`, tree SHA `ba8a19206173ccde8fd26b6ae731ca4a0ee8c7cc`.
- Supporting subtree SHAs: `translation/en/references/` = `41f3799c0dd0caa7bdef8e9b4820f45f21ace473`; `translation/en/experience/` = `d8d0db60cb0ef7dafd527f1d5302683bb3fbbb93`.

## Known position discrepancy in the execution-arrangement document

The pinned `translation/EXECUTION-SEGMENTATION.md` contains an “Application to the current position” paragraph naming Pass 3 / Checkpoint 16 / `experience/wireless.md`. That paragraph is stale and conflicts with the canonical current-position record and Pass 4 / Checkpoint 01. The stale paragraph predates the current segmentation revision; the newer revision added evidence-boundary and separate-investigation-proposal guidance but did not update the embedded position.

Use the current-position record and the active checkpoint to establish position. Do not use the stale paragraph to override this task's observer position. This discrepancy is outside Task 008's skill-entrypoint scope; do not edit the segmentation document. Report only if it materially prevents execution of this task.

## Exact evidence boundary

Use the complete pinned `translation/en/SKILL.md` and the pinned `translation/en/` Git tree. Use the inventory and translation README only to establish the intended English file mapping and Phase 5 criteria.

The tree can be enumerated from the pinned tree object, for example through the GitHub Git Trees endpoint for tree SHA `ba8a19206173ccde8fd26b6ae731ca4a0ee8c7cc`. Do not substitute a moving branch-head tree without recording and reconciling the revision.

Do not load every referenced document's body merely to prove that its path exists. Fetch a target's content only if a specific ambiguity cannot be resolved from the tree metadata and report why that additional evidence was necessary.

## Scope of work

1. Read the complete pinned English `SKILL.md`.
2. Extract its navigation routes, including:
   - new-project entry;
   - taking over an existing project;
   - making a technical decision;
   - troubleshooting;
   - assessment-only use;
   - the process-stage table and its main references;
   - the file index and maintenance instructions.
3. Enumerate the pinned English tree and verify that every file path presented as an intended English document reference in the entrypoint resolves to the expected file.
4. Distinguish actual Markdown links from literal paths, inline-code examples, glob patterns and directory references. Do not report a literal example as a broken link without establishing that it is intended as a navigable dependency.
5. Compare the entrypoint's stated routes and file index with the inventory's English mapping. Report missing, ambiguous, stale or inconsistent destinations with exact source location and tree evidence.
6. Assess only whether the entrypoint's declared reading order is structurally followable from the referenced files. Do not assess the technical content or semantic fidelity of those files; Pass 3 addressed source-based translation review.
7. Report any dependency that cannot be resolved from the pinned evidence and stop rather than broadening the scope.

## Exclusions

- Do not re-open Pass 3 semantic translation review.
- Do not assess current Android/Linux technical correctness, upstream currency, real-device behaviour, operational safety or installation compatibility.
- Do not execute new-device assessment, project continuation, technical-decision or troubleshooting workflows. These are separate Phase 5 operations.
- Do not validate plugin/skill platform discovery or installation metadata in this task.
- Do not redesign the skill, rewrite navigation, or infer missing dependencies from domain knowledge.
- Do not treat a path's existence as proof that its content is correct or its workflow is usable.
- Do not treat this single operation as completion of Pass 4.

## Permitted changes

- This is read-only with respect to skill content.
- Do not modify `SKILL.md`, any file under `translation/en/`, source documents, inventory, translation instructions, glossary, metadata, current-position or checkpoint records.
- Append the required eight-field handoff to this task record only.
- If a defect is found, report it with evidence; do not repair it. The orchestrator will decide whether a separate corrective task is supportable.

## Stop conditions

Stop and report if:
- the pinned entrypoint or tree revision cannot be retrieved;
- the tree is incomplete or cannot establish whether a destination exists;
- a reference depends on document content rather than path existence and that content is outside the evidence boundary;
- a reading route cannot be assessed without loading a wider dependency set;
- the task would require semantic translation review, technical validation, workflow execution or metadata compatibility assessment.

## Validation

- Confirm the fetched entrypoint blob SHA is exactly `f613aa7ef89eb9204c4f9804faf7aebace596a4e`.
- Confirm the enumerated tree corresponds to `translation/en/` tree SHA `ba8a19206173ccde8fd26b6ae731ca4a0ee8c7cc`.
- Record each checked path, whether it resolves, and whether it is a Markdown link, an explicit navigation path, or a literal/example reference.
- Reconcile the intended destinations against the inventory mapping.
- Preserve the distinction between structural existence, reading-order coherence, semantic correctness and actual workflow usability.
- Record all evidence revisions, unresolved matters and the read-only change boundary.

## Required handoff

Append a reconstructable handoff to this task record using the eight fields in `translation/EXECUTION-SEGMENTATION.md`:

1. Task identity.
2. Observer position.
3. Observations.
4. Decisions.
5. Unresolved matters.
6. Validation.
7. Change boundary.
8. Next dependency.

State whether the task was completed, stopped for an adequacy reason, or requires additional evidence. Moving this task to `translation/tasks/completed/` records worker execution only; it does not constitute orchestrator acceptance.

For the initial trial, only one worker should select and execute a task at a time. No locking or claim mechanism is assumed.


---

## Worker handoff — Task 008

### 1. Task identity

- **Execution status:** COMPLETED — read-only structural validation and handoff completed; orchestrator acceptance remains separate.
- **Traversal / checkpoint:** Pass 4 / Checkpoint 01; Phase 5 — Whole-skill validation.
- **Task:** `translation/tasks/008-phase5-entrypoint-navigation-integrity.md`, blob SHA `8a5bdc0ed4ee83c75ab42977bc55009de68a8224`.
- **Entrypoint:** `translation/en/SKILL.md`, blob SHA `f613aa7ef89eb9204c4f9804faf7aebace596a4e`.
- **Pinned English tree:** `translation/en/`, tree SHA `ba8a19206173ccde8fd26b6ae731ca4a0ee8c7cc`.
- **Supporting trees:** `references/` = `41f3799c0dd0caa7bdef8e9b4820f45f21ace473`; `experience/` = `d8d0db60cb0ef7dafd527f1d5302683bb3fbbb93`.

### 2. Observer position

Determine whether the English entrypoint's documented navigation and file references resolve against the pinned English tree, and whether the declared reading routes are structurally followable. This is a read-only structural check. It does not reopen Pass 3 translation review, assess technical correctness, or execute representative workflows.

### 3. Observations

- The pinned Git tree was retrieved by its exact tree SHA. GitHub returned the same SHA and `truncated: false`; it contains 22 blobs: `SKILL.md`, six files under `references/`, and fifteen Markdown files under `experience/`.
- The entrypoint's process-table and file-index references to English documents all resolve in that tree. The inventory maps the entrypoint, all six framework references and all fifteen experience documents to corresponding English targets; every mapped target is present.
- The new-project route (line 17), technical-decision route (line 19), troubleshooting route (line 20), and assessment-only route (line 21) point to existing process/decision/pattern material. All explicit main-reference destinations in the Stage 0–8 table (lines 27–35) resolve.
- The supporting references named elsewhere in the entrypoint also resolve: `references/discipline.md` (lines 35, 52, 98), `references/verification.md` (lines 58, 101), and `references/build-environment.md` (lines 66, 99). The distribution-specific reference `references/ubuntu.md` resolves (lines 13, 32, 34, 102).
- The experience destinations resolve: `experience/patterns.md` (lines 20, 103), `experience/README.md` (lines 104, 109), `experience/usb-and-storage.md` (lines 29, 32), `experience/bootloader-and-boot.md` and `experience/kernel-and-resources.md` (line 30), `experience/display.md`, `experience/touch-and-input.md`, and `experience/gpu.md` (line 31), and `experience/build-and-release.md` (line 34).
- The `experience/` directory reference (lines 17, 20, 33) resolves. The `experience/*.md` glob (line 105) expands to all fifteen Markdown files in the subtree: thirteen domain documents plus `README.md` and `patterns.md`, which are also separately indexed.
- There are no Markdown link constructs in the entrypoint. Document destinations are expressed as inline-code paths; directory references and the glob are likewise literal navigation references, not Markdown links.
- `AGENTS.md` appears at lines 18 and 82. The wording explicitly identifies it as the existing project's own file, not an English skill document. It is not in the pinned skill tree, so its presence is project-local and cannot be established by this tree check. The status table and technical-debt table in the takeover route are likewise project-local artifacts, not mapped English skill files.
- The task's cited prior current-position revision `2f02b450881f5b0a4381eb56132a404ef024fb52` was retrieved and confirms the pre-Pass-4 Pass 3 / Checkpoint 23 closure. The current-position file now has blob SHA `ece75385e002774b70b6dca1999753e04097de40` and records Pass 4 / Checkpoint 01 / Task 008. This is a subsequent control-position update, not a mismatch in the pinned skill evidence.
- The known stale “Application to the current position” paragraph in the execution-arrangement document remains outside this task's entrypoint scope. The active checkpoint and current-position record establish the correct Pass 4 position.

### 4. Decisions

- No missing or stale English document destination was found in the entrypoint's process routes or file index.
- Treat `AGENTS.md`, the status table and the technical-debt table as conditional project-local dependencies, not broken English-tree references. The existing-project route is structurally followable when those project records are supplied; their existence is not guaranteed by the skill tree.
- Record the `experience/*.md` description as a minor scope ambiguity: the glob includes the separately indexed README and patterns index as well as the thirteen domain documents. Its expansion is unambiguous and all matches exist; no repair is made.
- No target or skill-content modification is justified or permitted by this task.

### 5. Unresolved matters

- Whether a particular adopting project has an `AGENTS.md`, status table and technical-debt table is outside the pinned English-tree evidence. No project instance was supplied, and this task does not execute the takeover workflow.
- The glob's descriptive label could be read as applying only to the thirteen domain documents even though the literal glob also matches the README and patterns index. This is a minor index-description ambiguity, not a missing destination.
- Structural path existence does not establish the referenced documents' semantic correctness or actual workflow usability; those are expressly outside this operation.

### 6. Validation

- **Entrypoint blob:** exact match, `f613aa7ef89eb9204c4f9804faf7aebace596a4e`.
- **Tree:** exact match, `ba8a19206173ccde8fd26b6ae731ca4a0ee8c7cc`; complete, not truncated; 22 file blobs.
- **Subtrees:** exact matches for `references/` (`41f3799c0dd0caa7bdef8e9b4820f45f21ace473`) and `experience/` (`d8d0db60cb0ef7dafd527f1d5302683bb3fbbb93`).
- **Governing references:** `translation/README.md` = `f9bf34ae28e838ae881f9e207797f26c1b327baa`; `translation/INVENTORY.md` = `0a8e58e9a6f45fd54c718672dc8d7a3a9056f7ee`; Pass 4 / Checkpoint 01 = `598722e61e115068af5c90d90dbdf8224dd7c764`; Pass 3 / Checkpoint 23 = `2f356c06db43c84e5112c7a23c4bb51240c55563`; `translation/EXECUTION-SEGMENTATION.md` = `05ee569421b89366bbc0993afe62cb22a0644913).
- **Reference classification:** all intended English document destinations were checked against the enumerated tree; all resolve. `AGENTS.md` is recorded separately as a project-local path. The directory and glob references were checked by tree expansion. No Markdown links were present.
- **Reading-order result:** the entrypoint's declared routes are structurally followable from the pinned English tree, subject to the explicitly project-local records required for existing-project takeover. This is not a finding that the workflows have been executed or are operationally usable.
- **Phase distinction:** structural existence and reading-order coherence were assessed; semantic correctness, current technical validity and actual workflow usability were not assessed.

### 7. Change boundary

- Appended this handoff to `translation/tasks/008-phase5-entrypoint-navigation-integrity.md` only.
- No files under `translation/en/` were changed. No source files, inventory, README, glossary, metadata, current-position record, checkpoint or execution-arrangement document was changed.
- The task remains in `translation/tasks/`; no move to `completed/` was made. Worker completion does not constitute orchestrator acceptance.

### 8. Next dependency

No additional evidence is required to complete this bounded structural check. The orchestrator should independently reconstruct and evaluate this handoff before deciding whether Task 008 is accepted. The project-local takeover artifacts, representative workflow execution, cross-document validation and metadata compatibility require separate evidence and separately bounded operations; they were not performed here.
