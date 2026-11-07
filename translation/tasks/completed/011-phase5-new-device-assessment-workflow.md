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


---

## Worker handoff — Task 011

### 1. Task identity

- **Execution status:** COMPLETED — the bounded documentation workflow trace and reconstructable handoff are complete. The hypothetical request does not support a device-specific assessment or Stage 0 completion; that is the intended evidence boundary, not a claim of feasibility or a completed real assessment.
- **Traversal / checkpoint:** Pass 4 / Checkpoint 04; Phase 5 — Whole-skill validation.
- **Task:** `translation/tasks/011-phase5-new-device-assessment-workflow.md`; pre-handoff blob SHA `102db73d29daa7bf0f805eb81f932966a1d53ace`.
- **Pinned English evidence, all exact blob SHA matches:**
  - `translation/en/SKILL.md` — `f613aa7ef89eb9204c4f9804faf7aebace596a4e`.
  - `translation/en/references/process.md` — `3e770386533dda9f0b5162043c08e9ffea454467`.
  - `translation/en/references/decisions.md` — `3e63f6f4b54ab987c9e438dbf015d4f7f72d85c9`.
  - `translation/en/references/discipline.md` — `078e020ad497e9ec69388ad55c5beeb4677fd22a`.
  - `translation/en/experience/testing-and-process.md` — `b78afbf1b3a9f05a6765a51b2241eb56bd5c0eca`.
- **Pinned English tree:** `translation/en/` — `ba8a19206173ccde8fd26b6ae731ca4a0ee8c7cc`; independently retrieved by exact tree SHA, with `truncated: false`. The `references/` subtree is `41f3799c0dd0caa7bdef8e9b4820f45f21ace473`; the `experience/` subtree is `d8d0db60cb0ef7dafd527f1d5302683bb3fbbb93`.
- **Position and governing evidence, exact SHA matches:**
  - Pass 4 / Checkpoint 01 — `598722e61e115068af5c90d90dbdf8224dd7c764`.
  - Pass 4 / Checkpoint 02 — `e2eee68d113988b3aed8a01fb03682fa130aa1f0`.
  - Pass 4 / Checkpoint 03 — `e6b0a752a82d7484b75083ad4a5279bc81342549`.
  - Active Pass 4 / Checkpoint 04 — `23e479d0988f0e289650aae718b55a9182cdb11e`.
  - Pass 3 closure / Checkpoint 23 — `2f356c06db43c84e5112c7a23c4bb51240c55563`.
  - `translation/README.md` — `f9bf34ae28e838ae881f9e207797f26c1b327baa`.
  - `translation/INVENTORY.md` — `0a8e58e9a6f45fd54c718672dc8d7a3a9056f7ee`.
  - `translation/EXECUTION-SEGMENTATION.md` — `05ee569421b89366bbc0993afe62cb22a0644913`.
  - Accepted Task 008 — `3b334e3537cb25dae7faffc0d6778b048d6529e6`.
  - Accepted Task 009 — `5662215f0ef445748c3174b1740c0b2efff7018e`.
  - Accepted Task 010 — `0357640e694f52b687d8d20b45270cdba1496cf1`.

### 2. Observer position

Trace the English **New project** and **Assessment only** entrypoint routes through Stage 0 — Project Scoping for the deliberately incomplete Android-tablet request. Determine what the documented sequence requires, which evidence and outputs can be identified from the skill, what remains unavailable without device-specific evidence or external research, and where user confirmation gates completion and device interaction. This is a documentation-only workflow trace; it does not assess an actual device or perform project setup.

### 3. Observations

**Entrypoint and Stage 0 route**

- The New project route (`SKILL.md`, lines 15–17) directs the agent to begin at Stage 0, reading the corresponding process section before relevant experience files.
- The Assessment only route (`SKILL.md`, line 21) explicitly says to complete Stage 0 and produce an assessment report and decision list. It does not define a separate abbreviated assessment procedure.
- The Stage 0 row in the entrypoint (`SKILL.md`, lines 25–27) summarises the activities and states the completion criterion: the user has confirmed the decision list; the device must not be touched before confirmation.
- The detailed Stage 0 section (`process.md`, lines 26–48) supplies the ordered sequence, outputs, completion criterion and relevant experience. No additional process document is needed to reconstruct the main sequence.

**Eight Stage 0 activities and their evidence requirements**

