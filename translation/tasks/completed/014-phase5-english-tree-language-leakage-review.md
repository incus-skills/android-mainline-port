# Task 014 — Phase 5 English-tree source-language leakage review

## Purpose and observer position

Perform one bounded, whole-tree review for accidental source-language leakage in the English operational skill.

Pass 4 / Checkpoint 01 defines source-language leakage as part of Phase 5 whole-skill validation. Pass 3's source-based translation review is closed; do not reopen it. Task 013 completed the cross-document stage/record-contract review and was accepted at Pass 4 / Checkpoint 07.

The question is whether the pinned English tree contains accidental untranslated source-language prose, mixed-language residue, or other language leakage that could impede an English-reading agent. This is a detection and classification task, not a translation or correction task.

## Position and governing criteria

- Traversal: **Pass 4 — whole-skill English validation**.
- Project phase: **Phase 5 — Whole-skill validation**.
- Current position: Pass 4 / Checkpoint 07.
- Accepted operations: Tasks 008–013.
- Checkpoint 01: `reconstruction/zh/continuation/checkpoints/pass-4-checkpoint-01.md`, blob SHA `598722e61e115068af5c90d90dbdf8224dd7c764`.
- Checkpoint 06: `reconstruction/zh/continuation/checkpoints/pass-4-checkpoint-06.md`, blob SHA `c91664de1db8e06af3341292748d5123b92362af`.
- Checkpoint 07: `reconstruction/zh/continuation/checkpoints/pass-4-checkpoint-07.md`, created for this position.
- Phase 5 criteria: `translation/README.md`, blob SHA `f9bf34ae28e838ae881f9e207797f26c1b327baa`.
- Context-segmentation boundary: `translation/EXECUTION-SEGMENTATION.md`, blob SHA `05ee569421b89366bbc0993afe62cb22a0644913`.
- Pinned English tree: `translation/en/`, tree SHA `ba8a19206173ccde8fd26b6ae731ca4a0ee8c7cc`, expected 22 file blobs.

## Exact evidence boundary

Use only:

1. The complete pinned English tree `translation/en/` at tree SHA `ba8a19206173ccde8fd26b6ae731ca4a0ee8c7cc`. Retrieve the recursive tree, confirm exact SHA and `truncated: false`, enumerate all paths/blob SHAs, and inspect all files.
2. `translation/README.md` and Pass 4 / Checkpoint 01 and Checkpoint 07 only for Phase 5 purpose, scope and current position.
3. `translation/EXECUTION-SEGMENTATION.md` only for worker/orchestrator boundaries and the required eight-field handoff.

Do not use Chinese source files, upstream material, external documentation, web search, actual project workspaces or prior unpinned versions. Do not infer a translation error by comparing against an unavailable source. If determining whether a candidate is intentional requires source material outside this boundary, classify it as unresolved rather than guessing.

## Scope of work

1. Enumerate all files in the pinned English tree and confirm that the recursive tree is complete.
2. Scan the full content of all 22 files for candidate source-language leakage, including:
   - non-English prose or fragments;
   - mixed-language sentences, headings, labels, comments or table cells;
   - untranslated explanatory text in otherwise English documents;
   - accidental source-language residue in navigation, examples or user-facing instructions.
3. Inspect the context of every candidate. Distinguish:
   - actual prose or instructions apparently left untranslated;
   - proper names, project names, people/organization names and established product names;
   - technical identifiers, commands, paths, filenames, code, configuration keys, protocol names and log strings;
   - quotations, citations, licence notices or source-attribution material;
   - intentionally retained terms whose function is evident from the English context;
   - ambiguous candidates whose intent cannot be established from the pinned English tree alone.
4. Search for language-mixing that does not rely only on non-Latin character detection. Inspect headings, comments, examples and labels where a short untranslated phrase could be missed by a character-only scan.
5. For each candidate, record exact path, section/line or location, surrounding context, classification, evidence and confidence. Do not reproduce large passages.
6. Determine whether any candidate materially blocks an English-reading agent, is merely a harmless retained token, or requires source-author clarification.
7. Report the overall result and the limitations of an English-only inspection. Do not convert an ambiguous candidate into a confirmed translation error.

## Exclusions

- Do not reopen Pass 3 or perform source-to-target semantic comparison.
- Do not read or use Chinese source files, even to resolve a suspected candidate.
- Do not use upstream or external sources to decide whether wording is translated correctly.
- Do not assess technical correctness, current upstream validity, workflow usability, installation compatibility or metadata.
- Do not rewrite, translate, delete or otherwise modify any of the 22 English skill files.
- Do not modify Chinese source, README, inventory, glossary, metadata, checkpoints, current-position records or previous task records.
- Do not create a correction proposal or prescribe new terminology.
- Do not claim that a clean scan proves translation fidelity or source completeness.

## Permitted changes

- Read-only with respect to skill content and project source material.
- Append the required eight-field handoff to this task record only.
- Once execution and handoff are complete, move this task record unchanged except for the appended handoff to `translation/tasks/completed/`. This records worker execution only and does not imply orchestrator acceptance.

## Stop conditions

Stop and report if:

