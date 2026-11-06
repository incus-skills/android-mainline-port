# Context-segmented execution for translation work

## Status

This document records a proposed execution arrangement for the translation project. It does not alter the Chinese source, redefine the translation passes, or replace the existing source-to-target review discipline. Its effectiveness remains to be observed in use.

## Purpose

Reduce repeated context loading during document-level work without weakening source fidelity, project continuity, or whole-skill coherence.

The working hypothesis is that a single-file operation should not need to reconstruct the entire repository and the entire prior conversation. The project position can be maintained by a thin orchestrator, while bounded workers receive only the evidence and constraints required for their assigned operation.

## Responsibility separation

### Orchestrator

Maintains the small, durable control position:

- repository, branch and relevant source baseline;
- current pass, purpose, scope and observer position;
- current checkpoint and next document;
- applicable global constraints and exclusions;
- task dependencies and outstanding decisions;
- worker results and integration status.

The orchestrator routes and integrates work. It does not repeat each worker's document-level reasoning or keep every source document active.

### Bounded file worker

Receives a task packet for one source/target unit, normally one document pair. The packet includes:

- exact source path and revision or commit;
- exact English target path and revision;
- the current pass purpose and observer position;
- applicable translation rules and glossary entries;
- directly relevant dependencies or cross-references only;
- allowed files and operations;
- validation criteria and stop conditions.

For Pass 3, the worker compares the Chinese source and English target for semantic fidelity, imperative force, conditional scope, decision logic, technical tokens, structure, cross-references and uncertainty. It does not expand into external technical validation.

A worker may be a separate agent, a fresh context, a sub-agent, or a manually bounded operation. These are execution choices, not methodological requirements.

### Integration check

The orchestrator or a separately assigned reviewer checks the returned change against the relevant project-wide constraints:

- source remains unchanged;
- target corresponds to the intended source revision;
- terminology and links remain coherent where relevant;
- no excluded technical audit or redesign has entered the translation;
- the worker's unresolved issues and provenance are retained;
- the change is suitable for acceptance or requires another bounded hop.

Integration does not mean re-reading the entire repository after every file. It means selecting the wider evidence required by the actual dependency and risk of the change.

## Evidence boundary and separate investigation proposals

The acceptance-bound reconstruction and any external investigation proposal are distinct outputs of the same traversal.

For source-grounded translation review, every claim, distinction, technical token, qualification and proposed correction must be traceable to explicit evidence in the designated source or target. The source establishes what it says; the target establishes what the translation says; a finding describes only a correspondence demonstrable from those two. External knowledge may prompt a separate question, but it cannot establish a translation finding or justify a translation change.

Before any target or review modification, identify the exact source and/or target evidence that necessitates it. If no such evidence can be identified, do not make the change. This rule applies to corrective passes as well: correction inherits the original evidence boundary and does not expand it.

Workers and orchestrators may also notice questions or opportunities that require evidence beyond the current translation task, such as current upstream validation, real-device testing, time-sensitive technical review, improvement or extension of the skill. These may be recorded as **investigation proposals**, separately from source-grounded findings and acceptance decisions.

A proposal should retain enough provenance to reconstruct:
- where it originated (worker/task/document or orchestrator/integration observation);
- what observation prompted it;
- the question or opportunity;
- what external evidence or test would be needed;
- the possible consequence if resolved.

Proposals are not findings, corrections, task instructions or acceptance evidence. They do not alter the translation task's scope, the Pass 3 criteria or the acceptance boundary. They may provide a starting point for separately bounded work after or alongside translation, when that work is explicitly selected. Do not create a proposal merely to fill a record, and do not introduce a proposal-management system before actual use demonstrates a need.

## Worker handoff record

Each completed operation returns:

1. **Task identity:** pass, checkpoint, source/target paths and revisions.
2. **Observer position:** the question being asked and the scope explicitly excluded.
3. **Observations:** source/target distinctions and relevant evidence.
4. **Decisions:** proposed changes, with reasons and source correspondence.
5. **Unresolved matters:** ambiguity, uncertainty and questions not resolved.
6. **Validation:** checks performed, results and evidence locations.
7. **Change boundary:** files changed and files deliberately left untouched.
8. **Next dependency:** whether another file or wider check is actually required.

The handoff should be concise but reconstructable. It is not a substitute for preserving the actual source, target, diff or review record.

## Context loading policy

Use four practical context bands:

| Band | Content | Loading rule |
|---|---|---|
| Project control | Pass, scope, observer position, checkpoint, next hop | Load when establishing or re-establishing the orchestrator |
| Global constraints | Translation discipline, glossary, source authority, exclusions | Load once for the orchestrator; include relevant portions in each worker packet |
| File evidence | Source, target, existing file-specific review and direct dependencies | Load for the assigned file operation |
| Wider integration evidence | Other files, indexes, links, cross-file terminology and repository state | Load selectively when a concrete dependency or integration check requires it |

A file worker must not assume that omitted context does not exist. It must report when its task cannot be adequately completed without additional evidence and stop rather than silently broaden its scope.

## Application to the current position

The current translation position remains Pass 3 / Checkpoint 16:

- last completed: `experience/audio.md`;
- next: `experience/wireless.md`;
- next operation: source-to-English semantic and technical translation review;
- external technical correctness and real-device validation remain excluded.

The first bounded operation should receive the Chinese wireless source, the English wireless target, the relevant glossary and only those dependencies identified by the source or existing review record. The orchestrator retains the pass position and integrates the result.

## What this does not change

- Pass numbering and checkpoint meaning.
- The declared scope or purpose of Pass 3.
- The Chinese source's authority.
- The distinction between translation, correction and external technical validation.
- The requirement to reconcile whole-skill scope at the appropriate validation stage.
- The need to preserve provenance and unresolved questions.

## Evaluation questions

Observe whether this arrangement:

- reduces repeated repository and conversation loading;
- preserves the worker's ability to understand the assigned source/target pair;
- makes handoffs reconstructable by a different observer or runtime;
- exposes dependencies instead of hiding them;
- avoids excessive integration overhead;
- preserves the existing adequacy, sufficiency and completeness distinctions.

Do not treat the proposed segmentation as validated merely because the task boundaries have been written down.
