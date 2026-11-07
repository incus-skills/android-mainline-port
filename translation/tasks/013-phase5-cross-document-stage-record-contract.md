# Task 013 — Phase 5 cross-document stage and record-reconciliation contract review

## Purpose and observer position

Perform one bounded cross-document consistency/dependency review under Pass 4 — whole-skill English validation.

Task 012 found that the pinned entrypoint, process and discipline documents do not explicitly define a current-stage field or a rule for reconciling missing, stale or conflicting project records. Determine whether other documents in the pinned English skill tree supply such a rule, whether the process's permitted stage overlap is represented coherently, and whether Stage 8 has a completion condition elsewhere in the tree.

Use the same deliberately incomplete, documentation-only continuation scenario:

> An agent is asked to continue an existing Android mainline port. The project is said to contain an AGENTS.md, a status table, a technical-debt table, a work log and per-feature documents. No actual project records or device details are supplied. The handoff does not state a process stage. Consider also the possibility that the records, if later supplied, disagree about the next step or current device state.

This task tests the cross-document contract, not an actual project's state. It is documentation-only and read-only.

## Position and governing criteria

- Traversal: **Pass 4 — whole-skill English validation**.
- Project phase: **Phase 5 — Whole-skill validation**.
- Current position: Pass 4 / Checkpoint 06.
- Accepted operations: Task 008 — entrypoint/navigation integrity; Task 009 — technical-decision workflow; Task 010 — troubleshooting workflow; Task 011 — new-device assessment workflow; Task 012 — existing-project continuation workflow.
- Pass 4 / Checkpoint 01: reconstruction/zh/continuation/checkpoints/pass-4-checkpoint-01.md, blob SHA 598722e61e115068af5c90d90dbdf8224dd7c764.
- Previous Pass 4 / Checkpoint 05: reconstruction/zh/continuation/checkpoints/pass-4-checkpoint-05.md, blob SHA bd80287c4db36295b5883ea0153b5fed9f1ebe8a.
- Current Pass 4 / Checkpoint 06: reconstruction/zh/continuation/checkpoints/pass-4-checkpoint-06.md, blob SHA c91664de1db8e06af3341292748d5123b92362af.
- Pass 3 closure: reconstruction/zh/continuation/checkpoints/pass-3-checkpoint-23.md, blob SHA 2f356c06db43c84e5112c7a23c4bb51240c55563.
- Phase definitions and Phase 5 criteria: translation/README.md, blob SHA f9bf34ae28e838ae881f9e207797f26c1b327baa.
- Context-segmentation specification: translation/EXECUTION-SEGMENTATION.md, blob SHA 05ee569421b89366bbc0993afe62cb22a0644913.
- Pinned English tree: translation/en/, tree SHA ba8a19206173ccde8fd26b6ae731ca4a0ee8c7cc. The recursive tree contains 22 file blobs and is not truncated.
- Primary records already reviewed in Task 012:
  - translation/en/SKILL.md — f613aa7ef89eb9204c4f9804faf7aebace596a4e.
  - translation/en/references/process.md — 3e770386533dda9f0b5162043c08e9ffea454467.
  - translation/en/references/discipline.md — 078e020ad497e9ec69388ad55c5beeb4677fd22a.
- Accepted Task 012 record: translation/tasks/completed/012-phase5-project-continuation-workflow.md, blob SHA d2a86399ab84a22f8d31e198157d3507f64b69ad.

## Exact evidence boundary

Use only:

1. The complete pinned English tree translation/en/ at tree SHA ba8a19206173ccde8fd26b6ae731ca4a0ee8c7cc. Verify the recursive tree is complete and enumerate its file paths and blob SHAs before analysis.
2. Pass 4 / Checkpoints 01, 05 and 06, plus Task 012's accepted handoff, only for position, provenance and the exact unresolved questions carried forward.
3. translation/README.md and translation/EXECUTION-SEGMENTATION.md only for Phase 5 scope and orchestration boundaries.

Do not use Chinese source files, upstream repository material, actual project records, external documentation, issue trackers or web search. Do not assume that a rule exists because a workflow would benefit from one. If a required dependency lies outside the pinned evidence boundary, identify it and stop.

## Scope of work

