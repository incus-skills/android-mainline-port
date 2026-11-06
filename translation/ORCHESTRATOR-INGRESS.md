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

Distinguish recorded facts, observer-reconstructed relationships and unresolved questions. Repository evidence is the continuity source for this clean session; do not rely on inherited conversation context or silently repair gaps through inference.

## Prepare bounded work

After reconstructing the position, prepare the next supportable worker task using the fields in [Context-segmented execution](EXECUTION-SEGMENTATION.md):

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

Pending tasks are plain, numbered Markdown records in `translation/tasks/`. Assign the next unused sequence number and keep the number as provenance; it does not encode priority, ownership or execution order. A worker discovers pending work from this directory, selects an available task, and follows its own ingress. The worker appends its reconstructable handoff to the task record. Once execution is complete, move the record unchanged except for the appended handoff to `translation/tasks/completed/`. Completion records execution, not orchestrator acceptance. The orchestrator independently evaluates the handoff and determines subsequent work. Do not introduce locking, status fields or other machinery unless actual use exposes a need.

## Expected result of this ingress

Return:

1. a concise reconstruction of the current project position;
2. the evidence and provenance supporting it;
3. unresolved matters and any adequacy limits;
4. a bounded worker task specification, if a next operation is supportable;
5. any missing repository mechanism that prevents the task from being durably recorded or independently discovered.

Do not broaden the task to solve those missing mechanisms in the same traversal.