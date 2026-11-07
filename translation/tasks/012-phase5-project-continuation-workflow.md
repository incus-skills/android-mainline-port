# Task 012 — Phase 5 representative existing-project continuation workflow

## Purpose and observer position

Perform one bounded representative **taking over an existing project** workflow check under Pass 4 — whole-skill English validation.

Trace the English skill's **Taking over an existing project** route through the records it requires an agent to read and the instruction to identify the current process stage.

Use this deliberately incomplete, documentation-only handoff scenario:

> An agent is asked to continue an existing Android mainline port. The project is said to contain an AGENTS.md, a status table, a technical-debt table, a work log and per-feature documents. No actual project records or device details are supplied. The handoff does not state a process stage. Consider also the possibility that the records, if later supplied, disagree about the next step or current device state.

Determine whether the skill documentation allows an agent to reconstruct the current stage, current state, blockers, authorised next action and next dependency from the specified records; what information each record must supply; and where the agent must stop rather than infer missing or conflicting project facts.

This is a documentation-only workflow trace. It is not an actual project takeover, device assessment, external research exercise, or device operation.

## Position and governing criteria

- Traversal: **Pass 4 — whole-skill English validation**.
- Project phase: **Phase 5 — Whole-skill validation**, as defined in translation/README.md.
- Current position: Pass 4 / Checkpoint 05.
- Previous accepted operations: Task 008 — entrypoint/navigation integrity; Task 009 — technical-decision workflow; Task 010 — troubleshooting workflow; Task 011 — new-device assessment workflow.
- Pass 4 / Checkpoint 01: reconstruction/zh/continuation/checkpoints/pass-4-checkpoint-01.md, blob SHA 598722e61e115068af5c90d90dbdf8224dd7c764.
- Previous Pass 4 / Checkpoint 04: reconstruction/zh/continuation/checkpoints/pass-4-checkpoint-04.md, blob SHA 23e479d0988f0e289650aae718b55a9182cdb11e.
- Current Pass 4 / Checkpoint 05: reconstruction/zh/continuation/checkpoints/pass-4-checkpoint-05.md, blob SHA a2c0bf6f2fb1d1dd0bc32d33916c7b8c32890172.
- Pass 3 closure: reconstruction/zh/continuation/checkpoints/pass-3-checkpoint-23.md, blob SHA 2f356c06db43c84e5112c7a23c4bb51240c55563.
- Phase definitions and Phase 5 criteria: translation/README.md, blob SHA f9bf34ae28e838ae881f9e207797f26c1b327baa.
- Translation inventory: translation/INVENTORY.md, blob SHA 0a8e58e9a6f45fd54c718672dc8d7a3a9056f7ee.
- Context-segmentation specification: translation/EXECUTION-SEGMENTATION.md, blob SHA 05ee569421b89366bbc0993afe62cb22a0644913.
- English entrypoint: translation/en/SKILL.md, blob SHA f613aa7ef89eb9204c4f9804faf7aebace596a4e.
- Process stages: translation/en/references/process.md, blob SHA 3e770386533dda9f0b5162043c08e9ffea454467.
- Working discipline and project-record definitions: translation/en/references/discipline.md, blob SHA 078e020ad497e9ec69388ad55c5beeb4677fd22a.
- Accepted Task 008 record: translation/tasks/completed/008-phase5-entrypoint-navigation-integrity.md, blob SHA 3b334e3537cb25dae7faffc0d6778b048d6529e6.
- Accepted Task 009 record: translation/tasks/completed/009-phase5-technical-decision-workflow.md, blob SHA 5662215f0ef445748c3174b1740c0b2efff7018e.
- Accepted Task 010 record: translation/tasks/completed/010-phase5-troubleshooting-workflow.md, blob SHA 0357640e694f52b687d8d20b45270cdba1496cf1.
- Accepted Task 011 record: translation/tasks/completed/011-phase5-new-device-assessment-workflow.md, blob SHA 502f6da88d98fa3f22518b8511b7c3aaf19bacce.

## Exact evidence boundary

Use only:

1. The complete pinned English translation/en/SKILL.md, especially the Taking over an existing project route and its surrounding process navigation.
2. The complete pinned translation/en/references/process.md, to reconstruct the process stages, their stated activities, outputs and completion criteria.
3. The complete pinned translation/en/references/discipline.md, especially required project records, technical-debt records, when records are updated, directory structure, device/authorisation safety and handoff responsibilities.
4. Phase 5 criteria and the accepted Pass 4 checkpoints/tasks only for position, scope and distinctions between already accepted workflows and remaining work.

Do not load the full experience library, technical decision records, upstream project records or external documentation. If a required process or record dependency is not covered by these pinned documents, report it and stop rather than broadening the task.

## Scope of work