1. Reconstruct the Task 012 findings and their exact scope. Preserve the distinction between a missing rule in the three documents reviewed there and a rule that may exist elsewhere in the English tree.
2. Enumerate the 22 file blobs in the pinned English tree and scan the tree for relevant statements concerning:
   - current process stage, stage identification and stage completion;
   - parallel or overlapping stage activity;
   - status tables, progress records and feature completion;
   - AGENTS.md, work logs, technical debt, abandoned approaches and experiment records;
   - handoff, next step, blockers, device state, active operator and running tasks;
   - user-confirmed decisions and unattended-operation authorisation;
   - missing, stale, inconsistent or conflicting records;
   - Stage 8 maintenance activities and completion.
3. Read the complete documents needed to interpret every potentially relevant match. Record exact file paths, sections/lines and blob SHAs. Do not infer the absence of a rule from a partial excerpt or a search result alone.
4. Determine whether any document:
   - defines an explicit current-stage field or another canonical stage representation;
   - provides a deterministic method for inferring stage from feature-level evidence;
   - explains how concurrent stage activity is represented;
   - defines precedence among project records or a procedure for resolving conflicting values;
   - specifies recovery for missing or stale records, including when to mark information unknown or seek responsible-person/user adjudication;
   - provides a completion criterion for Stage 8.
5. Compare any relevant rules found across documents. Distinguish explicit requirements, recommendations, examples and implications. Identify direct contradictions, duplicated rules with differing force, incomplete links and unresolved dependencies. Do not invent a precedence rule or promote an implication into a requirement.
6. Revisit the incomplete scenario without adding project or device facts. State what can be established from the documentation, what still requires actual project records, and the safe stop boundary if stage, device state, operator, running tasks or authorisation cannot be established.
7. Decide whether Task 012's observed gaps are:
   - resolved elsewhere in the pinned English tree;
   - partially addressed but still ambiguous;
   - confirmed as documentation gaps within the pinned tree; or
   - not decidable because a necessary dependency is outside the evidence boundary.
8. Report findings only. Do not repair text, create a new policy, prescribe a schema, or change any skill/project artifact.

## Exclusions

- Do not perform an actual project takeover or inspect any project workspace.
- Do not invent AGENTS.md, status-table, feature-record, work-log or technical-debt contents.
- Do not assess any real Android device, technical feasibility, upstream support or current technical correctness.
- Do not conduct external research or use Chinese source files.
- Do not operate or simulate a device.
- Do not modify the English tree, Chinese source, README, inventory, glossary, metadata, checkpoints, current-position records or prior task records.
- Do not reopen Pass 3 translation review.
- Do not repeat or reassess the already accepted representative workflows except where Task 012's specific unresolved contract questions require a cross-document reference.
- Do not assess source-language leakage or installation/skill-discovery metadata; those remain separate Phase 5 operations.
- Do not claim that a documentation review proves actual takeover success, operational safety or context-segmentation effectiveness.

## Permitted changes

- Read-only with respect to skill content and project source material.
- Append the required eight-field handoff to this task record only.
- Once execution and handoff are complete, move this task record unchanged except for the appended handoff to translation/tasks/completed/. This records worker execution only and does not imply orchestrator acceptance.

## Stop conditions

Stop and report if:

- the pinned English tree cannot be retrieved exactly or its recursive listing is truncated;
- any relevant file differs from its pinned blob SHA;
- a potentially necessary rule depends on Chinese source, external material or actual project records;
- a relevant document cannot be retrieved in full;
- conflicting instructions prevent reconstructing the record or stage-identification boundary;
- the work would require a policy decision, text correction, actual project takeover or scope expansion.

## Validation

- Confirm the exact tree SHA, recursive-tree completeness, all 22 file paths and their blob SHAs.
- Confirm the three primary Task 012 evidence files and all additional documents used.
- Record exact sections/lines for each explicit rule or relevant absence; distinguish search discovery from full-document verification.
- Assess whether each Task 012 gap is resolved, partially addressed, confirmed within the pinned tree, or outside the available evidence.
- Distinguish explicit rule from implication, recommendation, example and absent scenario data.
- Preserve the distinction between procedural coherence, semantic correctness, current technical validity and actual project takeover success.
- Record the read-only change boundary and any unresolved dependency.

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
