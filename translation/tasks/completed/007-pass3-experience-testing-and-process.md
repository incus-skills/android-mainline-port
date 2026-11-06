# Task 007 — Pass 3 review of experience/testing-and-process.md

## Purpose and observer position

Perform the Pass 3 technical and semantic translation review of the testing, judgement, automation, collaboration and process experience document. Compare the complete Chinese source with its English translation. Preserve source meaning and operational force; do not validate technical claims against current upstream implementations, actual Android devices, or operational environments.

This is a bounded document-level operation within Pass 3, not a new pass, safety audit, test-system redesign or formalisation of collaboration mechanisms.

## Position and governing criteria

- Pass: Pass 3 — technical and semantic review of the operational English translation set.
- Previous accepted operation: Task 006 — Pass 3 review of `experience/build-and-release.md`; acceptance and correction provenance are recorded in [Checkpoint 22](../reconstruction/zh/continuation/checkpoints/pass-3-checkpoint-22.md) and [completed Task 006](completed/006-pass3-experience-build-and-release.md).
- Current position: Pass 3 / Checkpoint 22; `experience/build-and-release.md` is accepted and `experience/testing-and-process.md` is next.
- Current-position record: `reconstruction/zh/continuation/current-position.md`, blob SHA `2e3f467ae1b04ed9ca8fd98cd4a6463c037fb022`.
- Current checkpoint: `reconstruction/zh/continuation/checkpoints/pass-3-checkpoint-22.md`, blob SHA `4713edd899ad76d828121da96b5d620e26bd4983`.
- Previous checkpoint: `reconstruction/zh/continuation/checkpoints/pass-3-checkpoint-21.md`, blob SHA `55694ab10197f62e80f2e7532df1a98993054e59`.
- Pass 3 scope and review dimensions: `reconstruction/zh/continuation/checkpoints/pass-3-checkpoint-01.md`, blob SHA `0870d3ea261a6354649379b96891cda4ca2a0ba6`.
- Translation principles and Phase 4 instructions: `translation/README.md`, blob SHA `f9bf34ae28e838ae881f9e207797f26c1b327baa`.
- Controlled terminology: `translation/GLOSSARY.md`, blob SHA `7cd456b51223fd2cd3cf408cb14bae7f7d167254`.
- Execution boundary and handoff fields: `translation/EXECUTION-SEGMENTATION.md`, blob SHA `412268a0c065a628d65c0c2c1799830f718e92d1`.
- Source experience index: `experience/README.md`, `main`, blob SHA `b8b4b9110215280c35316df33d540e292c09356a`.
- English experience index: `translation/en/experience/README.md`, blob SHA `a7a28190615253246749909b380ddcba17268769`.
- Source pattern index: `experience/patterns.md`, `main`, blob SHA `c485e34f6fb06808d7c2678b5a2280494e5f71c0`.
- English pattern index: `translation/en/experience/patterns.md`, blob SHA `7309618ad08085bb514b6800a580774f829fe312`.

## Exact file evidence baseline

Fetch and verify these exact files before relying on them:

