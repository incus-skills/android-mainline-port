# Restructuring orchestrator session ingress

## Purpose

This is the entry point for a fresh orchestration session coordinating the Android Mainline Port repository restructuring workstream.

The governing objective is in [README.md](README.md). The current accepted position and next operation are in [current-position.md](current-position.md). These records exist to prevent each new session from re-reading the repository or reconstructing the overall goal from conversation history.

This ingress is specific to restructuring. Translation remains a separate, currently suspended workstream.

## Required entry sequence

1. Read [the governing objective](README.md).
2. Read [the current restructuring position](current-position.md). Treat it as the current session ingress and verify its branch/revision references.
3. Read this ingress and [Worker session ingress](WORKER-INGRESS.md) when preparing or evaluating worker operations.
4. Read only the active task record identified by current-position or the task-selection procedure below.
5. Retrieve only the evidence explicitly required by that task. Verify its pinned revisions. Do not re-read the whole repository or historical task sequence by default.
6. Consult the translation workstream's corresponding ingress/lifecycle records only when establishing structural correspondence or resolving a specific dependency. Do not import translation-specific evidence rules into restructuring tasks unless they are independently applicable.

If the current-position record is missing, stale, contradictory or cannot be verified, reconstruct only the minimum repository state needed to repair it. Record the limitation rather than silently assuming a position.

## Orchestrator responsibilities

Maintain a reconstructable account of:

- the governing restructuring objective and owner-approved constraints;
- current repository and branch position;
- task issued, worker handoff and independent acceptance state;
- decisions made, their authority and evidence;
- unresolved owner decisions, external dependencies and validation gaps;
- the next bounded operation and the exact evidence it needs.

The orchestrator coordinates and adjudicates. It should not perform the file-level work assigned to a worker merely because the relevant repository material is available.

## Task lifecycle and structural correspondence

The translation workstream is the reference for the lightweight context-segmented task process. In particular, inspect its actual task lifecycle before defining or changing restructuring task-directory semantics.

The translation orchestrator ingress describes pending task records, worker-appended handoffs, movement to tasks/completed/, and the distinction between worker completion and orchestrator acceptance. These are useful structural references; translation-specific source-evidence rules do not automatically govern restructuring.

The restructuring tree currently contains tasks/active/, tasks/completed/ and acceptance/. This is an observed arrangement, not yet a fully reconciled lifecycle specification. Task 001 and Task 002 have both been accepted while task definitions remain under active/ and handoffs are under completed/. Do not treat that duplication as an established policy or perpetuate the earlier acceptance-record explanation that inferred policy from Task 001 precedent.

A bounded lifecycle-reconciliation operation must establish the exact correspondence with the translation workflow, determine the intended restructuring paths, and correct the affected records without rewriting worker handoffs. Until that is done:
- do not move, delete or rewrite task records by assumption;
- do not issue work from an ambiguous task state;
- preserve the separate worker handoff and orchestrator acceptance records;
- record the observed discrepancy in current-position.

## Independent acceptance

Worker completion is not acceptance. For every completed worker operation:

1. Reconstruct the task, its exact evidence revisions, permitted scope, changed paths and handoff.
2. Independently verify material claims and validation against repository evidence.
3. Record one explicit disposition: ACCEPTED, REJECTED or UNRESOLVED.
4. Integrate accepted work into current-position only after acceptance.
5. Preserve worker handoffs as worker-authored records. Record orchestrator adjudication separately.
6. Do not infer overall restructuring completeness from acceptance of individual tasks.

Acceptance is limited to the task's declared scope. It does not silently authorise downstream implementation, branch creation, product changes or resumption of translation.

## Context and evidence discipline

- Use README and current-position as the durable objective and continuity boundary. Do not depend on prior conversation or compressed-session recollection.
- Do not make every worker read all prior reports. Put exact relevant evidence references and revisions in the task.
- Prefer commit/tree/blob identities over unpinned branch-relative assumptions.
- Keep observed facts, owner decisions, reconstructed relationships and unresolved matters distinct.
- When a task reveals a new dependency, update the task or issue a separate bounded task; do not expand scope silently.
- Update current-position after every accepted operation and whenever the next supportable operation or a material dependency changes.
- Update ingress records when the actual workflow changes. Do not allow procedural documentation to drift from repository practice.
- A context summary is not a substitute for repository records. A fresh session must be able to recover the objective and position from these files alone.

## Prepare bounded work

Before issuing a task, establish:

- why the operation is the next supportable step toward the governing objective;
- exact input paths and pinned revisions;
- worker observer position, purpose and scope;
- permitted and prohibited modifications;
- evidence and validation requirements;
- stop conditions and unresolved dependencies;
- the expected reconstructable handoff.

Do not dispatch work when the task lifecycle or required evidence is materially ambiguous. First issue a bounded operation to resolve that ambiguity.

## Boundaries

- Do not modify the source-language or English operational package from the orchestrator session unless a task explicitly assigns that work.
- Do not create or update the dedicated English branch until the required owner decisions and provisioning/discovery evidence are established and a bounded implementation task authorises it.
- Do not treat the candidate upstream commit as the final base until the owner decision is recorded.
- Do not invent the English branch name or stable consumer-facing ref.
- Do not resume translation tasks while the restructuring hold remains.
- Do not claim that a workflow is operational merely because its documentation exists.
- Do not claim that context segmentation is effective merely because ingress and continuity records exist; preserve operational evidence of its use.

## Expected result of ingress

Return a concise reconstruction of:
1. the governing objective;
2. current accepted position and provenance;
3. active task and its evidence boundary, or the reason no task can safely be issued;
4. unresolved decisions and dependencies;
5. the next bounded operation.

Keep the response compact. The repository records, not a repeated full-repository summary, carry the durable detail.
