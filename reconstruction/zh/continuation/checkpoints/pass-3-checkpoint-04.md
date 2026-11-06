# Pass 3 — Checkpoint 04: `references/process.md` Semantic Review

## 1. Position

- Pass: **Pass 3 — technical and semantic review of the operational English translation set**
- Document: `references/process.md`
- Source branch: `main`
- Source blob SHA: `799e94e94a591e6d16caf68a5a30686c6b35171a`
- English translation: `translation/en/references/process.md`
- Translation review record: `translation/reviews/references/process.md.md`
- Previous documents: `SKILL.md` (Checkpoint 02), `references/discipline.md` (Checkpoint 03)
- Next document: `references/decisions.md`
- Technical correctness validation: not performed.

## 2. Review results

The translation preserves:
- all nine stages, their order, goals, activities, completion criteria and experience references;
- the explicit parallelism rules;
- Stage 5 beginning once the RAM desktop works, without waiting for all peripherals;
- the per-peripheral isolation, real-device testing, integration, ABI packaging, cold-boot regression and technical-debt cycle;
- clean installation as a test of the public documentation;
- the first-hour user audit and support-table evidence boundary;
- user confirmation before pushing or publishing;
- the Stage 6/7 version rhythm and Stage 8 maintenance activities.

Commands, paths, identifiers and technical tokens were checked in the stage instructions. The source's missing Stage 8 completion criterion remains missing in the translation. No exact concurrency boundary or formal state machine was added.

No source-driven semantic correction was required. The translation was retained unchanged.

## 3. Important distinctions preserved

- Staged process versus strictly serial process.
- Offline preparation versus real-device test concurrency.
- RAM desktop milestone versus completion of all peripherals.
- Per-peripheral isolated test versus integration and regression.
- Documentation-led clean installation as an acceptance test of the documentation itself.
- Real-device evidence versus claims of support.
- Versioned development/release loop versus unbounded accumulation of candidates.
- Maintenance activities versus a defined terminal completion state.

## 4. Boundary and next position

- Source-based semantic review: completed for this document.
- Translation changes: none.
- Technical correctness validation: not performed.
- Whole-skill validation: outstanding.
- Next: `references/decisions.md`.

Pass 3 remains open; the other operational source/translation pairs remain in scope.