1. **Create the workspace** (`process.md`, line 30): create a private workspace, kernel repository and integration repository; write an initial `AGENTS.md`; agree commit author, accounts, repository names and licences with the user. The discipline reference identifies what `AGENTS.md` records, including device/target, user-confirmed decisions and authorisation boundaries (`discipline.md`, section 1, lines 7–18), and describes the private workspace and repository separation (section 4, lines 70–97). No workspace or repositories were created in this task.
2. **Verify device identity** (`process.md`, line 31): record the exact model, SoC model including suffix, bootloader unlockability, stock OS version, storage layout and A/B configuration. All six categories are absent from the hypothetical request. No device inspection is authorised or performed.
3. **Build a research pool** (`process.md`, lines 32–37): retrieve and pin URLs and commits for upstream SoC kernel support and relevant patch series; same-SoC community kernels and userspace projects; vendor open-source kernel and driver inventory/stock-source comparison; stock firmware packages with verified hashes; and comparable-device Linux projects, including their installation instructions, installers, retrospectives and technical-debt records. The task permits reconstruction of these evidence classes only; it expressly prohibits conducting the external research.
4. **Create a reuse-investigation table** (`process.md`, line 38): one row per feature, recording upstream status, community status (real-device result versus code only), vendor code, gaps and recommended source. No device-specific rows or recommendations can be populated from the supplied scenario.
5. **Create a hardware inventory** (`process.md`, line 39): the documented inventory covers panel, touch, GPU, wireless chip, amplifier and codec, fuel gauge, backlight, PMIC, sensors, keyboard bridge, camera and fingerprint sensor. The inventory fields are reconstructable; the actual components and their identities are not.
6. **Select technical approaches** (`process.md`, line 40): selection is to follow `decisions.md` and record reasons. The general criteria (`decisions.md`, section 1, lines 21–35) consider upstream proximity, rollback, real-device evidence, maintenance, replaceability, minimality, user impact, licence and temporary-solution exit conditions; important decisions record options, trade-offs, choice, rationale and reassessment triggers. These criteria do not supply missing device facts or justify choosing an approach for this tablet.
7. **Estimate workload** (`process.md`, line 41): the process requires ranges and identification of the largest uncertainties, using the method in `discipline.md`. The task's permitted discipline evidence is limited to sections 1 and 3; the referenced estimation method was not inspected. The requirement and its dependency are identifiable, but the method itself is not evaluated in this handoff.
8. **Confirm decisions with the user** (`process.md`, lines 42–46): present the decisions together, obtain confirmation and record confirmed decisions in `AGENTS.md`. This is the explicit Stage 0 completion criterion. No confirmation exists in the hypothetical, and this worker was expressly prohibited from asking the user or treating the criterion as met.

**Assessment decision list and confirmation boundary**

- The entrypoint (`SKILL.md`, lines 80–91) requires the agent to present all items together, with options, trade-offs and a recommendation for each, obtain confirmation, and record the confirmed decisions in `AGENTS.md`. The eight items are: data erasure and backup scope; single- or dual-system installation and Android retention; partition sizes; distribution and desktop environment; debug-channel default and preconfigured account; release accounts/repositories/licence/vendor-firmware distribution; workload, major risks and missing prerequisites; and operations permitted while the user is absent.
- The hypothetical supports identifying this decision list and the fact that its device-dependent options cannot yet be grounded. It does not support recommendations, partition sizes, backup scope, installation mode, distribution choice, workload ranges or unattended-operation permissions.
- The general decision criteria are guidance for evaluating later options, not substitutes for the evidence required to choose among them. The discipline reference requires user-made decisions to be recorded in `AGENTS.md` (`discipline.md`, section 3, lines 50–60); section 1 also defines the records and authorisation boundaries that `AGENTS.md` must contain.
- The task's user-confirmation boundary is therefore reconstructable: Stage 0 must not be declared complete until the user confirms the decision list, and the entrypoint explicitly says not to touch the device before that confirmation. The hypothetical contains neither the necessary device facts nor the confirmation.

**Comparable-project and reuse experience**

- The directly relevant experience entry on designing without first examining comparable projects (`testing-and-process.md`, lines 94–98) says to read installers, retrospectives and technical-debt records for same-SoC projects and projects by the same author before designing a process.
- The foundation-comparison entry (`testing-and-process.md`, lines 100–104) identifies device-tree approach, resource-management model, init and installation method as comparison dimensions before reusing community work. This gives context to the reuse-investigation activity; it does not identify any suitable project for the unspecified tablet.

**Assessment-only output relationship**

- The route itself is explicit that Assessment only means completing Stage 0 (`SKILL.md`, line 21). Stage 0's output list (`process.md`, line 44) includes the workspace, research-pool inventory, reuse-investigation table, hardware inventory, technical-selection record, assessment report and `AGENTS.md`; its completion criterion is user confirmation (line 46).
- There is a scope ambiguity worth preserving: the entrypoint describes the assessment-only deliverables as an assessment report and decision list, while the referenced Stage 0 process lists the broader workspace and project-record outputs, and its first activity explicitly creates both repositories. The route contains no assessment-only exception or statement that those outputs are omitted. A literal reading carries the full Stage 0 sequence and outputs into Assessment only, but the intended treatment of repository/workspace creation for a request that is explicitly assessment-only is not separately explained. This is an actual documentation ambiguity, not a missing fact about the hypothetical device.
- The workload-estimation method is a further evidence limitation for this bounded trace: Stage 0 points to `discipline.md`, but the task authorises only sections 1 and 3 for this purpose. No unpermitted section was loaded. The handoff records the dependency without expanding the evidence boundary.

### 4. Decisions