1. Reconstruct the Taking over an existing project entrypoint route from the pinned English SKILL.md. Record its explicit reading order and the action it directs the agent to take after reading the project records.
2. Read the pinned process reference and identify the process-stage structure, each stage's purpose and completion criteria sufficiently to explain what it means to identify the current stage.
3. Read the pinned discipline reference and reconstruct the documented roles and required contents of:
   - AGENTS.md / agent-instructions file;
   - status table;
   - work log;
   - per-feature detailed documents;
   - technical-debt table;
   - abandoned-approach record and experiment records where they affect continuation;
   - handoff/update records, device state, operator and running-task information;
   - user-confirmed decisions and unattended-operation authorisation.
4. Walk through the incomplete handoff scenario. Distinguish what can be reconstructed from the skill's documentation alone from information that must come from the actual project's records.
5. Determine whether the required record formats explicitly identify the current process stage, or whether the stage must be inferred from feature statuses, next steps, blockers and evidence. Do not invent a mapping if the documentation does not specify one.
6. Determine whether the documentation supplies a rule for reconciling missing, stale or conflicting records, especially when the status table, feature documents, work log, technical-debt table and AGENTS.md imply different next actions or device states.
7. Identify the minimum evidence needed before an incoming agent can safely continue, including current device state, who is operating it, running tasks, blockers, next step, user decisions and authorisation boundaries.
8. Identify where the agent must stop and seek reconstruction or user clarification rather than infer a process stage, device condition, permission or safe next action.
9. Assess whether this single representative continuation route is procedurally followable and record navigation gaps, ambiguities, missing dependencies or distinctions between missing scenario data and actual documentation gaps.
10. Report only procedural/documentation findings. Do not silently repair text or infer technical correctness from the documentation's presentation.

## Exclusions

- Do not perform an actual project takeover or inspect any external project workspace.
- Do not invent AGENTS.md, status-table, feature-record, work-log or technical-debt contents for a real project.
- Do not assess whether any actual Android device can run mainline Linux.
- Do not browse upstream kernel sources, community projects, vendor sources, issue trackers or external documentation.
- Do not perform or simulate device inspection, Fastboot queries, bootloader unlocking, backups, partition reads or any other device operation.
- Do not create, edit or update project records or artifacts.
- Do not make device-specific technical recommendations or choose among conditional approaches.
- Do not treat a status label, completed task or worker report as proof that a process stage or feature is complete unless the documented evidence supports it.
- Do not assume that omitted records or context do not exist. Identify the dependency and stop if it is necessary to complete the bounded trace.
- Do not reopen Pass 3 translation review.
- Do not reassess the new-device assessment, technical-decision or troubleshooting workflows already accepted under their stated boundaries.
- Do not assess installation, skill-discovery metadata or overall Phase 5 completion.
- Do not claim that this documentation walk-through proves actual takeover quality, operational usability or context-segmentation effectiveness.

## Permitted changes

- Read-only with respect to skill content and project source material.
- Do not modify translation/en/, Chinese source files, inventory, README, glossary, reviews, metadata, checkpoints, current-position records or other task records.
- Append the required eight-field handoff to this task record only.
- Once execution and handoff are complete, move the task record unchanged except for the appended handoff to translation/tasks/completed/. This records worker execution only and does not imply orchestrator acceptance.

## Stop conditions

Stop and report if:

- any pinned evidence cannot be retrieved at the stated revision;
- the route requires an unlisted document necessary to reconstruct the continuation procedure;
- the task would require actual project records, external research, device-specific evidence or user input to proceed beyond identifying the missing inputs;
- the process and discipline references give conflicting instructions that prevent reconstructing the continuation boundary;
- a safe next action would depend on resolving a conflicting device state, active operator, running task or authorisation record;
- the task would require actual project setup, technical selection, device interaction or scope expansion.

## Validation

- Confirm exact blob SHAs for all pinned evidence files and the active checkpoint.
- Record the exact sections/lines used to reconstruct the entrypoint route, process-stage definitions, record requirements and handoff/authorisation boundaries.
- Distinguish process-stage descriptions from project-specific evidence of the stage actually reached.
- Determine whether the required record schema contains an explicit current-stage field or a documented inference/reconciliation rule.
- Trace the incomplete handoff scenario without adding project or device facts.
- Distinguish required records, required fields, current project evidence, missing scenario inputs and stop points.
- Preserve the distinction between procedural coherence, semantic correctness, current technical validity and actual takeover success.
- Record all evidence revisions, unresolved matters and the read-only change boundary.

## Required handoff

Append a reconstructable handoff to this task record using the eight fields in translation/EXECUTION-SEGMENTATION.md:

1. Task identity.
2. Observer position.
3. Observations.
4. Decisions.
5. Unresolved matters.
6. Validation.
7. Change boundary.
8. Next dependency.

State whether the task was completed, stopped for an adequacy reason, or requires additional evidence. The worker must not claim orchestrator acceptance.
