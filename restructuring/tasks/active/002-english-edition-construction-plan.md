# Task 002 — English edition construction plan and path-ownership map

**Status:** READY  
**Workstream:** English edition restructuring  
**Task type:** Read-only planning and evidence reconciliation  
**Authority:** Orchestrator-issued  
**Target repository:** `incus-skills/android-mainline-port`  
**Ingress branch:** `translation/english-reconstruction`  
**Ingress revision:** `90866b5baa5f8946e2b61596f99cc411066fc607`

## 1. Purpose

Prepare an exact, reviewable construction plan for the agreed English edition branch without creating that branch or changing any product files.

Task 001 established the repository baseline and was accepted by the orchestrator. The project owner has separately agreed to:
- retain skill identity `android-mainline-port`;
- establish a dedicated English edition branch;
- place the operational English skill at the normal root-level package location;
- keep validation tooling and fixtures on a separate validation-harness branch;
- retain reconstruction records on the existing reconstruction branch.

This task translates those decisions and Task 001's evidence into a path-level construction proposal. It must expose collisions, dependencies and unresolved choices rather than resolving them by assumption.

## 2. Evidence inputs

Use the exact ingress revision above and verify the relevant pins before relying on them:

- Accepted Task 001 report: `restructuring/tasks/completed/001-baseline-inventory.md`.
- Task 001 acceptance: `restructuring/acceptance/001-baseline-inventory-acceptance.md`.
- Candidate upstream source commit: `5aa6297f40166bec966194d46795c4fd16b533db`.
- Candidate source tree SHA: `58a97a59ded09e79c15e2e1a081907ef96581a34`.
- English package tree: `translation/en/`, tree SHA `ec1ab0b8db21735d4905825872ac32dea2f5be11`.
- English entrypoint: `translation/en/SKILL.md`, blob SHA `b8c4e4e8e625552f9fd4dbd44e613f54ef359aec`.
- English repository guide: `README.en.md`, blob SHA `f099921d88bc8b5f138250fa546ed4287d92aadb`.
- Source-language marketplace: `.claude-plugin/marketplace.json`, blob SHA `abdaaf3d1763db8108b3895b542088f201b3af16`.
- Root licence: `LICENSE`, blob SHA `333b4f81388f8b3e2e40e0b8677bb9e103e809a9`.
- Relevant accepted assessment: `translation/tasks/completed/017-phase5-english-distribution-package-assessment.md`.

If any pin does not match at ingress, stop and report the mismatch. Do not silently substitute newer content.

## 3. Required analysis

### A. Candidate base comparison

- Verify the candidate upstream commit and its tree identity.
- Compare it with the fork's source-language `main` tree and the reconstruction branch's source-root paths.
- Identify whether the evidence supports using the candidate upstream commit as a clean starting point for an English-only product branch.
- Record any differences that would make a different base necessary.
- Do not select or change the base commit. The owner retains that decision.

### B. Exact English package promotion map

For all 22 files under `translation/en/`, produce an explicit source-path → proposed destination-path mapping.

- The proposed destination must place the package at the repository root.
- Preserve the declared skill identity and the complete internal relative-reference structure.
- Identify all source-root paths that collide with promoted English paths.
- For each collision, state whether the path would be inherited from the candidate base, replaced in the English branch, or requires a separate owner decision.
- Do not perform any copy, move, deletion or rewrite.

### C. Root-level supporting material

Assess the treatment required for:
- `README.en.md` (including its self-referential `[中文](README.md)` link and Chinese community content);
- `README.md` from the source baseline;
- `LICENSE`;
- `.claude-plugin/marketplace.json`;
- `docs/assets/gzh-qr.png` and `docs/assets/xhs-qr.jpg`;
- any other root-level files required for a complete, coherent English edition.

Distinguish:
1. necessary package content;
2. repository-level distribution/support content;
3. source-language-only content;
4. reconstruction-only content;
5. material requiring owner decision.

Do not delete or rewrite anything. Where a change would be necessary, describe the proposed change and its reason.

### D. Marketplace and provisioning boundary

- Reconcile the existing marketplace behavior with the dedicated English edition branch.
- Identify whether the current marketplace manifest should remain untouched on the reconstruction/source branch.
- Describe the minimum English-branch metadata needed for its intended consumer route, but do not author it.
- The external provisioning workflow is not established by repository evidence. State exactly what evidence would be needed to validate a locator/ref and do not invent the workflow.

### E. Construction and validation sequence

Produce an ordered, minimal sequence for a later implementation worker:
- branch creation from the owner-selected base;
- package promotion and collision resolution;
- README/licence/assets/marketplace treatment;
- static package validation;
- independent provisioning validation;
- handoff to the separate validation-harness workstream.

For every step, identify prerequisites, expected changed paths and validation evidence. Keep product changes separate from harness-only changes.

## 4. Required output

Create:

`restructuring/tasks/completed/002-english-edition-construction-plan.md`

The report must contain:

1. Ingress identity and verified evidence pins.
2. Candidate-base comparison and unresolved base decision.
3. Complete 22-file path mapping.
4. Root-path collision and ownership table.
5. README, licence, asset and marketplace treatment analysis.
6. External provisioning evidence gap.
7. Ordered implementation sequence with prerequisites and expected path changes.
8. Risk register and explicit owner decisions still required.
9. Explicit non-actions and handoff.

Every material finding must cite exact paths and, where possible, blob/tree SHA or line ranges. Mark inference as inference. Do not claim a proposed layout has been implemented or runtime-tested.

## 5. Scope restrictions

This task is read-only except for writing the single required handoff report.

The worker MUST NOT:
- create, update, rename or delete branches;
- copy, move, rename, delete or rewrite product files;
- modify README, licence, marketplace or package metadata;
- create validation scripts, fixtures, CI workflows or provisioning configuration;
- select the final upstream base commit;
- decide unresolved owner choices;
- modify translation/reconstruction records, accepted Task 001 report or acceptance record;
- change the translation workstream's current position or resume translation tasks;
- introduce OE terminology or architecture into the Android project.

## 6. Stop conditions

Stop and report if:
- any required evidence pin differs;
- the candidate source tree cannot be retrieved or compared;
- the full 22-file English package inventory cannot be verified;
- the exact destination mapping cannot preserve the package's relative references;
- a required packaging choice cannot be assessed without owner input.

## 7. Completion and handoff

Worker completion does not constitute acceptance.

The handoff must state:
- exact ingress commit;
- exact report path and resulting commit SHA;
- commands/repository views used;
- any incomplete areas and reasons;
- confirmation that no branches or product/harness files were changed.

Return control to the orchestrator. Do not issue Task 003 or perform any implementation operation.
