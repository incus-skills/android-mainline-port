# Task 001 — Baseline inventory and provenance assessment

**Status:** READY  
**Workstream:** English edition restructuring  
**Task type:** Read-only assessment  
**Authority:** Orchestrator-issued  
**Target repository:** `incus-skills/android-mainline-port`  
**Ingress branch:** `translation/english-reconstruction`  
**Ingress revision:** Resolve the branch HEAD at worker start and record the full commit SHA in the handoff. Do not silently substitute a later revision during execution.

## 1. Purpose

Establish an evidence-backed inventory of the repository state relevant to creating a standalone English edition and a separate validation harness.

This task is deliberately observational. It must establish what exists, where it came from, and what dependencies or ambiguities are visible before any restructuring operation is authorised.

## 2. Agreed architectural boundaries

The following decisions are fixed for this workstream:

1. The English edition retains the skill identity `android-mainline-port`.
2. The English edition is intended to be independently discoverable, provisionable and operable.
3. Its eventual branch should remain structurally as close to upstream as practical, with the operational skill at the normal package location (root-level package layout).
4. Validation tooling, fixtures and test-only material belong in a separate validation-harness branch, not in the operational product merely to support validation.
5. Translation/reconstruction records remain separate from the operational English package.
6. The existing reconstruction history and records must not be deleted or rewritten as part of this task.
7. OE's relationship to the Android project remains open. Do not introduce OE terminology, categories, dependencies or structural requirements into this assessment.

These are boundaries, not permission to implement the restructuring.

## 3. Worker assignment

Perform a read-only assessment of the following:

### A. Repository and branch state

- Identify the current ingress commit and record its full SHA.
- Record the repository default branch and the relevant upstream and reconstruction branches that can be verified.
- Identify existing branches that appear relevant to English translation, reconstruction, validation or distribution.
- Do not create, rename, delete or update branches.

### B. Upstream baseline and provenance

- Identify the upstream repository and its current default branch.
- Identify candidate upstream commits corresponding to the source state from which the accepted English reconstruction was made.
- Compare the reconstruction's source references, revision pins, checkpoints and task records to establish which upstream revision is evidenced, if any.
- Distinguish an evidenced base revision from a plausible or convenient candidate.
- Do not select or declare the final base revision; report evidence and unresolved ambiguity for owner/orchestrator decision.

### C. English package inventory

- Enumerate every file under the current English package tree.
- Record the package entrypoint and metadata, including the declared skill name.
- Identify references to files outside the English package tree.
- Identify links, assets, scripts, templates or other resources whose resolution may depend on the wider reconstruction repository.
- Identify possible language leakage or source-language material, but do not remove or rewrite anything.
- Record any naming/layout constraints relevant to placing the package at repository root.

### D. Reconstruction and coordination inventory

- Locate the current-position record, ingress instructions, task definitions, handoffs, checkpoints and acceptance records relevant to the English reconstruction.
- Identify which records are operationally necessary to continue reconstruction and which are historical evidence.
- Do not move or reclassify records. Report observed locations and dependencies only.

### E. Validation and provisioning evidence

- Inventory existing validation scripts, workflows, fixtures, CI configuration, release configuration and package/distribution metadata.
- Identify documented installation/provisioning paths and the repository paths or refs they currently consume.
- Distinguish actual executable configuration from documentation examples and assumptions.
- Do not create or run validation workflows in this task.

## 4. Required output

Create one handoff report at:

`restructuring/tasks/completed/001-baseline-inventory.md`

The report must contain:

1. **Ingress identity:** repository, branch, full commit SHA, and execution date.
2. **Repository/branch inventory:** observed branches and their apparent roles, with evidence.
3. **Upstream provenance:** candidate base revision(s), evidence, confidence and unresolved questions.
4. **English package inventory:** complete path list, package metadata and dependency/link observations.
5. **Reconstruction record inventory:** relevant record paths and their apparent functions.
6. **Validation/provisioning inventory:** actual files and documented workflows, clearly distinguished.
7. **Risk and ambiguity register:** especially file ownership, package collisions, external dependencies, language leakage and uncertain provenance.
8. **Suggested task dependencies:** what can safely follow, and what requires an owner decision first.
9. **Explicit non-actions:** confirm that no product files, branches, workflows or existing records were changed.

Every material finding must include a repository path and, where possible, a line range, commit SHA, tree entry or other reproducible evidence. Label inference as inference. Do not convert absence of evidence into evidence of absence.

## 5. Scope restrictions

This is a read-only repository assessment except for writing the single required handoff report.

The worker MUST NOT:

- create or update product or harness branches;
- move, rename, delete or rewrite existing files;
- modify the English package, README, metadata or installation instructions;
- add validation scripts, fixtures or CI workflows;
- select the upstream base revision on behalf of the owner;
- alter current-position, checkpoint, task or acceptance records;
- introduce OE into the Android repository architecture;
- claim the English package is independently provisionable without testing it.

If the report cannot be written at the specified path because the repository's established task-record convention requires another location, stop and report the discrepancy rather than creating a competing convention.

## 6. Stop conditions

Stop and report without attempting a workaround if:

- the ingress branch or revision cannot be established;
- the upstream repository identity cannot be verified;
- required source/provenance records are missing or contradictory;
- the English package inventory cannot be completed;
- a finding would require modifying existing material to verify it;
- the required report path conflicts with an established repository convention.

## 7. Completion and handoff

Worker completion does not constitute acceptance.

The handoff must state:

- the exact ingress commit;
- the exact report path and resulting commit SHA;
- the commands or repository views used to establish the inventory;
- any incomplete areas and why they remain incomplete;
- confirmation of all scope restrictions and non-actions.

Return control to the orchestrator for independent review. Do not issue Task 002 or perform any subsequent restructuring operation.
