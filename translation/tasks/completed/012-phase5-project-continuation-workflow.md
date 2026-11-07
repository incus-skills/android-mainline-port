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
- Current Pass 4 / Checkpoint 05: reconstruction/zh/continuation/checkpoints/pass-4-checkpoint-05.md, blob SHA bd80287c4db36295b5883ea0153b5fed9f1ebe8a.
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


---

## Worker handoff — Task 012

### 1. Task identity

- **Execution status:** COMPLETED — the bounded documentation-only continuation workflow trace is complete. No actual project takeover, external research, project-record inspection or device operation was performed.
- **Traversal / checkpoint:** Pass 4 — whole-skill English validation / Checkpoint 05; Phase 5 — Whole-skill validation.
- **Task:** `translation/tasks/012-phase5-project-continuation-workflow.md`; pre-handoff blob SHA `6e11828a95881f56157c8ca6166ce0e35870125b`.
- **Current-position record:** `reconstruction/zh/continuation/current-position.md`, blob SHA `fdccdaff5a7060927c609b399a4786bf3e5a6e49`; it identifies Task 012 as the next operation and prescribes the ingress sequence.
- **Active checkpoint:** `reconstruction/zh/continuation/checkpoints/pass-4-checkpoint-05.md`, blob SHA `bd80287c4db36295b5883ea0153b5fed9f1ebe8a`.
- **Pass 4 scope checkpoint:** `reconstruction/zh/continuation/checkpoints/pass-4-checkpoint-01.md`, blob SHA `598722e61e115068af5c90d90dbdf8224dd7c764`.
- **Previous checkpoint:** `reconstruction/zh/continuation/checkpoints/pass-4-checkpoint-04.md`, blob SHA `23e479d0988f0e289650aae718b55a9182cdb11e`.
- **Pass 3 closure:** `reconstruction/zh/continuation/checkpoints/pass-3-checkpoint-23.md`, blob SHA `2f356c06db43c84e5112c7a23c4bb51240c55563`.
- **Phase definitions:** `translation/README.md`, blob SHA `f9bf34ae28e838ae881f9e207797f26c1b327baa`.
- **Inventory:** `translation/INVENTORY.md`, blob SHA `0a8e58e9a6f45fd54c718672dc8d7a3a9056f7ee`.
- **Segmentation specification:** `translation/EXECUTION-SEGMENTATION.md`, blob SHA `05ee569421b89366bbc0993afe62cb22a0644913`.
- **Pinned English evidence, all exact blob SHA matches:**
  - `translation/en/SKILL.md` — `f613aa7ef89eb9204c4f9804faf7aebace596a4e`.
  - `translation/en/references/process.md` — `3e770386533dda9f0b5162043c08e9ffea454467`.
  - `translation/en/references/discipline.md` — `078e020ad497e9ec69388ad55c5beeb4677fd22a`.
- **Accepted prior task records, all exact blob SHA matches:** Task 008 `3b334e3537cb25dae7faffc0d6778b048d6529e6`; Task 009 `5662215f0ef445748c3174b1740c0b2efff7018e`; Task 010 `0357640e694f52b687d8d20b45270cdba1496cf1`; Task 011 `502f6da88d98fa3f22518b8511b7c3aaf19bacce`.

### 2. Observer position

The question was whether an incoming agent can reconstruct an existing project's current process stage, state, blockers, authorised next action and next dependency from the records named by the skill, and whether the documentation supplies a way to reconcile missing, stale or conflicting records.

The scenario intentionally supplies no actual project records or device facts. The trace therefore tests the documented record contract and stop boundary, not a particular project's state. Excluded: actual project takeover, technical assessment, external research, device operation, and changes to project artifacts or skill content.

### 3. Observations

