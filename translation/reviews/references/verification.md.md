# Reconstruction and Translation Record: references/verification.md

## Source and provenance

- Source path: `references/verification.md`
- Source ref: `main`
- Source blob SHA: `fa1ba5200fdb38fb4578ff9f6862e0f8af1f241d`
- English artifact: `translation/en/references/verification.md`
- Translation branch: `translation/english-reconstruction`
- Translation commit: `b8aa4299e3d821dc2a905cd095edcb163c069f2c`

## Translation intent

This is a functional English translation of the source evidence and verification guide. It is not a technical audit, endorsement of the test methods, or validation that the stated hardware metrics are sufficient for every device.

The first-pass objective is to reconstruct how the skill defines evidence, distinguishes levels of success, organises testing, and connects automated observations with human confirmation. The validity of individual technical claims is reserved for a separate technical-evidence pass.

Preserve:
- the seven status labels and their distinct meanings and required records;
- the explicit exclusions from functional proof;
- the subsystem-specific distinction between hardware metrics and real-world outcomes;
- cold-boot, clean-install, control, positive-control and regression requirements;
- the division between automated checks and tasks requiring human participation;
- the instruction to continue evidence collection after ordinary functional failures but stop immediately for safety failures;
- the experiment-record structure, performance-budget examples and first-release standard;
- technical tokens, commands, interface names, timings and stated examples.

Do not turn evidence categories into a formal state machine, infer transitions that the source does not specify, or treat machine-observable signals as substitutes for human-observable outcomes.

## Reconstructed document role

The document defines how the project should justify claims that a feature or build has passed. Its concern is the evidential basis of a claim, not the sequence of project stages.

Its structure is:

1. **Status terms** — a seven-level vocabulary, each with a meaning and required record.
2. **Non-evidence** — signals that may indicate activity or a software-path milestone but do not establish functional success.
3. **Subsystem evidence** — pairs hardware metrics with real outcomes where human observation is needed.
4. **Test methods** — controls for confounding variables, environment differences, debugging effects and safety.
5. **Test automation** — host orchestration, device self-checks, automatic recovery and machine substitution for human tasks.
6. **Boot/performance budgets** — repeated measurements against device-specific limits.
7. **Experiment records** — a four-part pre-experiment statement and a constraint on claims of ineffectiveness.
8. **First-release standard** — another person can build, install and use the result from the documentation; full peripheral completion and upstream submission are not prerequisites.

The document is cross-cutting: its evidence rules apply throughout the process and inform the decision catalogue's acceptance criteria. It also complements `discipline.md`, which records a different set of project/feature progress labels. These relationships are visible in the source content; the source does not define a unified status model or a formal transition graph.

## Terminology and translation decisions

- `怎么判断“通过”` → **How to Judge Whether Something “Passes”**. The wording keeps the focus on the evidential basis for a pass claim.
- `待调研` → **Awaiting investigation**.
- `有源码线索` → **Source lead available**. This denotes the existence of relevant upstream/community code, not a successful build or verified implementation.
- `离线通过` → **Offline pass**. The source explicitly limits this to compilation, QEMU, unit tests and script self-checks; it does not establish hardware operation.
- `真机启动` → **Boots on real device**. This is an image-start milestone, not functional acceptance.
- `真机通过` → **Passes on real device**. This requires a real result, hardware metrics, scenario and evidence location.
- `冷启动通过` → **Cold-boot pass**. The clean-install and no-prior-state condition is retained.
- `可发布` → **Publishable**. The exact release artifact must be the one whose hash is recorded.
- `真实结果` → **Real result**. Kept distinct from hardware metrics and software status signals.
- `人眼确认` → **visual confirmation**. The source's example is human confirmation, not an automated screenshot verdict.
- `正对照` → **positive control**. Retained as a test-control concept.
- `调试通道` → **debugging channel**, consistent with earlier translation choices.
- `首版标准` → **First-Release Standard**. The source sets a usability/reproducibility threshold, not a requirement that all peripherals or upstream submissions be complete.

