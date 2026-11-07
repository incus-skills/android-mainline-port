# Task 009 — Phase 5 representative technical-decision workflow

## Purpose and observer position

Perform one bounded representative-workflow check under Pass 4 — whole-skill English validation. Trace the English skill's **technical-decision** route for one decision record, **Backup and Root Access**, and determine whether the documented route gives an agent a structurally coherent way to reach, frame, and record that decision without inventing missing device facts.

This is a procedural walk-through of the documentation. It is not an external technical review, a recommendation for a real device, or execution of backup/root operations.

## Position and governing criteria

- Traversal: **Pass 4 — whole-skill English validation**.
- Project phase: **Phase 5 — Whole-skill validation**, as defined in `translation/README.md`.
- Current position: Pass 4 / Checkpoint 02.
- Previous accepted operation: Task 008 — entrypoint and navigation integrity.
- Pass 4 / Checkpoint 01: `reconstruction/zh/continuation/checkpoints/pass-4-checkpoint-01.md`, blob SHA `598722e61e115068af5c90d90dbdf8224dd7c764`.
- Current checkpoint: `reconstruction/zh/continuation/checkpoints/pass-4-checkpoint-02.md`, blob SHA `e2eee68d113988b3aed8a01fb03682fa130aa1f0`.
- Task 008 accepted record: `translation/tasks/completed/008-phase5-entrypoint-navigation-integrity.md`.
- Phase definitions and Phase 5 criteria: `translation/README.md`, blob SHA `f9bf34ae28e838ae881f9e207797f26c1b327baa`.
- Translation inventory: `translation/INVENTORY.md`, blob SHA `0a8e58e9a6f45fd54c718672dc8d7a3a9056f7ee`.
- English entrypoint: `translation/en/SKILL.md`, blob SHA `f613aa7ef89eb9204c4f9804faf7aebace596a4e`.
- Decision reference: `translation/en/references/decisions.md`, blob SHA `3e63f6f4b54ab987c9e438dbf015d4f7f72d85c9`.
- Working-discipline reference: `translation/en/references/discipline.md`, blob SHA `078e020ad497e9ec69388ad55c5beeb4677fd22a`.
- Pinned English tree from Task 008: `translation/en/`, tree SHA `ba8a19206173ccde8fd26b6ae731ca4a0ee8c7cc`.

## Exact evidence boundary

Use only:
1. The complete pinned English `translation/en/SKILL.md`.
2. The general decision criteria and the **Backup and Root Access** section of the pinned `translation/en/references/decisions.md`.
3. The directly relevant decision-recording and user-confirmation requirements in the pinned `translation/en/references/discipline.md`.
4. The Phase 5 criteria in `translation/README.md` and the accepted Task 008 / active Pass 4 checkpoint records for scope and position.

Do not load the full experience library or unrelated decision sections. If a specific dependency cannot be resolved from this evidence, report it and stop rather than broadening the task.

## Scope of work

1. Reconstruct the technical-decision entry route from the English entrypoint. Identify the stated first reading destination and any subsequent documentation dependency necessary for this one decision.
2. Read the general decision criteria and the complete **Backup and Root Access** decision record.
3. Read only the relevant portions of `references/discipline.md` that establish how decisions, user confirmation, evidence and project records are handled.
4. Perform a documentation walk-through using this deliberately incomplete scenario: **a user asks whether an Android device can be ported, but supplies no device model, partition layout, root/recovery capability, backup scope, or evidence of an available recovery path.**
5. Determine what the documented route allows the agent to conclude, what information it must obtain, which decision options or conditional rules become relevant only after facts are supplied, and where the agent must stop pending user confirmation or evidence.
6. Assess whether the decision record provides the documented elements needed for a reasoned decision: default, conditional rules, rationale, exceptions, and acceptance criteria where present. Do not demand fields that the source does not prescribe.
7. Assess whether the route distinguishes a documented default from an automatic recommendation for the hypothetical device, and whether it preserves the user's authority over data-loss and backup-scope decisions.
8. Report only procedural/documentation findings. Do not silently repair wording or infer technical correctness from the document's presentation.

## Exclusions

- Do not assess whether rooting, Magisk, KernelSU, recovery boot, partition backup, GPT backup or any other technical recommendation is currently correct or safe.
- Do not browse upstream sources, device forums or vendor documentation.
- Do not identify or assume a real device, SoC, partition layout, bootloader capability or recovery method.
- Do not issue, simulate or recommend commands for a real device.
- Do not perform rooting, backup, flashing, partition access or other device operations.
- Do not convert missing scenario facts into documentation defects unless the route fails to identify that those facts are needed.
- Do not validate all decision records or all Phase 5 workflows.
- Do not reopen Pass 3 translation review.
- Do not assess installation or skill-discovery metadata compatibility.
- Do not claim that a documentation walk-through proves real-world workflow usability.

## Permitted changes

- Read-only with respect to skill content and project source material.
- Do not modify `translation/en/`, Chinese source files, inventory, README, glossary, reviews, metadata, checkpoints or current-position records.
- Append the required eight-field handoff to this task record only.
- Do not move the task to `completed/`; worker completion remains separate from orchestrator acceptance.

## Stop conditions

Stop and report if:
- any pinned evidence cannot be retrieved at the stated revision;
- the technical-decision route depends on an unlisted document necessary to complete this bounded trace;
- the scenario would require device-specific facts or external evidence to proceed beyond identifying missing inputs;
- the documentation does not make the user-confirmation or evidence boundary reconstructable;
- the task would require technical correctness review, real-device operation or scope expansion.

## Validation

- Confirm exact blob SHAs for the entrypoint, decision reference and working-discipline reference.
- Record the exact sections/lines used to reconstruct the route and decision-recording requirements.
- Trace the hypothetical scenario without filling in missing device facts.
- Distinguish documented defaults, conditional rules, unresolved inputs, user decisions and stop points.
- Record whether the route is procedurally followable for this bounded decision, with limitations.
- Preserve the distinction between documentation coherence, semantic correctness, current technical validity and actual operational usability.
- Record all evidence revisions and the read-only change boundary.

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

State whether the task was completed, stopped for an adequacy reason, or requires additional evidence. Worker completion does not constitute orchestrator acceptance.

For the initial trial, only one worker should select and execute a task at a time. No locking or claim mechanism is assumed.
