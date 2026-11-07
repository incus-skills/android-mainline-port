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


---

## Worker handoff — Task 010

### 1. Task identity

- **Execution status:** COMPLETED — the bounded documentation walkthrough and reconstructable handoff are complete. This is a procedural finding only; it does not establish technical correctness or real-world troubleshooting success.
- **Traversal / checkpoint:** Pass 4 / Checkpoint 03; Phase 5 — Whole-skill validation.
- **Task:** `translation/tasks/010-phase5-troubleshooting-workflow.md`; pre-handoff blob SHA `1a3ce44a04a1f11a273a6ab6f55b87596e5027eb`.
- **Pinned English evidence:** `translation/en/SKILL.md` = `f613aa7ef89eb9204c4f9804faf7aebace596a4e`; `translation/en/experience/patterns.md` = `7309618ad08085bb514b6800a580774f829fe312`; `translation/en/experience/usb-and-storage.md` = `7c3225d7a7f8650ce27a345922937cdadcf0cc59`; `translation/en/experience/README.md` = `a7a28190615253246749909b380ddcba17268769`.
- **Pinned English tree:** `translation/en/` = `ba8a19206173ccde8fd26b6ae731ca4a0ee8c7cc`; `references/` subtree = `41f3799c0dd0caa7bdef8e9b4820f45f21ace473`; `experience/` subtree = `d8d0db60cb0ef7dafd527f1d5302683bb3fbbb93`.
- **Position evidence:** Pass 4 / Checkpoint 01 = `598722e61e115068af5c90d90dbdf8224dd7c764`; Checkpoint 02 = `e2eee68d113988b3aed8a01fb03682fa130aa1f0`; active Checkpoint 03 = `e6b0a752a82d7484b75083ad4a5279bc81342549`; accepted Task 008 record = `3b334e3537cb25dae7faffc0d6778b048d6529e6`; accepted Task 009 record = `5662215f0ef445748c3174b1740c0b2efff7018e`.
- **Governing references:** `translation/README.md` = `f9bf34ae28e838ae881f9e207797f26c1b327baa`; `translation/INVENTORY.md` = `0a8e58e9a6f45fd54c718672dc8d7a3a9056f7ee`; `translation/EXECUTION-SEGMENTATION.md` = `05ee569421b89366bbc0993afe62cb22a0644913`.

### 2. Observer position

Trace the single documented symptom — a filesystem behaving abnormally after a sparse image is flashed over a partition containing old data — from the troubleshooting route in the English entrypoint, through Category D and its general handling, to the named DONT_CARE sparse-image entry. Determine whether the route allows the symptom to be classified and the specific documentation to be located, while keeping the documented cause distinct from a confirmed diagnosis. This is a documentation-only walkthrough. It excludes external technical validation, device-specific inference and all device operations.

### 3. Observations

- The entrypoint's troubleshooting instruction (pinned `SKILL.md`, line 20) directs the reader first to `experience/patterns.md` for classification and general troubleshooting, then to search `experience/` for specific cases using distinctive log error codes, function names and configuration items.
- The pattern index introduction (pinned `patterns.md`, lines 1–3) explicitly prescribes the sequence: compare the symptom with **How to recognise it**, investigate using **General handling**, then consult the corresponding domain file for specific entries. It also notes that one symptom may belong to more than one category.
- The experience-library index (pinned `experience/README.md`, lines 27–31) independently confirms that new faults should be classified in `patterns.md` before locating a specific entry in the corresponding file. It says distinctive terms from logs can be used to grep the library, and qualifies that symptom text is written to be searchable where possible.
- The hypothetical symptom is an exact match for the recognition signal in Category D: old data appears in the new system (`patterns.md`, line 62). Category D is titled **Residual state distorts the result** (line 60).
- Category D's general handling (`patterns.md`, lines 64–68) prescribes listing possible residual-state sources, explicitly including old partition data; reproducing on a clean system with a fresh boot and empty directories; and accepting a fix only under conditions where no residual state is present.
- Category D's common manifestations (line 70) explicitly include sparse skipping blocks that retain old data and logs, and cross-reference `usb-and-storage.md`. This makes the general-to-specific destination explicit without requiring an unrelated category or document.
- The named entry in `usb-and-storage.md` (lines 35–42), **DONT_CARE in a Sparse Image Does Not Clear Old Data and May Replay Old Journal Records**, repeats the hypothetical symptom (line 39). It documents:
  - **Cause:** `img2simg -s` marks empty blocks to be skipped, leaving old contents on the device. Where the filesystem UUID is fixed, journal blocks from a previous installation of the same version may still pass checks, allowing crash recovery to replay an old journal into the new filesystem. It also states that zero-filled blocks inside files, inode tables and the journal area must be written (line 40).
  - **Resolution:** skip only free blocks identified by the bitmap while still writing inode tables and the journal area, or alternatively format on the device (line 41).
  - **Applicability:** General (line 42).
