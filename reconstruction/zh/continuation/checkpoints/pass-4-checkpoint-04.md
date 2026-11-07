# Pass 4 — Checkpoint 04: Task 010 accepted; new-device assessment workflow next

## Position

- Traversal: **Pass 4 — whole-skill English validation**.
- Project phase: **Phase 5 — Whole-skill validation**.
- Previous checkpoint: Pass 4 / Checkpoint 03.
- Accepted operation: **Task 010 — representative troubleshooting workflow trace**.
- Acceptance status: **ACCEPTED**, after independent reconstruction of the worker handoff, exact baseline comparison and direct evidence review.
- Next bounded operation: **Task 011 — representative new-device assessment workflow**.
- This checkpoint does not close Pass 4 or establish whole-skill usability.

## Task 010 — independent reconstruction and acceptance

### Handoff and change boundary

- Task preparation commit: `483438ac50630ad246c9caded85d5551824c64f1`.
- Worker completion commit: `a58eeec1560efffd7d4e25c3ced9959e44f5c89e`.
- The completion commit changes only `translation/tasks/010-phase5-troubleshooting-workflow.md), appending the eight-field handoff. The original task content was retrieved from its recorded pre-handoff blob `1a3ce44a04a1f11a273a6ab6f55b87596e5027eb`; it is an exact prefix of the current handoff-bearing content.
- The handoff-bearing task blob is `0357640e694f52b687d8d20b45270cdba1496cf1`.
- After acceptance, the task was moved unchanged to `translation/tasks/completed/010-phase5-troubleshooting-workflow.md`. The completed record retains blob SHA `0357640e694f52b687d8d20b45270cdba1496cf1`. The pending copy was removed. The path move was recorded by commits `06dcffe8d0af85f2212967073969478c46554a39` and `8f3b1749fbf420e84bf76922e03d07f890f047ae`.
- No English skill content, Chinese source, inventory, README, glossary, review, metadata or prior checkpoint was changed by the worker completion commit or the subsequent task move.

### Independent evidence

The four pinned English file revisions were fetched independently and matched the task baselines:

- `translation/en/SKILL.md`: `f613aa7ef89eb9204c4f9804faf7aebace596a4e`.
- `translation/en/experience/patterns.md`: `7309618ad08085bb514b6800a580774f829fe312`.
- `translation/en/experience/usb-and-storage.md`: `7c3225d7a7f8650ce27a345922937cdadcf0cc59`.
- `translation/en/experience/README.md`: `a7a28190615253246749909b380ddcba17268769`.

The pinned English tree `ba8a19206173ccde8fd26b6ae731ca4a0ee8c7cc` was independently retrieved. GitHub returned the exact requested tree SHA with `truncated: false`; its recursive listing contains 24 entries, including 22 file blobs and the two subtree entries. All four required files are present at their expected blob SHAs.

The route and case were checked directly:

- The troubleshooting route in `SKILL.md` directs the reader to `experience/patterns.md`.
- The pattern index prescribes symptom recognition, general handling and then the corresponding domain entry. It explicitly allows a symptom to fit more than one category.
- Category D recognises old data appearing in a new system, includes old partition data among possible residual-state sources, prescribes clean-system reproduction and points to `usb-and-storage.md` for sparse-block retention.
- The named DONT_CARE entry repeats the hypothetical symptom and states its documented cause, resolution alternatives and applicability.
- The scenario does not establish that the documented cause applies to a real device. The handoff preserves this distinction.

### Acceptance decision

**ACCEPTED — Task 010, limited to procedural coherence of the single representative troubleshooting route for the documented sparse-image symptom.**

The entrypoint, pattern index, Category D and specific USB/storage entry form a reconstructable general-to-specific route. The worker correctly identified the scenario as a candidate for Category D without treating the documented cause as a confirmed diagnosis. The evidence boundary, read-only scope and handoff are adequate.

The specific entry contains no case-specific confirmation procedure or post-resolution acceptance test. The experience-library entry-format example includes a `How to confirm` field, but does not state that every entry must contain every illustrated field. This difference is therefore retained as an observation, not adjudicated as a defect. It does not prevent acceptance of the assigned route-coherence operation, and no separate correction task is justified by the available evidence.

Acceptance does not establish:
- technical correctness or current safety of the sparse-image explanation or resolution;
- that the documented cause applies to any real device;
- actual flashing, formatting, filesystem repair or recovery behaviour;
- all-category or whole-library troubleshooting usability;
- other Phase 5 workflows, cross-document consistency, source-language leakage or metadata compatibility;
- Pass 4 completion or effectiveness of context segmentation.

## Pass 4 position after Task 010

- Task 008 remains accepted for entrypoint/navigation structural integrity.
- Task 009 remains accepted for procedural coherence of the Backup and Root Access technical-decision route.
- Task 010 is now accepted for the single representative troubleshooting route.
- The three accepted operations have different validation boundaries; none implies completion of the other Phase 5 checks.
- Phase 5 still explicitly requires representative new-device assessment and project-continuation workflows, cross-document consistency/dependency checks, source-language leakage checks, and installation/skill-discovery metadata compatibility.
- The current-position record at the start of this reconstruction had blob SHA `9897a0c1f99a0127244ca6c30af6d146626cc496`. Its Task 010 line still described the task as pending and contained the literal unresolved text `${t.result.sha}` in place of a blob SHA. This is a continuity-record defect, not a defect in the worker handoff. The record is to be reconciled to this checkpoint and the persisted Task 011 revision.
- The task sequence was independently listed: Task 010 was the only pending task; completed records 001–009 were present. After moving Task 010, Task 011 is the next unused sequence number.
- The known stale “Application to the current position” paragraph in `translation/EXECUTION-SEGMENTATION.md` still points to Pass 3 / Checkpoint 16 / `experience/wireless.md`. The canonical current-position record and active checkpoints remain authoritative. This pre-existing discrepancy is retained; it is not silently repaired here.

## Next operation — Task 011

Task 011 will test the representative **new-device assessment** route under Phase 5. It will trace the “New project” and “Assessment only” entrypoint instructions through Stage 0, its required inputs and outputs, the user decision list, and the relevant record/confirmation boundaries.

The bounded hypothetical is a user asking whether an otherwise unspecified Android tablet can be ported, without supplying device identity, SoC, bootloader state, stock OS version or storage/A/B details. The worker must determine what the documentation allows an agent to establish, what evidence is still required, where user confirmation is required, and where assessment must stop. It must not invent device facts, perform external research, interact with a device or claim feasibility.

This is the next supportable hop because new-device assessment is an explicit Phase 5 representative workflow and is distinct from Task 009's single technical decision and Task 010's troubleshooting trace. Project continuation remains a separate representative workflow.

## Continuity reconciliation

Following acceptance:

- Prior checkpoints remain unchanged as historical records.
- This checkpoint records the newly accepted Task 010 position.
- Task 010's completed record is preserved unchanged from its handoff-bearing blob.
- Task 011 is prepared as the next pending numbered task.
- The current-position record is updated to identify Checkpoint 04, accepted Task 010, Task 011 as the next operation, and the exact Task 011 blob SHA.
- Pass 3 inventory entries are not changed: Task 010 is a Phase 5 workflow validation, not a Pass 3 translation review.
- The pending/completed task sequence is reconciled with the checkpoint and current-position record.