- **Procedural finding:** the principal Stage 0 sequence is reconstructable. The entrypoint routes both New project and Assessment only to Stage 0; the process reference specifies its eight activities, outputs and confirmation-based completion criterion; the permitted decision and discipline references establish the general selection and recording constraints.
- The hypothetical can be advanced only as a procedural plan. It does not permit a feasibility conclusion or device-specific technical selection. In particular, the missing model, exact SoC, unlock state, stock OS version, storage layout and A/B configuration prevent the documented device-identification and evidence-gathering activities from being completed.
- External upstream, same-SoC, vendor and comparable-project research is an identified Stage 0 requirement, but was not performed because the task explicitly excludes external research. The same applies to firmware retrieval/hash verification and any real hardware inventory.
- The user decision list can be reconstructed, but its options, trade-offs and recommendations cannot responsibly be completed from the scenario. User confirmation remains a hard completion gate; no device interaction is permitted before it.
- The Assessment-only/workspace output relationship is recorded as ambiguous rather than silently resolving the apparent scope tension. No documentation text is changed under this task.
- No actual project workspace, repositories, research pool, inventories, assessment report or project records were created. This is a workflow trace, not execution of Stage 0.

### 5. Unresolved matters

- The six device-baseline facts remain absent: model, exact SoC (including suffix), bootloader unlockability, stock OS version, storage layout and A/B configuration. Further device-specific evidence would be required before technical selection or a feasibility conclusion.
- The required external research has not been performed and is outside this task. Its results, pinned revisions, hashes and real-device/community evidence remain unknown.
- The actual hardware inventory, reuse table, technical approaches and workload ranges cannot be populated from the hypothetical. The Stage 0 workload step references a method in `discipline.md` that lies outside the sections authorised for this task; its detailed method has not been evaluated.
- The intended Assessment-only treatment of Stage 0's workspace, kernel-repository and integration-repository outputs remains ambiguous. The current literal route includes Stage 0, but does not explain whether an assessment-only request should create those artifacts.
- No user confirmation is present. Stage 0 completion is therefore not met; this handoff does not claim an assessment report or project assessment has been completed.
- No external investigation proposal arose beyond the unresolved documentation questions above. External technical validity, feasibility and operational usability were not investigated.

### 6. Validation

- Exact blob SHA matches were confirmed for all five pinned English evidence files listed in Task 011. The pinned English tree was independently retrieved by exact SHA `ba8a19206173ccde8fd26b6ae731ca4a0ee8c7cc`; GitHub returned the same tree SHA and `truncated: false`.
- Exact blob SHA matches were confirmed for the active Checkpoint 04, Checkpoints 01–03, Pass 3 closure, README, inventory, execution-segmentation specification, and accepted Task 008 / 009 / 010 records. The pending task record itself matched its pre-handoff SHA `102db73d29daa7bf0f805eb81f932966a1d53ace` before this append.
- Route and process evidence locations:
  - `SKILL.md`: New project / Assessment only, lines 15–21; Stage 0 summary, lines 25–27; user decision list, lines 80–91.
  - `process.md`: Stage 0 goal and eight activities, lines 26–42; outputs, completion criterion and relevant experience, lines 44–48.
  - `decisions.md`: general decision criteria, lines 21–35.
  - `discipline.md`: required records, section 1, lines 7–18; user decision recording and handoff requirements, section 3, lines 50–68.
  - `testing-and-process.md`: comparable-project and reuse-foundation entries, lines 94–104.
  - `README.md`: Phase 5 representative workflow and exit condition, lines 87–95.
- The hypothetical was traced without adding device facts. Required evidence, documented steps, outputs, user decisions, missing scenario inputs and stop points were kept distinct.
- No external websites or repositories were consulted for technical research; no device was inspected or operated; no project artifacts were created. Validation establishes only the procedural reconstructability and the identified documentation ambiguity for this one scenario. It does not establish semantic correctness, technical currency, feasibility, operational safety or actual assessment quality.

### 7. Change boundary

- Appended this handoff to `translation/tasks/011-phase5-new-device-assessment-workflow.md` only.
- No files under `translation/en/`, Chinese source files, README, inventory, glossary, reviews, metadata, checkpoints, current-position records, execution-segmentation specification or other task records were modified.
- No external research, device inspection, device operation, workspace creation, repository creation or assessment artifact creation was performed.
- The task record is to be moved unchanged except for this appended handoff from `translation/tasks/` to `translation/tasks/completed/`. That move records worker execution only; it does not constitute orchestrator acceptance.

### 8. Next dependency

The orchestrator should independently reconstruct and evaluate this handoff, especially:
- whether the Assessment-only route's lack of an explicit exception for Stage 0 workspace/repository outputs warrants a separately bounded documentation clarification;
- whether a follow-up evidence boundary should authorise the relevant workload-estimation method in `discipline.md`;
- whether the project should proceed to the remaining representative project-continuation workflow and other outstanding Phase 5 checks.

No device-specific assessment can proceed from this scenario without the missing baseline facts, external evidence and subsequent user decisions. This worker does not select the next task, update project continuation position or claim acceptance.
