# English translation and reconstruction

## Purpose

This branch develops an English edition of `android-mainline-port` while building a verifiable understanding of the skill's operating model.

This is not a blind, sentence-by-sentence translation. Each English document must preserve the source's operational meaning, constraints, decision logic, evidence requirements, and relationships to other documents. Where meaning is uncertain, record the uncertainty instead of silently resolving it.

The Chinese source remains authoritative during this work. Existing `README.en.md` is an orientation and terminology aid, not a substitute for reading the source documents.

## Working principles

1. **Source before interpretation.** Read the complete Chinese source document and its referenced dependencies before translating it.
2. **Understand before rewriting.** Identify the document's role, intended reader, inputs, outputs, prerequisites, completion criteria, and links to other parts of the skill.
3. **Preserve operational force.** Distinguish requirements, recommendations, defaults, conditional rules, examples, and cautions. Do not weaken or strengthen them through translation.
4. **Keep technical tokens exact.** Preserve commands, paths, identifiers, filenames, kernel interfaces, configuration keys, error strings, and code unless a verified correction is explicitly recorded.
5. **Trace every translation.** Each translated document records its source path and is reviewed against that source.
6. **Separate translation from correction.** Do not silently repair technical errors, stale advice, or contradictions. Record them as review questions for a separate decision.
7. **Preserve provenance and licence notices.** Retain the upstream copyright and MIT licence. Clearly identify this work as an English translation derived from the upstream project.
8. **Validate the skill as a system.** Check links, file references, workflow ordering, terminology consistency, and agent usability across the whole document set.

## Proposed phases

### Phase 0 — Establish the baseline

- Record the upstream repository, source branch, and baseline commit.
- Inventory all Chinese-language source files and existing English material.
- Identify files that are executable skill entry points, framework references, experience records, examples, or project-specific material.
- Note the source document's internal links and dependency relationships.

**Exit condition:** the scope and file mapping are explicit; no source file is assumed translated merely because an English README describes it.

### Phase 1 — Reconstruct the skill's structure

For each document, produce a concise analysis before translation:

- **Role:** why this document exists.
- **Scope:** what it governs and what it does not.
- **Inputs and prerequisites:** what an agent must know or inspect first.
- **Outputs:** what the agent is expected to produce or change.
- **Decision logic:** defaults, conditions, exceptions, and owner decisions.
- **Evidence and completion:** how progress or success is established.
- **Dependencies:** related documents and required reading order.
- **Risks / ambiguities:** unclear, conflicting, or time-sensitive statements.

**Exit condition:** the framework, experience library, and their relationship can be explained without relying on translated prose alone.

### Phase 2 — Build a controlled glossary

- Record recurring Chinese terms and their chosen English equivalents.
- Preserve distinctions between terms that may look synonymous but have different operational roles.
- Record alternatives and rationale where a term is difficult to translate.
- Keep identifiers and established technical vocabulary unchanged.

**Exit condition:** recurring concepts have stable translations, and unresolved terms are visible.

### Phase 3 — Translate in dependency order

Suggested order:

1. `SKILL.md` — agent entry point and navigation contract.
2. `references/discipline.md` — documentation, evidence, debt, and handoff discipline.
3. `references/process.md` — end-to-end process and completion criteria.
4. `references/decisions.md` — decision defaults, conditions, mechanisms, and limits.
5. Remaining framework references: build environment, verification, Ubuntu integration.
6. `experience/README.md` and `experience/patterns.md`.
7. Domain-specific experience documents.
8. Supporting examples and any remaining user-facing material.

The actual order may change if the source dependency map shows a stronger prerequisite.

**Exit condition:** each in-scope source has a corresponding reviewed English document or an explicit exclusion.

### Phase 4 — Technical and semantic review

For each translated document:

- Compare every section against the Chinese source.
- Check that imperative strength and conditional scope are preserved.
- Verify commands, paths, code, identifiers, units, version references, and filenames.
- Check internal links and references against the English tree.
- Verify that tables, lists, and examples retain their original relationships.
- Record unresolved questions rather than guessing.

A translation is not technically validated merely because its English reads naturally.

### Phase 5 — Whole-skill validation

- Check that the English `SKILL.md` points only to existing English documents.
- Check that an agent can follow the documented reading order.
- Test representative workflows: new-device assessment, project continuation, technical decision, and troubleshooting.
- Check for contradictions, duplicated rules, missing dependencies, and accidental source-language leakage.
- Review installation and skill-discovery metadata for compatibility.

**Exit condition:** the English tree is coherent and usable as a skill, not merely a collection of translated files.

## File mapping and status

Maintain a source-to-English mapping here as the inventory is established. Use these statuses:

- `uninspected` — source not yet read in full.
- `analysed` — role and dependencies documented.
- `translated` — English draft exists.
- `reviewed` — checked against the source.
- `validated` — links and system-level behaviour checked.

Do not mark a file complete based on partial excerpts or the README summary.

## Change discipline

- Keep translation commits small and logically grouped.
- Prefer one source document (or a tightly coupled pair) per translation commit.
- Do not mix unrelated technical corrections with translation.
- When a source file changes upstream, re-check the corresponding English file.
- Keep the original Chinese files unchanged on this branch unless a separate, explicit reason requires otherwise.
- Before proposing a merge, summarize coverage, known deviations, unresolved questions, and validation results.

## Initial scope

The upstream `README.en.md` already provides a useful English overview. It should be reviewed for consistency with the source skill, but it does not establish that the skill's operational documents are translated.

The initial inventory is to be derived from the repository tree, then reconciled against the indexes in `SKILL.md`, `README.en.md`, and the relevant directory READMEs.
