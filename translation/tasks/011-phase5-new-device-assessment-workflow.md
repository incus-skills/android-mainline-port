# Task 011 — Phase 5 representative new-device assessment workflow

## Purpose and observer position

Perform one bounded representative **new-device assessment** workflow check under Pass 4 — whole-skill English validation.

Trace the English skill's **New project** and **Assessment only** routes through Stage 0 — Project Scoping. Use this deliberately incomplete hypothetical request:

> A user asks whether an Android tablet can be ported to mainline Linux, but supplies no device model, exact SoC, bootloader-unlock state, stock OS version, storage layout or A/B configuration.

Determine whether an agent can reconstruct the documented assessment sequence, identify the required evidence and outputs, distinguish work that can be planned from conclusions that cannot yet be reached, and preserve the user-confirmation boundary without inventing device facts.

This is a documentation-only workflow trace. It is not an actual assessment of a device, feasibility opinion, external research exercise, or device operation.

## Position and governing criteria

- Traversal: **Pass 4 — whole-skill English validation**.
- Project phase: **Phase 5 — Whole-skill validation**, as defined in `translation/README.md`.
- Current position: Pass 4 / Checkpoint 04.
- Previous accepted operations: Task 008 — entrypoint/navigation integrity; Task 009 — technical-decision workflow; Task 010 — troubleshooting workflow.
- Pass 4 / Checkpoint 01: `reconstruction/zh/continuation/checkpoints/pass-4-checkpoint-01.md`, blob SHA `598722e61e115068af5c90d90dbdf8224dd7c764`.
- Pass 4 / Checkpoint 02: `reconstruction/zh/continuation/checkpoints/pass-4-checkpoint-02.md`, blob SHA `e2eee68d113988b3aed8a01fb03682fa130aa1f0`.
- Pass 4 / Checkpoint 03: `reconstruction/zh/continuation/checkpoints/pass-4-checkpoint-03.md`, blob SHA `e6b0a752a82d7484b75083ad4a5279bc81342549`.
- Current checkpoint: `reconstruction/zh/continuation/checkpoints/pass-4-checkpoint-04.md`, blob SHA `23e479d0988f0e289650aae718b55a9182cdb11e`.
- Pass 3 closure: `reconstruction/zh/continuation/checkpoints/pass-3-checkpoint-23.md`, blob SHA `2f356c06db43c84e5112c7a23c4bb51240c55563`.
- Phase definitions and Phase 5 criteria: `translation/README.md`, blob SHA `f9bf34ae28e838ae881f9e207797f26c1b327baa`.
- Translation inventory: `translation/INVENTORY.md`, blob SHA `0a8e58e9a6f45fd54c718672dc8d7a3a9056f7ee`.
- Context-segmentation specification: `translation/EXECUTION-SEGMENTATION.md`, blob SHA `05ee569421b89366bbc0993afe62cb22a0644913`.
- English entrypoint: `translation/en/SKILL.md`, blob SHA `f613aa7ef89eb9204c4f9804faf7aebace596a4e`.
- Stage 0 process: `translation/en/references/process.md`, blob SHA `3e770386533dda9f0b5162043c08e9ffea454467`.
- Technical decision criteria: `translation/en/references/decisions.md`, blob SHA `3e63f6f4b54ab987c9e438dbf015d4f7f72d85c9`.
- Working discipline: `translation/en/references/discipline.md`, blob SHA `078e020ad497e9ec69388ad55c5beeb4677fd22a`.
- Stage 0 relevant experience: `translation/en/experience/testing-and-process.md`, blob SHA `b78afbf1b3a9f05a6765a51b2241eb56bd5c0eca`.
- Pinned English tree: `translation/en/`, tree SHA `ba8a19206173ccde8fd26b6ae731ca4a0ee8c7cc`.
- Accepted Task 008 record: `translation/tasks/completed/008-phase5-entrypoint-navigation-integrity.md`, blob SHA `3b334e3537cb25dae7faffc0d6778b048d6529e6`.
- Accepted Task 009 record: `translation/tasks/completed/009-phase5-technical-decision-workflow.md`, blob SHA `5662215f0ef445748c3174b1740c0b2efff7018e`.
- Accepted Task 010 record: `translation/tasks/completed/010-phase5-troubleshooting-workflow.md`, blob SHA `0357640e694f52b687d8d20b45270cdba1496cf1`.

## Exact evidence boundary

Use only:

1. The complete pinned English `translation/en/SKILL.md`, especially the New project route, Assessment only route, Stage 0 summary and user-input decision list.
2. Stage 0 — Project Scoping in pinned `translation/en/references/process.md`, including its stated goal, ordered activities, outputs, completion criterion and relevant experience.
3. The general decision criteria in pinned `translation/en/references/decisions.md`; do not load or audit all technical decision records. Follow only directly relevant references needed to understand how Stage 0 says technical approaches are selected.
4. Sections 1 and 3 of pinned `translation/en/references/discipline.md`, for required project records and user-confirmed decisions.
5. Only the directly relevant comparable-project/reuse entries in pinned `translation/en/experience/testing-and-process.md`.
6. Phase 5 criteria and the accepted Task 008 / Task 009 / Task 010 records and active Pass 4 checkpoints for position and scope.

Do not load unrelated stages, the full experience library or all technical decision records. If Stage 0 depends on an unlisted document necessary to complete this bounded trace, report the dependency and stop rather than broadening the task.

## Scope of work

1. Reconstruct the New project and Assessment only routes from the pinned English entrypoint. Identify their stated relationship to Stage 0.
2. Read Stage 0 in the pinned process reference. Trace its goal, eight ordered activities, outputs, completion criterion and relevant experience.
3. Walk through the deliberately incomplete hypothetical request. Identify which facts are absent, which activities can be described from the documentation alone, and which conclusions cannot be reached without device-specific evidence or external research.
4. Reconstruct the documented evidence requirements for:
   - device identity and platform baseline;
   - upstream, same-SoC community, vendor-source and comparable-project research;
   - hardware inventory and reuse investigation;
   - technical approach selection and workload estimation;
   - the user decision list, confirmation step and project record.
5. Follow only the relevant general decision criteria and discipline instructions needed to understand how evidence, options, user confirmation and project records constrain the Stage 0 workflow.
6. Determine whether the documentation makes the assessment outputs and completion boundary reconstructable, including the relationship between an **assessment-only** request and Stage 0's listed workspace/research/record outputs.
7. Identify where the agent must stop rather than invent missing facts, claim feasibility, select device-specific approaches or proceed to device interaction before user confirmation.
8. Assess whether this single representative workflow is procedurally followable and record any navigation gap, ambiguity, missing dependency or distinction between absent scenario information and an actual documentation gap.
9. Report only procedural/documentation findings. Do not silently repair text or infer technical correctness from the documentation's presentation.

## Exclusions

- Do not assess whether any actual Android device can run mainline Linux.
- Do not browse upstream kernel sources, community projects, vendor sources, issue trackers or external documentation.
- Do not perform or simulate device inspection, Fastboot queries, bootloader unlocking, backups, partition reads or any other device operation.
- Do not create a workspace, repositories, research pool, hardware inventory, reuse table, assessment report, `AGENTS.md` or other project artifacts. This is a workflow trace, not a real project setup.
- Do not invent model, SoC, bootloader, OS, storage or A/B facts.
- Do not make device-specific technical recommendations or choose among conditional approaches.
- Do not ask the user for missing details; identify what the documented workflow requires and stop at that boundary.
- Do not treat the documented completion criterion as met without user confirmation.
- Do not reopen Pass 3 translation review.
- Do not assess troubleshooting, project-continuation workflow, installation, skill-discovery metadata or overall Phase 5 completion.
- Do not claim that this documentation walk-through proves actual assessment quality or operational usability.

## Permitted changes

- Read-only with respect to skill content and project source material.
- Do not modify `translation/en/`, Chinese source files, inventory, README, glossary, reviews, metadata, checkpoints or current-position records.
- Append the required eight-field handoff to this task record only.
- Once execution and handoff are complete, move the task record unchanged except for the appended handoff to `translation/tasks/completed/`. This records worker execution only and does not imply orchestrator acceptance.

## Stop conditions

Stop and report if:

- any pinned evidence cannot be retrieved at the stated revision;
- the Stage 0 route requires an unlisted document necessary to complete this bounded trace;
- the task would require external research, device-specific evidence or user input to proceed beyond identifying the missing inputs;
- the entrypoint, process reference and decision/discipline references give conflicting instructions that prevent reconstructing the assessment boundary;
- the task would require actual project setup, technical selection, device interaction or scope expansion.

## Validation

- Confirm exact blob SHAs for all pinned evidence files and the active checkpoint.
- Record the exact sections/lines used to reconstruct both entrypoint routes, Stage 0 activities/outputs/completion and user-confirmation requirements.
- Trace the hypothetical request without adding device facts or claiming feasibility.
- Distinguish required evidence, documented workflow steps, outputs, user decisions, missing scenario inputs and stop points.
- Assess whether the assessment-only route's relationship to the Stage 0 outputs is clear, ambiguous or unresolved; do not resolve ambiguity by assumption.
- Preserve the distinction between procedural coherence, semantic correctness, current technical validity and actual assessment success.
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

State whether the task was completed, stopped for an adequacy reason, or requires additional evidence. The worker must not claim orchestrator acceptance.
