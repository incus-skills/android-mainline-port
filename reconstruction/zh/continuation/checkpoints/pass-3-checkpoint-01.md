# Pass 3 — Checkpoint 01: Declaration of Technical and Semantic Review

## 1. Pass declaration

- Pass: **Pass 3 — technical and semantic review of the operational English translation set**
- Purpose: compare English operational documents against their Chinese sources for preservation of meaning and operational force.
- Scope: `SKILL.md`, six documents under `references/`, and fifteen documents under `experience/`.
- Source branch: `main`
- English working branch: `translation/english-reconstruction`
- Prior baselines: Pass 1 operational structural/linguistic reconstruction; Pass 2 repository-facing README review.
- First document: `SKILL.md`
- Technical correctness validation against upstream implementations or real devices: **out of scope**.

## 2. Review dimensions

For each source/translation pair, inspect:

1. Imperative force: requirements, prohibitions, recommendations, defaults, examples and cautions.
2. Conditional scope: when a rule applies, exceptions and limits.
3. Decision logic: conditions, branches, alternatives and owner decisions.
4. Technical tokens: commands, paths, identifiers, filenames, kernel interfaces, configuration keys, error strings, units and version references.
5. Structure: headings, tables, lists, examples and relationships.
6. Cross-document references: whether translated links and named dependencies resolve to the intended English document.
7. Uncertainty: whether ambiguity or missing source information has been preserved rather than silently resolved.

A translation that matches source wording does not thereby establish that the source's technical claim is correct. External truth, current upstream status and real-device behaviour remain separate validation questions.

## 3. Review method

- Read the complete source and complete English counterpart.
- Use the existing translation review record as provenance, not as a substitute for rechecking the pair.
- Record material discrepancies and justified corrections.
- Do not rewrite merely for stylistic preference.
- Do not change original Chinese sources.
- Preserve separate review and reconstruction artifacts.
- Work in dependency order, beginning with `SKILL.md`, then the framework references and experience library.
- Keep unresolved external or domain questions bounded and traceable.

## 4. Exit condition

Pass 3 can close when every in-scope operational source/translation pair has been compared against these dimensions, material discrepancies are corrected or explicitly recorded, and the inventory distinguishes source-based review from external technical validation.

This exit condition does not include:
- proving technical claims against current upstream kernel or distribution behaviour;
- real-device testing;
- validating current third-party agent installation interfaces;
- whole-skill activation or representative workflow execution.

Those belong to separately scoped validation work.

## 5. First document position

The first source is `SKILL.md`, the entry point and navigation contract. Its review must preserve the relationship between activation, entry routes, reading order, stage process, decision principles and linked framework/experience documents. The next checkpoint should record the actual source/translation comparison, not assume the current English structure is correct merely because Pass 1 reconstructed the Chinese source.

## 6. Re-entry

1. Read `reconstruction/zh/continuation/current-position.md`.
2. Read this declaration.
3. Read `translation/README.md` Phase 4.
4. Read `translation/GLOSSARY.md`.
5. Fetch the complete `SKILL.md`, `translation/en/SKILL.md` and existing `translation/reviews/SKILL.md.md` before reviewing.