| Role | Repository path | Ref | Blob SHA |
|---|---|---|---|
| Chinese source | `experience/testing-and-process.md` | `main` | `ff1dbea0833dc9e9eb2f668000855f73ec0a9599` |
| English target | `translation/en/experience/testing-and-process.md` | `translation/english-reconstruction` | `391f94ab1508b02cd4b1acc8942ccef1e4600372` |
| Existing review (Pass 1) | `translation/reviews/experience/testing-and-process.md.md` | `translation/english-reconstruction` | `7e1d78eee519a7caef00fb5a837bc7b8c5c83c3c` |
| Pass 3 criteria | `reconstruction/zh/continuation/checkpoints/pass-3-checkpoint-01.md` | `translation/english-reconstruction` | `0870d3ea261a6354649379b96891cda4ca2a0ba6` |
| Current position | `reconstruction/zh/continuation/current-position.md` | `translation/english-reconstruction` | `2e3f467ae1b04ed9ca8fd98cd4a6463c037fb022` |
| Current checkpoint | `reconstruction/zh/continuation/checkpoints/pass-3-checkpoint-22.md` | `translation/english-reconstruction` | `4713edd899ad76d828121da96b5d620e26bd4983` |
| Previous checkpoint | `reconstruction/zh/continuation/checkpoints/pass-3-checkpoint-21.md` | `translation/english-reconstruction` | `55694ab10197f62e80f2e7532df1a98993054e59` |
| Completed preceding task | `translation/tasks/completed/006-pass3-experience-build-and-release.md` | `translation/english-reconstruction` | `8bc62329aab109f927accd1e9aac6b2307176403` |
| Inventory | `translation/INVENTORY.md` | `translation/english-reconstruction` | `e4ff4085e3d722e679729bc6102048030088e742` |
| Glossary | `translation/GLOSSARY.md` | `translation/english-reconstruction` | `7cd456b51223fd2cd3cf408cb14bae7f7d167254` |
| Translation instructions | `translation/README.md` | `translation/english-reconstruction` | `f9bf34ae28e838ae881f9e207797f26c1b327baa` |
| Segmentation arrangement | `translation/EXECUTION-SEGMENTATION.md` | `translation/english-reconstruction` | `412268a0c065a628d65c0c2c1799830f718e92d1` |
| Source experience index | `experience/README.md` | `main` | `b8b4b9110215280c35316df33d540e292c09356a` |
| English experience index | `translation/en/experience/README.md` | `translation/english-reconstruction` | `a7a28190615253246749909b380ddcba17268769` |
| Source pattern index | `experience/patterns.md` | `main` | `c485e34f6fb06808d7c2678b5a2280494e5f71c0` |
| English pattern index | `translation/en/experience/patterns.md` | `translation/english-reconstruction` | `7309618ad08085bb514b6800a580774f829fe312` |

The existing review is Pass 1 provenance, not a substitute for comparing the complete source and target afresh. Its Pattern E/F/H/I correspondences are explicitly attributed to the collection and pattern indexes; the source document body itself does not contain inline links to those indexes. Preserve this distinction.

## Scope of work

Review the complete source/target pair against all seven Pass 3 dimensions:

1. Imperative force: requirements, prohibitions, recommendations, defaults, examples and cautions.
2. Conditional scope: applicability, exceptions and limits.
3. Decision logic: conditions, branches, alternatives and owner decisions.
4. Technical tokens: commands, paths, identifiers, kernel parameters, configuration symbols, hashes, quoted error text, numeric values and units.
5. Structure: headings, lists, examples and relationships.
6. Cross-document references: direct links and named dependencies, distinguishing source-explicit relationships from review-record or observer-reconstructed correspondences.
7. Uncertainty: preserve ambiguity and missing source information rather than silently resolving it.

Pay particular attention to preserving distinctions among:
- a running service/probe/emulator result and user-perceptible or hardware-backed function;
- offline evidence, real-device evidence, human confirmation and acceptance decisions;
- cold-boot testing and runtime switching; new-feature isolation on RAM images and later integration;
- test harnesses, criteria, test data, host scripts and observation methods as possible sources of false results;
- negative test inputs, saved evidence, offline re-evaluation, falsification and clean-boot requirements;
- host-side script correctness, protocol/terminal behaviour, command exit status, output capture, device enumeration and USB/Fastboot handoff;
- user participation, advance notice, long-running recording, automated desktop interaction and physical-touch confirmation;
- device concurrency, one-operator constraints and boot-ID-bound evidence;
- diagnostic direction, earliest failing layer, clean state and control experiments;
- collaboration constraints, NO-GO records, sub-agent task boundaries and verification requirements;
- the difference between a rule existing in the experience library and that rule being present in the active operator's task brief;
- recovery prerequisites and critical device-safety constraints;
- public documentation, community contribution review, provenance, and legal/ethical boundaries.