Commands, paths, interface names, kernel parameters and numeric examples are retained as technical tokens.

## First-pass structural observations

1. The seven labels form an evidential progression from uninvestigated lead to a hash-identified publishable artifact. However, the source does not define formal transition rules, mandatory sequencing between every adjacent label, or a state-machine model.
2. The source is explicit that “boots on real device” and “passes on real device” are different claims. Booting is not functional proof.
3. The source distinguishes hardware metrics from real results. Some subsystems have both; others use a human-observable outcome as the decisive confirmation.
4. The list of non-evidence is important: a component or service appearing active, an interface existing, a command returning success, or simulated inputs agreeing with code does not establish that the user-facing function works.
5. Human participation is narrowed to tasks that cannot be replaced by machine observation, while user observations are still recorded separately from machine metrics.
6. Test tooling itself is subject to verification. Negative inputs, empty output, timeouts and missing commands must be treated as failures; test data must not share the tested code's assumptions.
7. The source favours repeated cold boots and regression comparisons over conclusions drawn from a single successful run.
8. The four-sentence experiment record is a compact way to state the intervention, discriminating outcomes and pass metric before running the experiment.
9. The instruction about “ineffective” constrains interpretation: without knowing that other conditions were good, the correct conclusion is only that no improvement was observed under current unknown conditions.
10. The first-release threshold is deliberately narrower than full hardware completeness or upstream acceptance: reproducible build, installation and use by another person following the documentation.

## Relationship to other source documents

- `discipline.md` defines six project/feature progress labels: 待调研、有源码线索、离线通过、真机启动、真机通过、真机失败.
- This document defines seven evidence/status labels: 待调研、有源码线索、离线通过、真机启动、真机通过、冷启动通过、可发布.
- `discipline.md` also contains a separate three-label evidence-scope vocabulary: 离线通过、真机通过、冷启动通过.

The labels overlap, but the sets are not identical and the source does not define a mapping between the three vocabularies. This translation preserves each list in its own document. It does not unify them, infer equivalence beyond identical labels, or invent a mapping for labels present in only one list.

The decision catalogue uses acceptance criteria and real-device evidence throughout; this guide supplies the evidence distinctions those decisions rely on. The process guide defines project stages, while this document provides cross-stage verification discipline.

## Ambiguities and items to preserve for later review

- The seven status labels look like a progression, but the source does not establish whether every feature must pass through every label or whether the labels are independent classifications in all cases. Preserve the list without adding transition semantics.
- Some subsystem metrics are platform-specific or examples (for example, USB disconnect timing and particular display metrics). They should not be universalised beyond the source's intended device context.
- The “human observation” column is not equally populated for every subsystem. A dash is retained where the source supplies no separate human-observable outcome.
- The source's GNOME desktop-automation description is detailed and interface-specific. Translation preserves the stated method; current API availability and behaviour are outside this pass.
- The GNOME automation table continues after the explanatory paragraph in the source without repeating its header. The English rendering repeats the column headings for readability; this is a presentation aid, not a change to the rows' meaning.
- The first-release standard does not require all peripherals to be complete or all work to be upstreamed. Do not silently strengthen this criterion during synthesis.

## Review boundary and status

- Structural fidelity: seven status rows, subsystem evidence table, test methods, automation sections, budgets, experiment record and first-release standard retained.
- Evidence distinctions: machine metrics, real outcomes and human confirmation remain distinct.
- Cross-document vocabulary: overlapping but non-identical status sets are explicitly preserved.
- Technical tokens: commands, paths, interfaces, timings and numerical examples retained.
- Source ambiguity: no state transitions or unified status mapping invented.
- Technical correctness: **not reviewed or validated**.
- Current status: **translated**. Semantic review and technical validation remain outstanding.

The next review should compare this translation against the Chinese source and assess the guide in the context of the process, discipline and decision catalogue. Technical claims and test-method suitability require a separate evidence-based review.
