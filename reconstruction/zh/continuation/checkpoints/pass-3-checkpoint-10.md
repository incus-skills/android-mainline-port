# Pass 3 — Checkpoint 10: `experience/patterns.md` Semantic Review

## 1. Position

- Pass: **Pass 3 — technical and semantic review of the operational English translation set**
- Document: `experience/patterns.md`
- Source branch: `main`
- Source blob SHA: `c485e34f6fb06808d7c2678b5a2280494e5f71c0`
- English translation: `translation/en/experience/patterns.md`
- Translation review record: `translation/reviews/experience/patterns.md.md`
- Previous document: `experience/README.md`
- Next document: `experience/bootloader-and-boot.md`
- Technical correctness validation: not performed.

## 2. Review result

The source and English translation were compared across the introduction and all nine categories. One terminology correction was made in Category D: `已移动的标签` is rendered as **moved tags**, rather than “moved labels”, preserving the source's tag terminology without adding an unsupported claim about the specific tag system. No other source-driven semantic correction was required.

The comparison confirmed preservation of:
- the instruction to classify a new symptom using recognition criteria, investigate through general handling and then consult concrete domain entries;
- the explicit allowance for one symptom to belong to multiple categories;
- all nine category distinctions and their recognition symptoms, general handling steps, manifestations and domain references;
- the sequence of evidence capture, comparison, reproduction and correction in the bootloader and resource-ownership patterns;
- timing/lifecycle investigation, residual-state isolation and measurement-method controls;
- the distinction between lower-layer signals and end-to-end user-facing acceptance;
- upstream-fix checking, implementation-transfer boundaries and recovery-path testing;
- technical identifiers, commands, paths, interfaces, examples and cross-file references.

## 3. Distinctions and boundaries preserved

- The nine categories are heuristic and may overlap; they are not a mutually exclusive partition.
- Recognition criteria orient investigation; they do not prove a root cause.
- General handling sections contain distinct investigative routes, not one uniform algorithm.
- The source defines no primary-category selection, tie-break rule, category priority or root-cause confirmation threshold.
- Domain-file references are navigational pointers, not evidence that every referenced entry was independently validated in this pass.
- The source's examples and upstream-status claims remain source claims and may be version-sensitive.
- Category I's SysRq instruction is preserved as a source recommendation, not technically endorsed.

## 4. Pass 3 disposition

- Source-based semantic review: **completed**.
- Translation correction: **moved labels** → **moved tags** in Category D's residual-state examples.
- Technical correctness, current upstream status, safety and device-specific applicability: **not validated**.
- Whole-library validation: **outstanding**.
- Next document: `experience/bootloader-and-boot.md`.

The nine patterns remain diagnostic orientation aids, not proven diagnoses or mutually exclusive classes. The Chinese source remains authoritative.