Do not turn these observations into a formal evidence-state machine, test-orchestration architecture, concurrency/locking protocol, sub-agent lifecycle, universal diagnostic algorithm or safety endorsement unless the source explicitly does so. Preserve the source's pitfall sequence and its actual uncertainty.

## Cross-document evidence

The existing Pass 1 review records that:
- the source and English collection indexes list this document's subject scope;
- the pattern index explicitly cites it under Patterns E, F, H and I;
- those relationships arise from the indexes, not inline links in the source body.

Verify the corresponding entries in the supplied source and English indexes/pattern indexes. Do not promote index relationships into source-body links or infer a stronger dependency.

## Permitted changes

- Modify only `translation/en/experience/testing-and-process.md` where a material, source-justified correction is required.
- Append a clearly labelled Pass 3 section to `translation/reviews/experience/testing-and-process.md.md`, preserving the existing Pass 1 record.
- Do not modify the Chinese source, pattern/index files, criteria, current-position/checkpoint records, inventory, glossary or unrelated files.
- Do not rewrite for style alone.
- If no translation correction is justified, leave the target unchanged and record that result.

## Stop conditions

Stop and report rather than broaden the task if:
- a required source/target/governing revision differs materially from the stated baseline;
- a required passage or named index relationship cannot be obtained;
- a translation decision depends on current Android behaviour, device testing, external technical correctness or operational safety validation;
- an index relationship cannot be distinguished from an inline source relationship;
- the task cannot be completed within the stated scope.

Do not resolve external technical questions or silently expand the task.

## Validation

- Re-read the complete changed target and review record.
- Confirm the Chinese source is unchanged.
- Confirm every proposed target change is supported by the Chinese source.
- Confirm source-explicit references and index/review correspondences remain distinguished.
- Preserve Pass 1 review content as an exact prefix.
- Record exact revisions examined, changed files, checks performed and unresolved matters.

## Separate investigation proposals

If the review raises questions that require current upstream evidence, real-device testing or other external investigation, do not resolve them within this task. Where useful, record them separately from Pass 3 findings in the handoff, identifying the observation that prompted the question and the external evidence or test that would be needed. Such proposals are not translation findings, corrections or acceptance evidence. Do not manufacture proposals if none arise.

## Required handoff

Append the reconstructable handoff to this task record, following the eight fields in `translation/EXECUTION-SEGMENTATION.md`: task identity; observer position; observations; decisions; unresolved matters; validation; change boundary; next dependency.

The handoff must state whether the task was completed, stopped for an adequacy reason, or requires further evidence. Do not move this task to `completed/` until worker execution and handoff are complete. Moving it does not constitute orchestrator acceptance.

For the initial trial, only one worker should select and execute a task at a time; no locking or claim mechanism is assumed.


---

## Worker handoff — Task 007

### 1. Task identity

- Pass 3 — technical and semantic review; current position Pass 3 / Checkpoint 22.
- Source: `experience/testing-and-process.md`, `main`, blob SHA `ff1dbea0833dc9e9eb2f668000855f73ec0a9599` (unchanged).
- English target: `translation/en/experience/testing-and-process.md`, `translation/english-reconstruction`, original blob SHA `391f94ab1508b02cd4b1acc8942ccef1e4600372`; updated blob SHA `b78afbf1b3a9f05a6765a51b2241eb56bd5c0eca`.
- Review record: `translation/reviews/experience/testing-and-process.md.md`, Pass 1 baseline SHA `7e1d78eee519a7caef00fb5a837bc7b8c5c83c3c`; Pass 3 appended, resulting blob SHA `be2e323e070c75c77f54baa77e579b234b760b20`.
- Task record: `translation/tasks/007-pass3-experience-testing-and-process.md`, original blob SHA `e8bfb287f07256beb8239def3b88f2930479ef5c`; this handoff appended.
- Commit containing target correction: `a7f3f1647632c6fcca3a9705974b07b4a0142e4a`.
- Commit containing Pass 3 review: `f2c55fd33d68cdcc828912f4006ee2d65e4cbbc1`.

