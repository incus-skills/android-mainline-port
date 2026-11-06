# Pass 3 — Checkpoint 11: `experience/bootloader-and-boot.md` Semantic Review

## 1. Position

- Pass: **Pass 3 — technical and semantic review of the operational English translation set**
- Document: `experience/bootloader-and-boot.md`
- Source branch: `main`
- Source blob SHA: `3b1283ff22c393ca53f02c1b10edc5547389c2b6`
- English translation: `translation/en/experience/bootloader-and-boot.md`
- Translation review record: `translation/reviews/experience/bootloader-and-boot.md.md`
- Previous document: `experience/patterns.md`
- Next document: `experience/kernel-and-resources.md`
- Technical correctness validation: not performed.

## 2. Review result

The source and English translation were compared across the recommended route, completion criteria, first-boot checklist, all 21 pitfall entries and the two unresolved questions. **No source-driven translation correction was required.** The English artifact remains unchanged.

During this pass, the first-pass review record's pitfall count was corrected from 19 to **21** after counting the source's `###` entries. This corrects the reconstruction record, not the translated content.

The comparison confirmed preservation of:
- the seven-step recommended route from read-only evidence acquisition through persistent boot;
- the per-device image-format test requirement, including the same-SoC warning;
- RAM and persistent completion criteria and the first-boot checklist;
- all 21 pitfall entries, their source-specific marker presence, symptoms, causes, confirmation steps, fixes, prevention notes and applicability statements;
- the distinction between reversible RAM testing and persistent installation;
- bootloader transformations, device-tree identity/symbol handling, boot-image formats, serial-number sources, Fastboot responsiveness, debug-channel ordering, initramfs handoff and recovery behaviour;
- the two unresolved questions without supplying speculative solutions;
- commands, kernel symbols, device-tree properties, paths, service names, image formats, configuration identifiers, numeric values and version labels.

## 3. Distinctions and boundaries preserved

- The recommended route is a device-tested progression, not a universal image-format recipe.
- The stock device is used as both evidence source and positive control.
- Bootloader return values are not conflated with actual handoff, kernel execution or user-accessible debugging.
- Packaging-time interventions remain distinct from changes to upstream-style kernel DTS source.
- Reversible RAM testing remains distinct from persistent installation.
- Recovery and debugging prerequisites remain part of the route; they are not treated as optional afterthoughts.
- Pitfall entries retain source-level differences in field coverage; no missing confirmation or prevention fields are invented.
- The two unresolved questions remain unresolved: unattended recovery from early hard hangs, and the exact mechanism behind long-idle Fastboot unresponsiveness.

## 4. Pass 3 disposition

- Source-based semantic review: **completed**.
- Translation changes: **none required**.
- Reconstruction-record correction: pitfall-entry count **19 → 21**.
- Technical correctness, device-specific safety, current upstream status and real-device behaviour: **not validated**.
- Whole-library validation: **outstanding**.
- Next document: `experience/kernel-and-resources.md`.

The Chinese source remains authoritative. This review establishes translation fidelity only and does not validate the technical recommendations.