**Entrypoint route.** In `translation/en/SKILL.md`, “How to use this skill” (lines 15–21; existing-project route at approximately line 18) directs the incoming agent to read the project's own `AGENTS.md`, status table and technical-debt table, then identify the current stage in the process. The route does not explicitly name the work log or per-feature documents at this point; their roles are defined in the discipline reference. The general mandatory working-discipline summary (lines 50 onward) reinforces that the next person must be able to continue from the documentation alone.

**Process-stage model.** `translation/en/references/process.md` “Overview” (lines 5–20) defines Stages 0–8 and three parallelism principles. Stage descriptions and their completion criteria are set out in the corresponding sections: Stage 0 (lines 26–50), Stage 1 (52–66), Stage 2 (68–91), Stage 3 (93–109), Stage 4 (111–127), Stage 5 (129–147), Stage 6 (149–170), Stage 7 (172–192), and Stage 8 (194 onward). The stages describe activities, goals and completion criteria. Stage 8 has no stated completion criterion. Some work can overlap: Stage 1 with the latter part of Stage 0; native display takeover from Stage 4; and offline preparation in Stage 6 while real-device testing remains serialised one peripheral at a time.

**Record contract.** `translation/en/references/discipline.md` “1. Required Documents and What They Must Record” (lines 7–27) defines:
- `AGENTS.md`: device and target; dated user-confirmed decisions; unattended-operation authorisation boundaries; device-safety and commit-attribution rules; material prohibited from public repositories. It explicitly says not to record current progress there.
- Status table: one-sentence status for each feature using only the prescribed status terms; link to its detailed document; the single next step; blockers; current device state; who is operating it; and whether tasks are running.
- Work log: dated changes in conclusions, not a chronological activity log.
- One detailed document per feature: approach and reuse sources; versioned evidence; investigation process and earliest failing layer; rejected hypotheses with original wording and rejection scope; completion criteria; next experiment.
- One technical-debt table, with required entry contents and lifecycle further specified in section 2 (lines 29–48).
- Abandoned-approach and experiment records, including evidence, conditions for reconsideration, expected outcomes, decision criteria, results, and evidence locations.

Section 3 “When to Write Records” (lines 50–68) adds update triggers: before starting a feature, before and after device sessions, when conclusions change, when introducing debt or abandoning an approach, when the user decides, after repeated failure, and before handoff. Before ending a session, the status table must be updated with next step, blockers and device state. Section 4 (lines 70–97) separates private records/scripts from the independent kernel and integration repositories and excludes device-unique data and secrets from repositories. Section 5 (lines 99–112) requires user agreement on attribution/repository/licence and explicit user approval before pushing, creating repositories, releasing, changing visibility or rewriting history. Section 6 (lines 114–133) includes one-device-operator-at-a-time, serial-number-in-every-command, reversibility and other operating constraints. Section 8 (lines 135 onward) assigns planning, acceptance and user communication to the primary agent; sub-agent outputs are candidate results that the primary agent checks.

**Stage identification is not explicitly encoded.** The required status-table fields do not include a current process-stage field. The process reference defines stages and completion criteria, while the status table reports feature-level statuses, a next step and blockers. The documents do not define a deterministic mapping from those feature records to a single current stage, nor say how to represent overlapping stages. An incoming agent may compare actual evidence against the stage descriptions, but must not present an inferred stage as an explicitly recorded fact. Where evidence does not support a unique stage, the stage remains unresolved.

**No record-conflict reconciliation rule is specified.** The reviewed references do not establish a precedence hierarchy or procedure for reconciling disagreement among `AGENTS.md`, status table, work log, feature documents, technical-debt table, abandoned-approach records or experiment records. They define each record's role and update timing, and assign primary-agent responsibility for planning and checking sub-agent work, but do not define a conflict-resolution algorithm, authoritative record for each contested field, or recovery process for stale/missing records.

**Scenario trace.** From the documentation alone, the incoming agent can identify which records must be requested/read and what information each is expected to contain. It cannot establish the actual project stage, feature status, current device state, active operator, running tasks, blockers, user decisions, authorisations, or next experiment because none of the actual records are supplied. Nor can it determine whether the records agree. These are absent scenario inputs, not evidence that a real project lacks those records.

