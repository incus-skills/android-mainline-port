# Worker session ingress

## Purpose

This is the entry point for an independent worker session participating in the context-segmented execution experiment. The worker must be able to reconstruct its local task boundary from repository records, without access to the orchestrator's conversation.

This is an experimental ChatGPT-session arrangement. It does not replace or redefine the harness-oriented agentic segmentation strategy.

## Entry and selection

1. Read [Context-segmented execution](EXECUTION-SEGMENTATION.md) for the worker boundary and required handoff.
2. Inspect the pending task records in [the task sequence](tasks/). Each record is an independently bounded operation.
3. Select one available task. For this initial trial, only one worker should select and execute work at a time; do not add a claim or locking mechanism. Do not assume that the lowest number is highest priority or that tasks must execute strictly in sequence. If concurrent selection occurs, record the collision as an observed limitation rather than inventing a protocol.
4. Read the selected task completely and verify its stated file revisions and governing references before acting.
5. If the task is missing required evidence, conflicts with repository state or cannot be completed within its stated boundary, stop and report that in the task record.

## Execution

- Treat the selected task as the authority for its local scope, permitted changes, validation and stop conditions.
- Do not rely on the originating orchestrator conversation or request unstated context from it.
- Do not expand the task to adjacent work merely because related material is available.
- Preserve source files and other excluded files exactly as the task directs.
- Record observations, decisions, evidence, unresolved matters and validation in a reconstructable handoff appended to the task record, using the eight fields in [Context-segmented execution](EXECUTION-SEGMENTATION.md).
- Do not move a task to `completed/` until execution and handoff are complete. Moving the record indicates worker execution is finished; it does not indicate orchestrator acceptance.

## Boundaries

The worker executes and reports. The orchestrator independently reconstructs and evaluates the handoff, integrates changes and determines the next operation. Do not create task-management machinery, assign work to other workers or alter the project-level continuation position.
