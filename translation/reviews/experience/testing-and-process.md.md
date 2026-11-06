# Translation Review — Testing Judgement, Automation, Collaboration and Process

## 1. Source and purpose

- Source: `experience/testing-and-process.md`
- Source branch: `main`
- Source blob SHA: `ff1dbea0833dc9e9eb2f668000855f73ec0a9599`
- English translation: `translation/en/experience/testing-and-process.md`
- Pass: **Pass 1 — initial structural and linguistic reconstruction**
- Review purpose: preserve the source's claims, distinctions, examples, severity markers and applicability boundaries in English.
- Technical validation: **not performed**.

This review records translation and structural observations. It does not establish that the technical procedures or recommendations are correct.

## 2. Document role and shape

The source is a pitfall collection. It has no introductory recommended route, acceptance checklist, formal process sequence, or dedicated unresolved section. Its entries range across test evidence, host-side tooling, user participation, device concurrency, diagnostic reasoning, agent delegation, reuse of prior work, release communication, device safety and community contribution review.

The source title names testing judgement, automation, collaboration and process. The body is not organised into those four explicit sections; it is a single sequence of pitfalls. The translation preserves that shape rather than introducing a taxonomy or workflow.

## 3. Main distinction groups visible in the entries

### Evidence and acceptance

- Running code, an active service, a successful probe or a QEMU pass is not equivalent to user-visible function.
- Offline evidence is labelled separately from real-device evidence.
- New features are to be isolated on RAM images before integration.
- Changes to boot-path components require cold-boot evidence from an image containing the change.
- User-level issues are closed only through the user's actual entry point.
- Human confirmation and machine/hardware evidence are recorded separately.

### Test validity and measurement

- The test harness, criteria, input data and observation method can themselves be faulty.
- Checks should include a negative input that is expected to fail.
- Evidence saved before a tool fix can be re-evaluated offline without repeating the device test or relaxing the criterion.
- Root-cause claims require an attempt at falsification using another data point or a control.
- Exclusion experiments require a clean boot.
- An apparent lack of improvement remains qualified when other conditions are unknown.

### Host/device boundary and user participation

- Host scripts are treated as part of the test system, not as reliable infrastructure by default.
- The source lists protocol, terminal, command-exit, output-capture, addressing and device-enumeration failure modes.
- User actions require advance notice and a clear observation target; long-running recording may replace short interaction windows.
- Automatable desktop actions should not be delegated to the user, but injected input is not treated as proof of physical touch.
- Device evidence is bound to boot ID, and only one operator may act on the device at a time.

### Diagnostic direction and context

- Repeated failures should trigger a search for the earliest failing layer rather than another terminal-layer workaround.
- A hung boot is not a valid baseline for exclusion experiments.
- Comparable projects and their retrospectives should be inspected before designing a process.
- Community implementations are compared at the level of their foundations before deciding whether to switch baselines or import feature families.
- Desktop session state and automatic behaviour are test conditions that may need explicit control.

### Collaboration, continuity and safety

- NO-GO records preserve rejected approaches and reverted changes across context compression or handoff.
- Sub-agent tasks are constrained by a single approach, allowed files, one verification command, pass criteria and a stopping condition.
- A sub-agent's test claim is not accepted unless the test method matches real use.
- Critical device-safety constraints must be placed in the actual task brief, not left only in the experience library.
- The candidate-kernel incident illustrates a gap between the existence of prior rules and their delivery to the active operator.

### External communication and contribution boundaries

- External documentation is for first-time project users, with conclusions owned in one place and reviewed before release.
- Community contributions are accepted where possible, but unrelated or legally/ethically risky changes are excluded; provenance is recorded after merge.

## 4. Explicit cross-document relationships

1. `experience/README.md` lists this document as covering judgement criteria, host scripts, automated testing, parallel collaboration, sub-agents and cooperation with the user. It also directs readers to `patterns.md` before searching domain-specific entries.
2. `experience/patterns.md` explicitly cites `testing-and-process.md` under:
   - **Pattern E — The measurement method itself is wrong**, with examples including offline success followed by real-device failure, runtime switching in place of cold boot, and host-script address/line-ending problems.
   - **Pattern F — Passing at a lower layer does not mean the user can use it**, with examples including `hci0 up`, camera capture and shadow-buffer display.
   - **Pattern H — Another implementation cannot be copied wholesale**, including the need to compare foundations before reusing community work.
   - **Pattern I — The recovery path itself is unreliable**, including candidate-kernel installation, GPU hang and shutdown/recovery failure.
