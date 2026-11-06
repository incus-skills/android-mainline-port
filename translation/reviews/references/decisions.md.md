# Reconstruction and Translation Record: references/decisions.md

## Source and provenance

- Source path: `references/decisions.md`
- Source ref: `main`
- Source blob SHA: `bc424790646c0de63c36fce2824dd09752d289d7`
- English artifact: `translation/en/references/decisions.md`
- Translation branch: `translation/english-reconstruction`
- Translation commit: `7a41a9f34927683577bc57a8148d652988c5aade`

## Translation intent

This is a functional English translation of the source decision catalogue, not a technical rewrite, endorsement or validation of its recommendations.

The first-pass objective is to learn the skill through its construction and contents: recover how the catalogue is organised, how its entries express conditional decisions, how it relates to the rest of the skill, and how its language communicates operational force. The truth or current validity of individual technical claims does not determine whether this structural and linguistic reconstruction can proceed. Those claims belong to a separate technical-evidence and validation pass after the source structure, operation and language have been translated.

Preserve the source's:
- nine-section organisation and the ordering of decision topics;
- entry pattern of default, conditional rules, rationale, applicability limits and acceptance criteria where present;
- distinctions between recommendations, prohibitions, temporary workarounds, diagnostic measures and user-owned decisions;
- conditions under which a default should be reassessed;
- technical identifiers, commands, paths, configuration keys, subsystem names and stated numerical values;
- omissions and unevenness in the source (for example, not every entry has an explicit rationale or acceptance section).

Do not silently correct technical claims, fill missing criteria, or convert a contextual recommendation into a universal rule.

## Reconstructed document role

The document is a decision catalogue, not a stage-by-stage procedure and not a flat set of unconditional best practices.

Its opening establishes a repeated decision-entry grammar:
1. **Default** — the starting approach.
2. **Rules** — condition/action relationships that qualify or redirect the default.
3. **Why** — the mechanism or reasoning that lets a reader judge whether the rule transfers to a new device.
4. **Does not apply** — stated boundaries or exceptions.
5. **Acceptance** — checks where the source provides them.

The source explicitly says that its rules are abstracted from choices and outcomes across multiple projects. When conditions differ, the reader is directed back to the general criteria in Section 1 and instructed to record the reason for reassessment.

Section 1 supplies nine cross-topic criteria: proximity to upstream; reversibility; real-device evidence; maintenance ownership; replaceability; minimality; user cost/risk; licensing; and an exit condition for temporary approaches. Important decisions are to record options, costs, choice, rationale and reassessment conditions.

The remaining sections group decisions by technical concern:
- preparation and boot;
- installation and dual boot;
- display and input;
- power;
- wireless;
- remote-processor peripherals;
- graphics and applications;
- engineering and release strategy.

The catalogue therefore connects technical choices to the project's broader process and discipline: evidence quality, reversibility, technical-debt registration, user authorisation, real-device acceptance, provenance and later reassessment. This relationship is reconstructed from the content; the source does not provide a formal dependency graph for the catalogue.

## Terminology and translation decisions

- `默认做法` → **Default**. This is a starting approach, not an unconditional requirement; the surrounding rules qualify it.
- `规则` → **Rules**. Retained as the source's condition/action guidance rather than recast as a formal policy layer.
- `为什么` → **Why**. Preserves the explanatory mechanism used to assess transferability to another device.
- `不适用` → **Does not apply**. Retains applicability boundaries without implying that every entry must have one.
- `验收` / `怎么测` → **Acceptance** / **How to test**. Kept distinct where the source distinguishes acceptance outcome from test method.
- `承重参数` → **load-bearing parameter**. Preserves the source's metaphor for a debugging parameter whose removal breaks the display; it is not promoted to a formal technical term.
- `原生接管` → **native takeover**. Consistent with the glossary and earlier translations.
- `真机证据` → **real-device evidence**. Retains the distinction between claims of support and demonstrated operation on hardware.
- `活动槽镜像集轮换` → **active-slot image-set rotation**. A descriptive rendering of the named dual-boot approach; its technical correctness is not assessed here.
- Technical identifiers and tokens are retained as source strings, including kernel parameters, service properties, subsystem names, command names, paths and version labels. Formatting is not used to alter their meaning.