- The named entry does **not** contain a case-specific **Confirmation** field or prescribe a test that establishes that the documented cause is the cause in a particular instance. This is distinct from the general Category D handling: the latter prescribes clean-system reproduction and the absence of residual state as the condition for accepting a fix, but it does not specify a case-specific test for fixed UUIDs, stale journal records or successful resolution of this entry.
- The experience-library entry-format example (`experience/README.md`, lines 33–42 and following) includes a `How to confirm` field. The named sparse-image entry omits that field. This is a visible difference between the documented format and this entry; no correction is made under this task.
- The hypothetical supplies the symptom and the prior presence of data, but no device identity, partition layout, filesystem identity, image details or observed evidence that would establish the entry's proposed cause. The exact symptom match supports classification and navigation only; it does not confirm the cause.
- The route does not require an unlisted dependency. The relevant index and the specific entry are both present in the pinned English tree.

### 4. Decisions

- **Procedural finding:** the documented route is followable for this bounded case. The entrypoint identifies the first destination; the pattern index defines the category-first sequence; Category D recognises old data in a new system, gives general handling and points to `usb-and-storage.md`; the named entry is present and its symptom matches the scenario.
- The symptom is a **candidate for Category D** and for the named sparse-image entry. The documented cause must remain a hypothesis about the scenario, not a confirmed root cause.
- The general handling provides a residual-state investigation frame and a clean-system condition for accepting a fix. The specific entry provides a cause and two resolution alternatives, but does not supply its own confirmation procedure or a case-specific post-resolution acceptance test.
- The missing `How to confirm` field is recorded as an entry-format/content observation, not silently repaired and not used to invalidate the navigation route. Whether this omission is intentional or should be addressed is left to the orchestrator.
- No commands, simulated operations, device-specific diagnosis, or technical recommendations for a real device are made. No documentation change is justified or permitted by this task.

### 5. Unresolved matters

- Whether the documented cause applies to any real instance remains unknown. The scenario provides no evidence about the image, filesystem UUID, journal contents or actual partition state.
- The specific entry does not state how to confirm its cause or how to test that either resolution has succeeded. The general Category D procedure does not resolve those case-specific omissions.
- The difference between the entry-format example's `How to confirm` field and the named entry's lack of such a field remains for independent consideration; this worker does not infer authorial intent or propose wording.
- No external investigation proposal arose within this bounded documentation walkthrough. Technical validity, safety and real-device applicability were expressly excluded and were not investigated.

### 6. Validation

- Exact blob SHA matches confirmed for all four pinned English files: entrypoint `f613aa7ef89eb9204c4f9804faf7aebace596a4e`; pattern index `7309618ad08085bb514b6800a580774f829fe312`; specific experience document `7c3225d7a7f8650ce27a345922937cdadcf0cc59`; experience-library index `a7a28190615253246749909b380ddcba17268769`.
- The pinned English tree was retrieved by its exact tree SHA `ba8a19206173ccde8fd26b6ae731ca4a0ee8c7cc`; GitHub returned the same SHA with `truncated: false`. It contains 22 file blobs. The entrypoint, both experience documents and their expected blob SHAs are present in that tree.
- The three Pass 4 checkpoint blob SHAs, the README and inventory SHAs, the execution-segmentation SHA, and both accepted task-record SHAs were fetched and matched the revisions recorded above. Checkpoint 03 confirms Task 009 accepted and identifies Task 010 as the next bounded troubleshooting trace.
- Route evidence locations: `SKILL.md` line 20; `patterns.md` lines 1–3, 60–70; `experience/README.md` lines 27–42; `usb-and-storage.md` lines 35–42.
- The hypothetical symptom was traced without adding device facts. The documented symptom match, documented explanation, general handling, case-specific resolution and absent case-specific confirmation procedure were kept distinct.
- No external sources were consulted; no device commands were issued, simulated or recommended. The validation establishes only documentation route coherence for this one case, not semantic correctness, technical validity, operational safety or actual troubleshooting success.

### 7. Change boundary

- Appended this handoff to `translation/tasks/010-phase5-troubleshooting-workflow.md` only.
- No files under `translation/en/`, Chinese source files, inventory, README, glossary, reviews, metadata, checkpoints, current-position records, execution-segmentation document or other task records were modified.
- No device operations or external technical investigations were performed.
- The task remains in `translation/tasks/`; it has not been moved to `completed/`. Worker completion does not constitute orchestrator acceptance.

### 8. Next dependency

No additional repository evidence is required to complete this bounded walkthrough. The orchestrator should independently reconstruct and evaluate the handoff, particularly the distinction between a followable category-to-entry route and the specific entry's missing confirmation procedure. It should decide whether the entry-format difference warrants a separate bounded documentation operation. No wider technical validation or real-device work is implied by this handoff.
