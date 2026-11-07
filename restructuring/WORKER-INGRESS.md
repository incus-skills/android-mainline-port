# Restructuring worker session ingress

## Purpose

This is the entry point for an independent worker session executing one bounded Android Mainline Port restructuring task.

The worker must be able to reconstruct its assignment from repository records without access to the orchestrator's conversation. The governing objective is in [README.md](README.md); the current coordination position is in [current-position.md](current-position.md).

## Entry and selection

1. Read [the governing objective](README.md) to understand why restructuring work exists.
2. Read [current-position.md](current-position.md) only far enough to verify the task's current status and any global constraints relevant to the assignment.
3. Read the exact task path supplied by the orchestrator. Do not scan or reread the entire task history to find work.
4. Verify the task's ingress branch/commit, file revisions and governing references before acting.
5. Read only the supporting evidence named by the task, plus dependencies shown to be necessary.
6. If the task is not clearly available, its evidence pins do not match, its scope conflicts with current repository state, or a required dependency is missing, stop and report the precise discrepancy. Do not substitute newer evidence silently.

The orchestrator must identify the assigned task path. Do not independently select a task from an ambiguous directory state.

## Execution

- The task record is the authority for local purpose, observer position, scope, permitted changes, validation and stop conditions.
- Work only on the assigned operation. Do not expand into adjacent restructuring, translation, provisioning or product work because related material is visible.
- Preserve the source-language product, English package and historical records unless the task explicitly authorises a specific change.
- Distinguish observed facts, owner decisions, reconstructed relationships and unresolved questions.
- Do not invent distribution, provisioning, installation, discovery or runtime behaviour.
- Record exact revisions and provenance for material evidence and every changed artifact.
- If a new dependency makes the task unsafe or incomplete, stop and report it rather than silently broadening the assignment.

## Handoff

Append a reconstructable handoff to the assigned task record using the task's specified handoff fields. At minimum, report:

1. operation performed and result;
2. exact ingress commit and relevant evidence revisions;
3. files changed, with paths and resulting identities;
4. observations and reasoning, separated from owner decisions;
5. validation performed and its limits;
6. unresolved matters, blockers and stop conditions;
7. explicit non-actions where they delimit the result;
8. the worker's completion disposition.

Do not claim orchestrator acceptance. Worker completion and orchestrator acceptance are separate.

Follow the task's prescribed task-record transition. Do not independently invent or alter task lifecycle rules. The current restructuring task directories are undergoing lifecycle reconciliation; until the orchestrator provides an unambiguous transition for the assigned task, leave task placement unchanged and report completion in the task record as instructed.

## Boundaries

- Do not modify the current-position record or orchestrator ingress.
- Do not create, assign or dispatch other tasks.
- Do not decide unresolved owner choices.
- Do not modify the dedicated English branch or source-language branch unless explicitly authorised by the task.
- Do not resume translation work.
- Do not treat successful execution as acceptance or overall project completion.
