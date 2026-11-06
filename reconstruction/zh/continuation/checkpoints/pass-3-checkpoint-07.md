# Pass 3 — Checkpoint 07: `references/verification.md` Semantic Review

## 1. Position

- Pass: **Pass 3 — technical and semantic review of the operational English translation set**
- Document: `references/verification.md`
- Source branch: `main`
- Source blob SHA: `fa1ba5200fdb38fb4578ff9f6862e0f8af1f241d`
- English translation: `translation/en/references/verification.md`
- English translation blob SHA: `1c0ee1a531dee1a7a42653a453231550f0302023`
- Translation review record: `translation/reviews/references/verification.md.md`
- Previous document: `references/build-environment.md`
- Next document: `references/ubuntu.md`
- Technical correctness validation: not performed.

## 2. Review result

The Chinese source and English translation were compared section by section. **No source-driven semantic correction was required.** The English translation remains unchanged.

The review covered:
- seven status labels, their definitions and required records;
- the distinction between source-code leads, offline checks, real-device boot, real-device functional pass, cold-boot pass and publishability;
- the explicit non-evidence list;
- subsystem-specific hardware metrics and real outcomes;
- cold-boot/control, clean-install, positive-control and debugging-variable requirements;
- raw real-device test data and test-environment differences;
- the safety-only immediate-stop exception and continued evidence collection for ordinary functional failures;
- host orchestration, device-side self-checks and automatic recovery;
- the division between automated checks and human participation;
- GNOME desktop automation and the continuation of the automation table after its explanatory paragraph;
- cold-boot acceptance, regression, negative testing, user participation, performance budgets, experiment records and first-release standard.

Commands, paths, interface names, identifiers, timings, quantities and examples were retained.

## 3. Distinctions and boundaries preserved

- A source lead is not a successful build; an offline pass is not evidence that hardware works.
- A device booting is distinct from a function passing on real hardware.
- Hardware metrics, real outcomes and human confirmation are separate evidence dimensions.
- The seven labels are not converted into a formal state machine. The source does not specify mandatory transitions between every adjacent label.
- The vocabularies in this document and `discipline.md` overlap but remain independently defined; no unified status model or mapping is invented.
- Machine-observable signals and injected events do not replace human-observable acceptance where the source requires it.
- The final three automation rows continue the source table after the GNOME paragraph. The English version repeats the table headings for readability; this is a presentation aid, not a semantic change.
- The first-release standard remains narrower than full peripheral completion or upstream submission.

## 4. Pass 3 disposition

- Source-based semantic review: **completed**.
- Translation changes: **none required**.
- Technical correctness, current API availability, test-method suitability and real-device behaviour: **not validated**.
- Whole-skill validation: **outstanding**.
- Next document: `references/ubuntu.md`.

This checkpoint records translation fidelity only. Technical claims require a separate evidence-based review and must not be treated as validated by this comparison.