### 4. Decisions

- No project stage, device condition, blocker, authorisation or next action is assigned in the hypothetical. The documentation does not supply project-specific facts.
- A status label alone is not treated as proof that a process stage or feature is complete. Feature completion must be assessed against the relevant feature document's evidence and completion criteria; stage position must be compared with the process definition.
- The incoming agent should reconstruct a candidate continuation position from the required records, preserve provenance and identify any disagreement. If a disagreement affects stage, device state, operator, running work, permission or the next action, it must not choose one record by intuition.
- In particular, do not issue device commands or start work when device state, active operator, running task or unattended-operation authorisation is uncertain. Stop for further reconstruction and the necessary responsible-person/user confirmation.
- No translation correction or repository-content change is proposed. The findings are procedural observations about the English documentation, not a technical audit.

### 5. Unresolved matters

1. **Current-stage representation:** the entrypoint asks the agent to identify the current stage, but the defined status table has no explicit stage field and no documented mapping from feature status/evidence to stage. Whether a stage should be recorded directly or reconstructed from evidence is not specified.
2. **Overlapping stages:** the process explicitly permits some parallel work, but does not specify how a single “current stage” should represent concurrent stage activity.
3. **Conflicting records:** no explicit precedence or reconciliation procedure covers conflicting next steps, blockers, feature conclusions, technical-debt status, device state, operator or running-task information.
4. **Missing/stale records:** update obligations are stated, but there is no recovery protocol defining when an incoming agent may reconstruct from other records, when to mark a value unknown, or who adjudicates.
5. **Stage 8:** the process lists maintenance activities but gives no completion criterion; this is a property of the process definition, not a blocker to tracing the handoff scenario.
6. No actual project records were supplied, so the scenario cannot establish any project-specific current position. No actual project deficiency is inferred.

### 6. Validation

- Verified the task's pinned revisions against the current branch. All listed task evidence revisions matched exactly, including Checkpoint 01, Checkpoint 04, Checkpoint 05, Pass 3 Checkpoint 23, README, inventory, segmentation specification, the three English evidence files, and completed Task 008–011 records.
- Verified the branch is `translation/english-reconstruction`; current-position record identifies Pass 4 / Checkpoint 05 and Task 012 as next.
- Read the complete pinned English entrypoint, process reference and discipline reference. Relevant section headings and line locations are recorded above.
- Traced the entrypoint reading instruction through the process-stage definitions and required-record contract.
- Distinguished documented record requirements from unavailable project facts; no device or project details were invented.
- Confirmed that the records define feature-level status and continuation fields but do not specify an explicit current-stage field or a deterministic stage inference rule.
- Confirmed that the reviewed documentation does not specify a precedence/reconciliation protocol for missing, stale or conflicting records.
- No stop condition prevented completion of this documentation-only trace. The lack of actual project records is intentional in the scenario and is the boundary at which a real takeover would have to stop.
- Validation is limited to procedural reconstructability and documentation findings. It does not establish semantic translation correctness beyond the pinned reviewed baseline, technical validity, real-device safety, actual takeover success, or context-segmentation effectiveness.

### 7. Change boundary

- **Changed:** this task record only, by appending this eight-field worker handoff.
- **Not changed:** `translation/en/`, Chinese source files, README, inventory, glossary, reviews, metadata, checkpoints, current-position record, prior task records, project records, or any device state.
- No external sources or external project records were accessed.

### 8. Next dependency

Orchestrator acceptance requires independent reconstruction of this handoff against the pinned evidence and verification that the task record contains only the appended handoff. If accepted, the next useful project-level operation is for the orchestrator to decide whether the current-stage and conflicting-record gaps merit a separate, narrowly scoped documentation investigation or clarification. This worker does not create that task, alter the process documents, or advance the project checkpoint.