3. The source body itself does not contain links to these documents. The relationships are explicit in the collection index and pattern index, not inline links in this file.

## 5. Observer-reconstructed correspondences

These are descriptive correspondences across the source material, not a source-defined formal model:

- **Evidence ladder:** service/probe/emulator evidence, hardware indicators, user-perceptible output and human confirmation are distinct evidence surfaces.
- **Test system as an object of testing:** criteria, harnesses, test data, host scripts and observation methods can produce false results.
- **Falsifiability:** negative inputs, controls and second data points are used to challenge an apparent pass or root-cause explanation.
- **Clean-state dependence:** boot identity, clean boots, RAM images, user/session state and prior package/slot contents affect whether results are attributable.
- **Operator and task serialization:** one operator per device, boot-ID-bound evidence and explicit handoff/task constraints limit interference.
- **Knowledge delivery versus knowledge storage:** the candidate-kernel incident distinguishes a rule existing in the library from that rule being present in the operator's task brief.
- **Recovery as a prerequisite to experimentation:** test actions are constrained by the ability to regain device access; unattended driver operations and candidate writes carry explicit recovery risks.
- **User-facing acceptance versus automation:** automation reduces avoidable user actions but does not erase the distinction between injected software events and physical interaction.

## 6. Translation decisions and terminology

- **判断** is rendered as **judgement** where it concerns the act of evaluating evidence or a result; **acceptance criteria** is used only where the entry concerns a test's pass/fail standard.
- **跑起来了 / 能用了** is rendered as **"it runs" / "it works"** to retain the contrast between execution and usable function.
- **真机** is rendered as **real device**; **离线** as **offline**. The labels "offline passed" and real-device evidence are kept distinct.
- **冷启动** is rendered as **cold boot**.
- **子 agent** is rendered as **sub-agent**; **主 agent** as **main agent**.
- **NO-GO** is retained as the source's record name.
- Severity labels are represented as **[Frequent]**, **[Time-consuming]** and **[Critical]**. No new marker is inferred where the source has none.
- Technical identifiers, commands, kernel parameters, configuration symbols, hashes and quoted error text are preserved.
- The translation retains the source's imperative force, including prohibitions on unattended driver unload/rebind and continuing tests after the first hardware failure. This is translation fidelity, not an independent safety endorsement.

## 7. Structural absences and uncertainty

- There is no dedicated "recommended route", "acceptance criteria" or "unresolved" section in this source.
- The title's four subject areas are not used as explicit body headings.
- The source does not define a formal evidence-state machine, test orchestration architecture, concurrency protocol, sub-agent lifecycle or general diagnostic algorithm.
- "One operator at a time" is explicit, but the source does not specify a locking mechanism or ownership protocol.
- It says to record each evidence item with a boot ID, but does not define a complete evidence-record schema.
- It prescribes one verification command in a sub-agent task brief, but does not define how to choose that command for every task.
- It requires a negative input for each check but does not specify a universal representation for negative cases.
- It gives concrete safety procedures and kernel settings; their correctness and applicability to other devices/kernels have not been assessed.
- The source includes GNOME-specific and 2026-specific cases alongside general entries. The applicability labels are preserved rather than generalised.

No gap is filled by inventing a process model. These absences are not treated as defects during this first reconstruction pass.

## 8. Bounded sufficiency

For the stated Pass 1 purpose, the source's pitfall sequence, evidence distinctions, tool/measurement failure cases, host/device and user boundaries, collaboration constraints, safety incident, contribution rule, applicability labels and explicit Pattern E/F/H/I correspondences have been recovered.

Remaining questions do not prevent this translation/reconstruction from proceeding because they do not alter the faithful representation of what the source states. A later pass could investigate implementation-specific task templates, evidence schemas, orchestration or safety controls, but only if the scope changes or new discriminating evidence is identified.

## 9. Validation boundary

- Source comparison: performed for translation.
- Structural reconstruction: performed at document and collection-reference level.
- Translation review: recorded here.
- Technical validation: not performed.
- Operational validation in an Android porting environment: not performed.
- Whole-skill English validation: not performed.