- the pinned English tree cannot be retrieved exactly or its recursive listing is truncated;
- any relevant file differs from its pinned blob SHA;
- a candidate cannot be classified without Chinese source or external material;
- a relevant document cannot be retrieved in full;
- the work would require translation, correction, terminology policy or scope expansion.

## Validation

- Confirm exact tree SHA, recursive-tree completeness, all 22 paths and blob SHAs.
- Confirm that every file was inspected in full, not only searched by excerpts.
- Record the scan method and its limits; a Unicode-character scan alone is insufficient.
- Inspect all candidates in context and distinguish actual prose from identifiers and intentional retained tokens.
- Record exact locations and classification for each candidate, including unresolved cases.
- Preserve the distinction between source-language leakage, translation fidelity, technical correctness and agent usability.
- Record the read-only change boundary and any unresolved dependency.

## Required handoff

Append a reconstructable handoff using the eight fields in `translation/EXECUTION-SEGMENTATION.md`:

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

## Worker handoff — Task 014

### 1. Task identity

- Pass 4 — whole-skill English validation; Phase 5 — whole-skill validation.
- Task 014 — English-tree source-language leakage review.
- Branch: `translation/english-reconstruction`.
- Task record before handoff: blob `1181aebbaf0d66270617a1870f612dbf38a71a81`.
- Pinned English tree: `translation/en/`, tree SHA `ba8a19206173ccde8fd26b6ae731ca4a0ee8c7cc`.
- The complete recursive tree contained 22 file blobs and returned `truncated: false`. All 22 files were fetched in full; every fetched blob SHA matched its tree entry.

### 2. Observer position

Determine whether the pinned English operational tree contains accidental untranslated source-language prose, mixed-language residue or other language leakage that could impede an English-reading agent. This was a detection and classification pass only. Pass 3, source-to-target comparison, technical correctness, upstream validation and correction were excluded.

### 3. Observations

- No accidental non-English prose, untranslated source-language fragment or mixed-language instruction was identified in the 22-file tree.
- A CJK-range scan found only the full-width bracket glyphs in the experience-library markers, for example `【Frequent】`, `【Time-consuming】` and `【Critical】`. Context was inspected in `experience/README.md` (lines 30, 37, 48–50), `experience/bootloader-and-boot.md` (including line 25 and subsequent marker-bearing entries), and `experience/kernel-and-resources.md` (including line 22 and subsequent marker-bearing entries). The bracketed contents are English labels; the glyphs function as visual delimiters, not untranslated text.
- A broader Unicode-script scan found no characters belonging to non-Latin scripts in any of the 22 files.
- Headings, section labels, Markdown comments and visible code-comment lines were inspected across the tree. No mixed-language heading, label or comment was identified.
- The heading `The App Store Displays Chinese as Squares` in `experience/ubuntu-userspace.md` (line 188) is English prose describing a reported display symptom; it is not retained Chinese text.
- Commands, paths, filenames, identifiers, configuration keys, protocol names, quoted strings and code were treated as technical or documentary tokens where applicable, not as prose leakage.

### 4. Decisions

- **No confirmed leakage candidate.** No correction, translation or terminology decision is proposed.
- The full-width marker delimiters are classified as intentional formatting around English labels, with high confidence.
- The App Store heading is classified as ordinary English descriptive prose, with high confidence.
- Overall result: no source-language leakage was found that appears to impede an English-reading agent.

### 5. Unresolved matters

- No candidate requires source-author clarification on the evidence available.
- This English-only review cannot establish translation fidelity, identify omissions relative to the Chinese source, or determine whether a Latin-script term was intentionally retained or transliterated. Those questions are outside this task's evidence boundary and were not inferred.
- No external investigation proposal arose; none has been manufactured.

### 6. Validation

- Retrieved the recursive Git tree by the pinned tree SHA `ba8a19206173ccde8fd26b6ae731ca4a0ee8c7cc`; returned SHA matched and `truncated` was `false`.
- Confirmed 22 blob entries and fetched the complete contents of every listed file by repository path at the specified branch. Every returned blob SHA matched the corresponding pinned tree entry.
- Inspected 3,491 lines across the 22 files.
- Ran a CJK/Japanese/Korean character-range scan and a broader Unicode-script scan over the complete contents. The only CJK-range hits were the documented full-width marker delimiters; the broader non-Latin-script scan returned no hits.
- Inspected headings, section labels, Markdown comments and code-comment lines across all files, and reviewed the marker hits in context.
- The character scans are detection aids, not proof of translation fidelity or completeness. In particular, they cannot reliably classify possible Latin-script transliterations without source comparison, which was prohibited.

### 7. Change boundary

- No English skill file, Chinese source, README, inventory, glossary, metadata, checkpoint, current-position record or previous task record was modified.
- Only this Task 014 record was extended with the handoff.
- The task record was then moved, with its appended handoff, from `translation/tasks/` to `translation/tasks/completed/`. No task-management mechanism or project-level continuation position was changed.

### 8. Next dependency

- No additional evidence is required to complete this bounded leakage-detection operation.
- The task is **completed by the worker and awaiting independent orchestrator reconstruction and acceptance**. Completion does not establish orchestrator acceptance, Pass 4 closure, whole-skill completeness or context-segmentation effectiveness.
