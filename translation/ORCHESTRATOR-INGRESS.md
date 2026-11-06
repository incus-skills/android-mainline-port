# Orchestrator session ingress

## Purpose

This is the repository entry point for a **fresh ChatGPT orchestration session** participating in the context-segmented execution experiment.

The session is the coordinating observer for the Android Mainline Port translation work. It reconstructs the project position, determines what bounded operation is supportable, and prepares work for an independent worker session.

This ingress is an experimental ChatGPT-session adaptation. It does not replace, merge with, or redefine the existing harness-oriented agentic segmentation strategy. The project-specific arrangement is described in [Context-segmented execution](EXECUTION-SEGMENTATION.md).

## Entry sequence

Read the following records from the `translation/english-reconstruction` branch:

1. [Current project position](../reconstruction/zh/continuation/current-position.md). Treat this as the current translation-work ingress. Follow its current reading sequence rather than relying on a remembered checkpoint or document position.
2. The current checkpoint and any supporting records identified by that ingress. Do not assume Checkpoint 16 or any other checkpoint remains current.
3. [Context-segmented execution](EXECUTION-SEGMENTATION.md). Read it as the proposed project-level execution arrangement, not as a change to translation criteria or as proof that segmentation is effective.
4. [Translation working principles and phases](README.md), particularly the current pass purpose and applicable review phase.
5. [Controlled glossary](GLOSSARY.md), when preparing a task whose work depends on terminology.

If the current-position record points to a newer or different sequence, follow that sequence. Verify the relevant branch, file paths and revisions before relying on them.

## Orchestrator position

Reconstruct and report, with provenance where available:

- current pass, purpose, declared scope and observer position;
- latest completed checkpoint and document, and the next document or operation;
- why the next operation is the supportable next hop;
- global constraints and exclusions that must travel with a worker task;
- established dependencies, suspected relationships and unresolved matters;
- exact evidence a bounded worker will need, and what can remain unloaded unless requested;
- any conflict between the repository records or any state that cannot be reconstructed.

## Independent acceptance and adjudication

Acceptance is an explicit responsibility of the orchestrator. It is not performed by the worker, implied by a task's move to `completed/`, or established merely because the worker reports successful validation.

For every completed worker operation, independently reconstruct the handoff from repository evidence before integrating it:

1. **Reconstruct:** retrieve the completed task, its stated evidence revisions, governing references, changed artifacts and relevant repository history. Verify the evidence against the recorded baselines.
2. **Evaluate:** compare the worker's account with the actual source, target, review record and changes. Assess source correspondence, provenance, permitted scope, preservation of prior records, validation and unresolved matters.
3. **Adjudicate:** record one of three dispositions:
   - **ACCEPTED** — the bounded operation is sufficiently evidenced, within scope and suitable for integration into accepted project continuity.
   - **REJECTED** — available evidence establishes a material defect or boundary violation that prevents acceptance. State the basis and the corrective or recovery action, if one is supportable.
   - **UNRESOLVED** — evidence is incomplete, conflicting or inadequate to decide. Identify what further reconstruction or bounded investigation is needed.
4. **Integrate:** only an **ACCEPTED** operation may be incorporated into the accepted project position and followed by the corresponding continuity reconciliation.

Do not convert a rejected or unresolved operation into accepted progress by advancing the checkpoint, current position or review inventory. Preserve the worker's completion record and the orchestrator's separate adjudication; do not rewrite the worker's disposition to imply acceptance.

Acceptance is scoped to the assigned operation and its stated validation boundary. For example, accepting a Pass 3 translation review establishes that the translation changes are sufficiently supported by the Chinese source and comply with the review task. It does not certify underlying Android technical claims when external technical validation was excluded.

Acceptance is also distinct from overall pass or project completeness. Do not infer either from a sequence of accepted file operations. Whole-pass and receiving-state readiness require their own appropriately scoped reconstruction and assessment.

## Evidence boundary and investigation proposals

The orchestrator must preserve the same evidence-admissibility boundary as the worker. For source-grounded Pass 3 work, claims, distinctions, technical tokens, qualifications and proposed corrections must be traceable to explicit source or target evidence. External knowledge, inferred qualifications and plausible domain explanations cannot establish translation findings. Before requesting or accepting a correction, identify the exact source/target evidence that necessitates it. This requirement applies to corrective work as well as the original review; correction does not expand the original evidence boundary.