### 2. Observer position

Determine whether the complete English translation preserves the Chinese source's meaning and operational force across imperative force, conditional scope, decision logic, technical tokens, structure, cross-document references and uncertainty. Current upstream correctness, actual Android-device behaviour, operational safety validation and redesign of test/collaboration systems were excluded.

### 3. Observations

The source/target pair preserves the ordered pitfall collection and its distinct cases concerning evidence quality, offline versus real-device testing, cold boot, host-side scripts, test-tool validity, user participation, desktop automation, device concurrency, diagnostic direction, clean-state experiments, sub-agent boundaries, reuse of community work, documentation, candidate-kernel recovery and community contributions.

The translation retains the source's requirements, prohibitions, alternatives, numeric values, technical identifiers, commands and applicability qualifiers. One wording issue was found: “releasing and rebinding the USB port” did not preserve the source's explicit “unbind and rebind” operation as directly as “unbinding and rebinding.” The source and English indexes list the document's subject scope. Both pattern indexes cite it under E/F/H/I; these are index relationships, not inline links in the source body.

### 4. Decisions

Changed only the target phrase from “releasing and rebinding the USB port” to “unbinding and rebinding the USB port,” directly corresponding to `解绑重绑 USB 端口`.

Appended a Pass 3 review section to the existing review record, preserving the Pass 1 record as its prefix. No other translation correction was identified. The review explicitly distinguishes source-body evidence, index-established relationships, and observer-reconstructed correspondences.

### 5. Unresolved matters

- The task specifies segmentation-document SHA `412268a0c065a628d65c0c2c1799830f718e92d1`, while the current branch revision is `05ee569421b89366bbc0993afe62cb22a0644913`. The current revision retains the worker/orchestrator boundary, evidence-admissibility rules, separate investigation-proposal boundary and eight handoff fields. Its embedded application-position paragraph is stale (Checkpoint 16 / `experience/wireless.md`) and conflicts with the task and canonical current-position record (Checkpoint 22 / `experience/testing-and-process.md`). The task and current-position record were used to establish this task's position; the segmentation document was used for its execution-boundary and handoff provisions. No out-of-scope file was changed. This discrepancy is referred to the orchestrator for independent assessment.
- External technical correctness, current upstream behaviour, real-device behaviour and operational safety remain unvalidated and outside scope.
- No separate external investigation proposal was necessary.

### 6. Validation

- All task-specified evidence-baseline revisions were fetched and checked. Every listed baseline matched except the segmentation-document SHA, as recorded above.
- Complete Chinese source and English target were compared.
- The target was re-fetched after update: the correction is present and the previous phrase is absent.
- The source was re-fetched after changes and remains at SHA `ff1dbea0833dc9e9eb2f668000855f73ec0a9599`.
- The review record was re-fetched after update; the Pass 3 section is appended and the original Pass 1 record remains its prefix.
- Source and English collection indexes and pattern indexes were checked; Pattern E/F/H/I references are index-established, not source-body inline links.
- The review and task handoff record the exact revisions and change boundary.
- No external technical or real-device validation was performed.

**Result: COMPLETED — one source-justified translation correction made; Pass 3 review and worker handoff recorded.**

### 7. Change boundary

Changed only:
- `translation/en/experience/testing-and-process.md` — one phrase-level correction.
- `translation/reviews/experience/testing-and-process.md.md` — appended Pass 3 review section.
- This task record — appended this worker handoff.

The Chinese source, indexes, patterns, checkpoints, current-position record, glossary, inventory, translation instructions, segmentation document and all unrelated files were left untouched.

### 8. Next dependency

Ready for independent orchestrator reconstruction and acceptance. The orchestrator should assess the stale embedded position and revision discrepancy in `translation/EXECUTION-SEGMENTATION.md`, then determine whether the Task 007 evidence and changes satisfy the task boundary. No next document task is selected by this worker.

**Worker execution status: COMPLETED. Orchestrator acceptance: pending.**
