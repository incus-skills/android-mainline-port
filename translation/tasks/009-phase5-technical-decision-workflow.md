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


---

## Worker handoff — Task 009

### 1. Task identity

- **Execution status:** COMPLETED — the bounded documentation walk-through and handoff are complete. The hypothetical scenario intentionally stops before any device-specific decision; this is the expected evidence boundary, not an attempt to supply missing facts.
- **Traversal / checkpoint:** Pass 4 / Checkpoint 02; Phase 5 — Whole-skill validation.
- **Task:** `translation/tasks/009-phase5-technical-decision-workflow.md`; pre-handoff blob SHA `63fa9b2262878f96521398b5c6aff156f6175737`.
- **Pinned evidence:** `translation/en/SKILL.md` = `f613aa7ef89eb9204c4f9804faf7aebace596a4e`; `translation/en/references/decisions.md` = `3e63f6f4b54ab987c9e438dbf015d4f7f72d85c9`; `translation/en/references/discipline.md` = `078e020ad497e9ec69388ad55c5beeb4677fd22a`.
- **Position evidence:** Pass 4 / Checkpoint 01 = `598722e61e115068af5c90d90dbdf8224dd7c764`; active Pass 4 / Checkpoint 02 = `e2eee68d113988b3aed8a01fb03682fa130aa1f0`; accepted Task 008 record = `3b334e3537cb25dae7faffc0d6778b048d6529e6`.
- **Governing references:** `translation/README.md` = `f9bf34ae28e838ae881f9e207797f26c1b327baa`; `translation/INVENTORY.md` = `0a8e58e9a6f45fd54c718672dc8d7a3a9056f7ee`; `translation/EXECUTION-SEGMENTATION.md` = `05ee569421b89366bbc0993afe62cb22a0644913`; pinned `translation/en/` tree = `ba8a19206173ccde8fd26b6ae731ca4a0ee8c7cc`.

### 2. Observer position

Trace the English technical-decision route for the single **Backup and Root Access** decision. Determine whether an agent can distinguish the documented default, conditional rules, rationale, exception, missing device inputs and user-confirmation boundary in the supplied incomplete scenario. This is a procedural documentation check only. It does not decide whether a particular device can be ported, assess technical correctness or perform device operations.

### 3. Observations

- The entrypoint directs a technical-decision workflow to the default approach and rules in the relevant section of `references/decisions.md` (English `SKILL.md`, line 19). Its Stage 0 completion criterion requires the user to confirm the decision list and says not to touch the device before confirmation (line 27).
- The entrypoint's user-input section says to present the decisions together after assessment, with options, trade-offs and a recommendation; confirm them with the user; then record confirmed decisions in `AGENTS.md` (lines 80–90). Its first listed decision is whether data may be erased and which partitions should be backed up.
- The general decision criteria apply to every decision (decisions.md, lines 21–35): consider upstream proximity, rollback, real-device evidence, maintenance, replaceability, minimality, user impact, licensing and temporary-solution exit conditions. The record also says to record options, trade-offs, choice, rationale and reassessment triggers.
- The complete **Backup and Root Access** record is present (decisions.md, lines 41–50):
  - **Default:** obtain root access first using Magisk or KernelSU by modifying `init_boot`; back up partitions individually within the scope confirmed by the user (usually excluding `userdata` and `super`); save primary and backup GPTs separately for every LUN; verify matching hashes on host and device.
  - **Conditional rules:** do not build a minimal stock-kernel RAM probe for partition reading; if Android cannot or should not be rooted, the stated alternative is a temporarily booted rooted recovery with block devices set read-only; a downloadable and verifiable official OTA may reduce the backup scope for system partitions, while device-specific data such as `persist` and calibration data still require separate backup; if dual boot requires Android to write boot partitions, use a root method modifying only `init_boot` and re-patch it after Android updates.
  - **Rationale:** establish a recovery path before persistent writes; the record explains why a minimal stock-kernel probe may fail to obtain USB.
  - **Exception:** if a vendor recovery tool can restore every partition without loss, including calibration data, backup scope can be reduced, while device-specific data must still be backed up.
