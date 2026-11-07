# Restructuring current position

**Workstream:** Android Mainline Port repository restructuring  
**Branch:** translation/english-reconstruction  
**Position date:** 2026-11-07  
**Status:** Governing objective recorded; task lifecycle reconciliation required before further worker dispatch.

## Governing objective

Restructure the repository to support independently identifiable and operational source-language and English editions, while establishing a sustainable workflow for maintaining their functionality, ingress procedures, provenance and relationship over time.

The English edition is a projection of upstream from a particular observer position at a particular point in time. Upstream changes perturb that projection and initiate a new reconstruction traversal when necessary.

The complete owner-approved objective and required outcomes are recorded in [README.md](README.md). That file is authoritative; this record carries only the current position.

## Owner-approved direction

Approved on 2026-11-07:

- Maintain independent source-language and English editions.
- Establish a dedicated English-edition branch with its operational package at the normal package location, subject to the remaining explicit decisions and evidence.
- Keep reconstruction history and coordination records separate from operational product content.
- Keep validation tooling and fixtures separate from the operational product on the separately intended validation-harness branch.
- Establish maintained orchestrator/worker ingress, task, acceptance and session-continuity procedures.
- Establish a repeatable upstream-to-English update workflow based on reconstruction, not blind merge or automatic translation.
- Preserve provenance and the English edition's own experience and continuity.
- Translation work remains on hold while restructuring takes precedence.

Do not infer the final English branch name, stable ref, base commit, supporting-file treatment, marketplace coexistence or provisioning route. These remain explicit decisions/evidence dependencies.

## Accepted restructuring work

### Task 001 — Baseline inventory and provenance assessment

- Worker report: restructuring/tasks/completed/001-baseline-inventory.md.
- Orchestrator acceptance: restructuring/acceptance/001-baseline-inventory-acceptance.md.
- Disposition: ACCEPTED.
- Scope: read-only baseline inventory and provenance assessment.
- It did not authorise product changes or select the final English branch/base.

### Task 002 — English edition construction plan and path-ownership map

- Worker report: restructuring/tasks/completed/002-english-edition-construction-plan.md.
- Orchestrator acceptance: restructuring/acceptance/002-english-edition-construction-plan-acceptance.md.
- Worker report commit: 864afe64a505503abbf72066b68ff5709518c2bf.
- Acceptance commit: 910bb9a2145999a694d1d02f1a4dc5c623c0b74f.
- Disposition: ACCEPTED.
- Scope: read-only English-edition construction plan and path-ownership assessment.
- The accepted evidence identifies yzddmr6/android-mainline-port@5aa6297f40166bec966194d46795c4fd16b533db as a strong candidate base, not an owner-selected final base.
- The report maps 22 English package files to root-level destinations and identifies all 22 collisions with source-language package files.
- It does not create the English branch, change product files, establish provisioning, or validate runtime operation.

## Current repository observations and reconciliation needs

At the accepted Task 002 state, the restructuring task tree contained:

    restructuring/
    ├── acceptance/
    │   ├── 001-baseline-inventory-acceptance.md
    │   └── 002-english-edition-construction-plan-acceptance.md
    └── tasks/
        ├── active/
        │   ├── 001-baseline-inventory.md
        │   └── 002-english-edition-construction-plan.md
        └── completed/
            ├── 001-baseline-inventory.md
            └── 002-english-edition-construction-plan.md

Both tasks have acceptance records, while their task definitions remain in active/ and worker handoffs are in completed/. This is an observed discrepancy, not an established lifecycle rule.

The Task 002 acceptance record contains a paragraph claiming that retaining accepted task definitions in active/ is consistent with Task 001 precedent. That reasoning was subsequently challenged and withdrawn: precedent alone does not establish policy. The acceptance record needs a bounded correction/reconciliation; preserve the worker handoff unchanged.

The translation workstream provides the structural reference for the lightweight task lifecycle. Its orchestrator ingress specifies pending task records, worker-appended handoffs, movement of the completed task record to tasks/completed/, and independent orchestrator acceptance. Restructuring must reconstruct that actual arrangement and align its own task lifecycle deliberately; do not assume the current active/ directory arrangement is correct or move files before the lifecycle rule is recorded.

No restructuring-level orchestrator ingress, worker ingress or current-position record existed before this update. This documentation update establishes those entry points. It does not yet reconcile the existing task records or prove the procedures operational.

## Next required operation

Before issuing another product or construction task, perform a bounded restructuring workflow and task-lifecycle reconciliation:

1. Compare the translation orchestrator and worker ingress procedures and the actual translation task-record transitions.
2. Establish the corresponding intended restructuring task paths and lifecycle, including how acceptance is recorded and how accepted task definitions cease to appear pending.
3. Reconcile Task 001 and Task 002 placement against that rule.
4. Correct the unsupported lifecycle rationale in the Task 002 acceptance record without rewriting its worker handoff or changing the accepted technical findings.
5. Update this current-position record and the restructuring ingress documents to match the verified procedure.

Do not treat this list as permission to change unrelated product files. The next task must define exact paths, revisions, allowed changes and validation.

## Remaining accepted-plan dependencies

After workflow reconciliation, continue the construction work only through bounded tasks that resolve the dependencies identified by Task 002:

- owner selection of the final base commit and English branch name/stable ref;
- treatment of root README, marketplace manifest, licence and community assets;
- actual external provisioning/discovery workflow and package locator;
- branch creation and exact 22-file package promotion, with source-language edition preserved;
- validation of package structure, provisioning/discovery and operational function;
- separate validation-harness branch and its relationship to the operational product;
- repeatable upstream-change assessment and English-edition update process.

Do not invent missing external workflow evidence or collapse these into one broad implementation task.

## Session continuation rule

A fresh orchestrator session should read only:

1. README.md — governing objective;
2. this file — current position;
3. ORCHESTRATOR-INGRESS.md — orchestration procedure;
4. the single active task identified by the reconciled task state.

A worker should read WORKER-INGRESS.md, the exact assigned task and only its pinned evidence packet.

Do not re-read all completed reports, translation checkpoints or the operational package unless the assigned task explicitly depends on them. This is a context-control rule, not permission to omit necessary evidence. If an evidence dependency is discovered, retrieve that dependency and record why it is necessary.

## Non-actions and limits

This continuity update records the approved objective and establishes restructuring ingress. It does not:

- move or delete Task 001 or Task 002 records;
- modify the existing acceptance records or worker reports;
- create the English-edition or validation-harness branch;
- modify source-language or English operational package files;
- establish or test an external provisioning workflow;
- resume translation;
- establish that the new ingress procedures have been validated through repeated operation.