The orchestrator may separately record investigation proposals arising from its wider observer position, including questions noticed across documents, accepted handoffs or integration. These may concern current upstream behaviour, real-device validation, time-sensitive advice, possible improvements or possible extensions to the skill.

Keep these proposals distinct from:
- source-grounded Pass 3 findings and translation corrections;
- worker completion and orchestrator acceptance;
- accepted project continuity and checkpoint advancement.

A proposal is not a finding, instruction to change the translation, or evidence for acceptance. It identifies a question or opportunity for a later, separately bounded investigation. Preserve its origin and the observation that prompted it, the question to investigate, the external evidence or test needed, and the possible consequence if resolved. Do not perform that investigation implicitly while adjudicating a translation task. If no proposal arises, do not manufacture one.

Worker-originated proposals should remain identifiable as such when integrated; orchestrator-originated proposals should likewise retain their provenance. Do not collapse either into accepted translation conclusions.

## Reconcile continuity records

Only after an operation has been independently **ACCEPTED**, compare the resulting accepted project position against all durable position-bearing records before preparing the next task:

- preserve prior checkpoints as historical records; create the next checkpoint for the newly established position;
- advance `current-position.md` to identify the latest completed document, next document and current reading sequence;
- update the corresponding Pass 3 entries in `translation/INVENTORY.md` so review status does not lag completed work;
- verify that the pending/completed task sequence agrees with those records.

A worker moving its task record to `completed/` records execution, not orchestrator acceptance. The orchestrator must evaluate the handoff first, then reconcile these records as a separate integration action. Do this before preparing or handing off the next worker task. Do not rewrite historical checkpoints or infer review completion from task creation alone.

Distinguish recorded facts, observer-reconstructed relationships and unresolved questions. Repository evidence is the continuity source for this clean session; do not rely on inherited conversation context or silently repair gaps through inference.

## Prepare bounded work

After reconstructing the position, prepare and persist the next supportable worker task in the numbered task sequence below, using the fields in [Context-segmented execution](EXECUTION-SEGMENTATION.md):

- task identity and purpose;
- exact input files and revisions;
- worker observer position and scope;
- applicable constraints, glossary material and exclusions;
- permitted output and modifications;
- validation requirements;
- stop conditions;
- expected reconstructable handoff.

Keep the worker's evidence boundary as small as adequacy permits. Do not pre-resolve the file-specific questions that belong to the worker. Do not assume omitted context is irrelevant: identify a dependency and either include it, request it, or stop if the task cannot be adequately supported.

## Boundaries

- Do not perform the file-level translation or review assigned to the worker.
- Do not modify source or target translation files while acting in the orchestrator role.
- Do not dispatch a worker from this session. The worker must enter independently through its own ingress.
- Use the lightweight task sequence described below; do not expand it into a scheduler, formal task schema, board or assignment system.
- Do not treat the proposed segmentation arrangement as validated because this ingress or its supporting documentation exists.
- Do not equate successful reconstruction of the orchestrator position with successful worker execution or end-to-end delegation.
- Do not adjudicate overall completeness from the orchestrator position. Preserve the evidence and unresolved matters needed for independent assessment.

## Task sequence

Pending tasks are plain, numbered Markdown records in `translation/tasks/`. Assign the next unused sequence number and keep the number as provenance; it does not encode priority, ownership or execution order. A worker enters through [Worker session ingress](WORKER-INGRESS.md), discovers pending work in this directory and selects an available task. The worker appends its reconstructable handoff to the task record. Once execution is complete, move the record unchanged except for the appended handoff to `translation/tasks/completed/`. Completion records execution, not orchestrator acceptance. The orchestrator independently evaluates the handoff and determines subsequent work. Do not introduce locking, status fields or other machinery unless actual use exposes a need.

## Expected result of this ingress

Return:

1. a concise reconstruction of the current project position;
2. the evidence and provenance supporting it;
3. unresolved matters and any adequacy limits;
4. a bounded worker task specification, if a next operation is supportable;
5. any missing repository mechanism that prevents the task from being durably recorded or independently discovered.

Do not broaden the task to solve those missing mechanisms in the same traversal.