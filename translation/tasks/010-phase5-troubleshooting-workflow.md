# Task 010 — Phase 5 representative troubleshooting workflow

## Purpose and observer position

Perform one bounded representative troubleshooting-workflow check under Pass 4 — whole-skill English validation. Trace the English skill's troubleshooting route for one documented symptom: **a filesystem behaves abnormally after a sparse image is flashed over a partition containing old data**.

Determine whether an agent can follow the documented route from the entrypoint, through problem-pattern classification and general handling, to the specific experience entry, without inventing facts or expanding into device operations.

This is a procedural walk-through of the documentation. It is not a technical correctness audit, a diagnosis of a real device, or execution of flashing or recovery operations.

## Position and governing criteria

- Traversal: **Pass 4 — whole-skill English validation**.
- Project phase: **Phase 5 — Whole-skill validation**, as defined in `translation/README.md`.
- Current position: Pass 4 / Checkpoint 03.
- Previous accepted operations: Task 008 — entrypoint/navigation integrity; Task 009 — technical-decision workflow.
- Pass 4 / Checkpoint 01: `reconstruction/zh/continuation/checkpoints/pass-4-checkpoint-01.md`, blob SHA `598722e61e115068af5c90d90dbdf8224dd7c764`.
- Pass 4 / Checkpoint 02: `reconstruction/zh/continuation/checkpoints/pass-4-checkpoint-02.md`, blob SHA `e2eee68d113988b3aed8a01fb03682fa130aa1f0`.
- Current checkpoint: `reconstruction/zh/continuation/checkpoints/pass-4-checkpoint-03.md`, blob SHA `e6b0a752a82d7484b75083ad4a5279bc81342549`.
- Task 008 accepted record: `translation/tasks/completed/008-phase5-entrypoint-navigation-integrity.md`.
- Task 009 accepted record: `translation/tasks/completed/009-phase5-technical-decision-workflow.md`.
- Phase definitions and Phase 5 criteria: `translation/README.md`, blob SHA `f9bf34ae28e838ae881f9e207797f26c1b327baa`.
- Translation inventory: `translation/INVENTORY.md`, blob SHA `0a8e58e9a6f45fd54c718672dc8d7a3a9056f7ee`.
- English entrypoint: `translation/en/SKILL.md`, blob SHA `f613aa7ef89eb9204c4f9804faf7aebace596a4e`.
- Pattern index: `translation/en/experience/patterns.md`, blob SHA `7309618ad08085bb514b6800a580774f829fe312`.
- Specific experience document: `translation/en/experience/usb-and-storage.md`, blob SHA `7c3225d7a7f8650ce27a345922937cdadcf0cc59`.
- Experience library index: `translation/en/experience/README.md`, blob SHA `a7a28190615253246749909b380ddcba17268769`.
- Pinned English tree from Task 008: `translation/en/`, tree SHA `ba8a19206173ccde8fd26b6ae731ca4a0ee8c7cc`.

## Exact evidence boundary

Use only:
1. The complete pinned English `translation/en/SKILL.md`.
2. The relevant troubleshooting route and file-index references in the pinned entrypoint.
3. The relevant introduction, Category D section, and any directly necessary cross-reference in pinned `translation/en/experience/patterns.md`.
4. The specific **DONT_CARE in a Sparse Image Does Not Clear Old Data and May Replay Old Journal Records** entry in pinned `translation/en/experience/usb-and-storage.md`.
5. The relevant navigation/usage instructions in pinned `translation/en/experience/README.md`.
6. Phase 5 criteria and the accepted Task 008 / Task 009 / active Pass 4 checkpoint records for scope and position.

Do not load unrelated pattern categories or the full experience library. If the route requires an unlisted dependency to complete this bounded trace, report it and stop rather than broadening the task.

## Scope of work

1. Reconstruct the troubleshooting route from the English entrypoint. Identify its first reading destination and the prescribed way to proceed from general classification to a specific case.
2. Read the relevant experience-library index instructions and the introduction to `patterns.md`.
3. Read Category D (residual state distorts the result) and trace its general handling steps.
4. Locate and read the named sparse-image entry in `usb-and-storage.md`.
5. Walk through this deliberately bounded hypothetical symptom: **after a sparse image is flashed over a partition that previously contained data, the filesystem behaves abnormally**. Do not assume device identity, partition layout, image contents, tool versions, or actual cause beyond what the selected documentation states.
6. Determine whether the documented route makes it possible to:
   - recognise the symptom as a candidate for Category D;
   - follow the category's general handling;
   - locate the specific experience entry;
   - distinguish the documented cause, resolution and applicability;
   - identify what evidence or tests the document prescribes, and what it does not specify;
   - recognise when the available scenario is insufficient for a real diagnosis or operation.
7. Assess whether the route is procedurally followable for this one case and record any navigation gap, ambiguity or missing dependency. Distinguish an actual documentation gap from information deliberately absent from the hypothetical scenario.
8. Report only procedural/documentation findings. Do not silently repair text or infer technical correctness from the documentation's presentation.

## Exclusions

- Do not assess whether the sparse-image diagnosis or resolution is currently technically correct or safe.
- Do not browse upstream sources, device forums, vendor documentation or external references.
- Do not identify or assume a real device, partition, image format variant, filesystem, tool version or current device state.
- Do not issue, simulate or recommend commands for a real device.
- Do not perform flashing, partition access, formatting, filesystem repair, backup or recovery operations.
- Do not turn the hypothetical symptom into a confirmed root cause; distinguish documented explanation from evidence available in the scenario.
- Do not validate all pattern categories, all troubleshooting entries or the full experience library.
- Do not reopen Pass 3 translation review.
- Do not assess installation or skill-discovery metadata compatibility.
- Do not claim that a documentation walk-through proves real-world troubleshooting success.

## Permitted changes

- Read-only with respect to skill content and project source material.
- Do not modify `translation/en/`, Chinese source files, inventory, README, glossary, reviews, metadata, checkpoints or current-position records.
- Append the required eight-field handoff to this task record only.
- Do not move the task to `completed/`; worker completion remains separate from orchestrator acceptance.

## Stop conditions

Stop and report if:
- any pinned evidence cannot be retrieved at the stated revision;
- the troubleshooting route depends on an unlisted document necessary to complete this bounded trace;
- the scenario requires device-specific facts or external evidence to proceed beyond identifying the missing inputs;
- the route does not make the general-to-specific investigation sequence reconstructable;
- the task would require technical correctness review, real-device operation or scope expansion.

## Validation

- Confirm exact blob SHAs for the entrypoint, pattern index, specific experience document and experience-library index.
- Record the exact sections/lines used to reconstruct the route, category and specific entry.
- Trace the hypothetical symptom without adding device facts or treating the documented cause as confirmed.
- Distinguish route instructions, pattern recognition, general handling, case-specific guidance, unresolved facts and stop points.
- Record whether the route is procedurally followable for this bounded case, with limitations.
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