## First-pass structural observations

1. The catalogue is deliberately heterogeneous. Some entries have all five elements; others have only a default and a rule, or a default and rationale. The translation preserves this rather than imposing a uniform template on every subsection.
2. The default is often conditional. For example, the display path changes according to upstream driver and panel-driver availability; the initramfs approach changes between early bring-up and product phases; suspend is deferred until prerequisite power-management work is complete.
3. Rules include several different operational functions: choosing between alternatives, diagnosing a failure, sequencing work, defining a safety boundary, and stating an acceptance test. They should not be collapsed into one undifferentiated category during later analysis.
4. The rationale is explicitly intended to support transfer to a new device. It gives mechanisms, not merely justifications for a preference.
5. The catalogue contains temporary diagnostic or transitional measures that are expressly not product solutions. The associated technical-debt registration and exit conditions are part of their meaning.
6. Some decisions require user choice or authorisation, particularly partition layouts, firmware distribution and risk-bearing speaker gain. These are not decisions the agent is authorised to make independently.
7. The “do not repeat unless new evidence appears” list in Section 9 is qualified by an evidence-based reopening condition. It is not an absolute, timeless prohibition.
8. The document does not define a formal priority order among all nine general criteria, nor a scoring or weighting method. The translation does not invent one.
9. The source's topics connect to other references and experience files through direct references (for example, GPU and Ubuntu integration) and shared concepts, but the document does not specify a complete cross-document link graph.

## Ambiguities and items to preserve for later review

- The exact technical meaning and expected contents of the “value chain” supplied for speaker-gain risk may need domain-context review. The translation preserves the idea as a chain of values and does not expand it.
- Some source terms are compressed or project-specific (for example, the active-slot image-set rotation method). Their technical interpretation should be checked in a separate domain review, not resolved by translation alone.
- The source uses shorthand in several rules, including command and kernel configuration expressions. These have been retained; no operational steps have been added to explain them.
- A few recommendations are explicitly tied to Qualcomm, GKI, Ubuntu, Debian-family systems or particular upstream interfaces. They remain scoped as in the source and should not be generalised during later synthesis.

## Review boundary and status

- Structural fidelity: first-pass translation preserves the nine-section organisation and topic ordering.
- Conditional logic: default/rule/rationale/applicability/acceptance distinctions retained where present.
- Technical tokens: source identifiers and values retained; typography may differ from the Chinese source.
- Source ambiguity: not silently resolved.
- Technical correctness: **not reviewed or validated**.
- Current status: **translated**. Semantic review and technical validation remain outstanding.

The next review should compare the English line by line with the Chinese source, then assess how this decision catalogue is used alongside the process, discipline, verification and experience documents. Technical claims should be assessed in a distinct pass with appropriate source and device evidence.


## Pass 3 — technical and semantic review

Source/translation pair rechecked against `references/decisions.md` at blob `bc424790646c0de63c36fce2824dd09752d289d7`.

### Review results

- The nine-section catalogue structure and topic order are retained.
- The decision-entry grammar remains default, conditional rules, rationale, applicability limits and acceptance where present; missing elements are not filled in.
- The nine general criteria retain their order and decision-record requirements.
- Conditional branches, prohibitions, temporary diagnostic measures, user-owned decisions and acceptance checks were checked across the translated sections.
- Numerical values, command names, kernel parameters, service properties, paths, subsystem names and version labels were retained.
- Qualcomm-, GKI-, Ubuntu-, Debian- and interface-specific scopes remain attached to the corresponding source rules.
- The list of uneconomical approaches remains conditional on new evidence; it is not translated as an unconditional timeless prohibition.
- No universal scoring system, priority weighting, extra acceptance gate or cross-document dependency graph was introduced.

### Corrections

No source-driven semantic correction was required. The existing translation was retained unchanged.

### Boundaries

The source contains compressed and domain-specific expressions, including the speaker-gain "value chain" and active-slot image-set rotation. The existing review record preserves these as interpretation boundaries. They are not resolved through stylistic rewriting.

This review does not validate the technical recommendations, current upstream status, safety or hardware behaviour.

- Source-based semantic review: completed for this document.
- Translation changes: none.
- Technical correctness validation: not performed.
- Whole-skill validation: outstanding.