- The relevant working-discipline requirements make `AGENTS.md` the record for device/target, user-confirmed decisions and authorisation boundaries (discipline.md, section 1); when the user makes a decision, record it there (section 3). Before a device session, expected results and decision criteria are recorded; results and evidence locations are recorded on the same day. Real-device conclusions require source commit, image hash, test scenario and evidence location.
- In the hypothetical scenario, none of the facts needed to select among the conditional paths is supplied: device identity/model, partition and LUN layout, whether Android can or should be rooted, whether a rooted recovery can be booted, whether dual boot requires Android to write boot partitions, whether a verifiable OTA or lossless vendor recovery tool exists, the recovery path, or the user's authorised backup/erasure scope.
- These omissions are not themselves documentation defects. The task deliberately withholds them; the record's conditional rules and user-confirmation requirements make clear that they cannot be assumed. The documented default is not an automatic device-specific recommendation.
- The decision record contains the general criteria, default, rules, rationale and exception. It does not state separate acceptance criteria for this decision; the introductory instruction asks for acceptance criteria **where needed**, so their absence is not treated as a defect.

### 4. Decisions

- **Procedural finding:** the documented route is followable for this bounded decision up to the point where device facts and user authority are required. The entrypoint identifies where to read; the decision record supplies the decision structure; the discipline and entrypoint specify how user decisions are confirmed and recorded.
- An agent may explain the documented default and identify the conditions that would change the path. It may not infer that the default applies to the hypothetical device, choose a rooting/recovery route, define the backup set, or conclude that the device is portable from the supplied information.
- The next information needed is evidence for the specific conditions above, including the device's actual partition/LUN layout and available recovery path, plus the user's explicit decision on data erasure and backup scope. The user must confirm the decision list before device interaction.
- The required stop point is before recommending or performing a device-specific root, recovery or backup operation. No commands or operations are proposed.
- No documentation correction is justified within this task. No acceptance criteria are invented for a record that does not prescribe them.

### 5. Unresolved matters

- All device-specific facts and the user's backup/erasure authorisation remain unknown by design.
- Whether any stated root, recovery, OTA or vendor-restoration route is technically available or safe on a real device is outside this task and requires separate evidence.
- This walk-through does not establish port feasibility, technical currency, operational safety, or actual workflow usability.

### 6. Validation

- Exact blob SHA matches confirmed for all pinned files: entrypoint `f613aa7ef89eb9204c4f9804faf7aebace596a4e`; decision reference `3e63f6f4b54ab987c9e438dbf015d4f7f72d85c9`; discipline reference `078e020ad497e9ec69388ad55c5beeb4677fd22a`; README `f9bf34ae28e838ae881f9e207797f26c1b327baa`; inventory `0a8e58e9a6f45fd54c718672dc8d7a3a9056f7ee); Checkpoint 01 `598722e61e115068af5c90d90dbdf8224dd7c764`; Checkpoint 02 `e2eee68d113988b3aed8a01fb03682fa130aa1f0`.
- The pinned English tree was retrieved by its exact tree SHA `ba8a19206173ccde8fd26b6ae731ca4a0ee8c7cc`; GitHub returned the same SHA, 22 file blobs and `truncated: false`. The three task-specific English files appear in that tree at their expected blob SHAs.
- The active checkpoint confirms Task 008 was accepted and identifies Task 009 as the next bounded representative technical-decision workflow. The accepted Task 008 record and the current Pass 4 checkpoint agree on that position.
- The eight handoff fields follow `translation/EXECUTION-SEGMENTATION.md`. The walk-through was confined to the designated evidence; no upstream, device, vendor or other external technical sources were consulted.
- Validation distinguishes documentation route coherence from semantic correctness, current technical validity, device-specific applicability and actual operational usability.

### 7. Change boundary

- Appended this handoff to `translation/tasks/009-phase5-technical-decision-workflow.md` only.
- No files under `translation/en/`, Chinese source files, README, inventory, glossary, reviews, metadata, checkpoints, current-position records or other task records were modified.
- No device commands or operations were performed.
- The task remains in `translation/tasks/`; it has not been moved to `completed/`. Worker completion does not constitute orchestrator acceptance.

### 8. Next dependency

No additional repository evidence is required to complete this bounded documentation walk-through. The orchestrator should independently reconstruct and evaluate this handoff. Any device-specific recommendation would require the missing device evidence and explicit user decisions identified above, under a separately authorised operation. Other Phase 5 representative workflows and metadata compatibility remain separate work.
