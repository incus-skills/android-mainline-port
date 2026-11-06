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

## Evidence admissibility

For source-grounded translation work, every claim, distinction, technical token, qualification and proposed correction introduced into the review must be traceable to an explicit passage or structural feature in the designated source or target.

- The Chinese source establishes what the source says; the English target establishes what the translation says; a Pass 3 finding may describe only a correspondence demonstrable from those two bodies of evidence.
- Do not introduce external technical knowledge, inferred qualifications, customary terminology or domain assumptions into a source-grounded comparison. Plausibility or technical correctness does not establish source correspondence.
- External knowledge may prompt a question for separate investigation, but it cannot establish a Pass 3 finding or justify a translation change.
- Before changing the target or review, identify the exact source and/or target evidence that necessitates the change and explain the correspondence. If that evidence cannot be identified, do not make the change; record the matter as unresolved or out of scope when useful.
- Distinguish source claims, target claims, observations about their correspondence and external interpretations. Only the first three belong in the source-grounded Pass 3 review.
- This evidence standard applies equally to the original review and every corrective pass. A correction inherits the original task's evidence boundary; it does not expand that boundary. An orchestrator's instruction to correct an identified defect does not authorise a new interpretation or evidence base.

## Investigation proposals

During the assigned work, the worker may separately record questions or opportunities that could merit later investigation using evidence outside the current task boundary. Examples include checking current upstream implementation behaviour, testing on physical devices, checking whether advice remains current, or considering a possible improvement or extension to the skill.

These are proposals, not findings or corrections. Keep them distinguishable from the source-grounded review and the required handoff decisions. Do not resolve them as part of the current task unless its stated scope explicitly permits that work. Do not use external investigation proposals as evidence for Pass 3 findings or acceptance. Record enough context to reconstruct what prompted each proposal and what external evidence would be needed; if none arise, do not manufacture any.

## Execution

- Treat the selected task as the authority for its local scope, permitted changes, validation and stop conditions.
- Do not rely on the originating orchestrator conversation or request unstated context from it.
- Do not expand the task to adjacent work merely because related material is available.
- Preserve source files and other excluded files exactly as the task directs.
- Record observations, decisions, evidence, unresolved matters and validation in a reconstructable handoff appended to the task record, using the eight fields in [Context-segmented execution](EXECUTION-SEGMENTATION.md).
- Do not move a task to `completed/` until execution and handoff are complete. Moving the record indicates worker execution is finished; it does not indicate orchestrator acceptance.

## Boundaries

The worker executes and reports. The orchestrator independently reconstructs and evaluates the handoff, integrates changes and determines the next operation. Do not create task-management machinery, assign work to other workers or alter the project-level continuation position.
