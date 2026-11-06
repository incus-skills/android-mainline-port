# Reconstruction and translation review: `experience/kernel-and-resources.md`

## Source

- Repository: `incus-skills/android-mainline-port`
- Source branch: `main`
- Source path: `experience/kernel-and-resources.md`
- Source blob SHA: `df43365853a42d05ed2d3824c4fae759c28aa985`
- Related pattern source: `experience/patterns.md`, blob SHA `c485e34f6fb06808d7c2678b5a2280494e5f71c0`
- Related domain source: `experience/display.md`, blob SHA `6cd5016457d4cbab87fae56a4dd5e3c1204bbbdd`
- English artifact: [`translation/en/experience/kernel-and-resources.md`](../../en/experience/kernel-and-resources.md)
- Chinese reconstruction: [`reconstruction/zh/kernel-and-resources.md`](../../../reconstruction/zh/kernel-and-resources.md)
- Review status: `reviewed` in Pass 3; technical validation remains outstanding.

## Translation intent and boundary

This is a functional English translation of the kernel configuration, resource ownership and device-tree experience document. It preserves the source's recommended route, completion criteria, pitfall order, symptoms, causes, confirmation methods, fixes, prevention advice, applicability statements and unresolved question.

Technical identifiers, kernel symbols, commands, paths, filenames, configuration values and interface names are retained. The translation does not update version-sensitive claims or validate the underlying kernel and hardware recommendations.

## Reconstructed document role

The file is a domain-specific experience document in the Android mainline-porting experience library. It develops the resource-ownership diagnostic category in `experience/patterns.md` (Category B: “A resource has no actual owner”) into concrete kernel, device-tree, provider/consumer and lifecycle failure mechanisms.

It is not a formal resource-ownership specification. The source does not define a unified ownership model or a mathematical invariant. “Ownership” is retained as the diagnostic and explanatory language used by the source.

## Reconstructed structure

1. **Recommended route** — kernel baseline and patch provenance; device-tree evidence; built-in/module boundaries; firmware-retained resources; distribution configuration alignment.
2. **Completion criteria** — actual kernel configuration, module delivery/loading, deferred devices, pending sync state, absence of global bypasses and repeatable cold boots.
3. **Pitfalls** — concrete failures with varying subsets of symptoms, cause, confirmation, fix, prevention and applicability.
4. **Unresolved** — the effect of early debugging parameters on panel bring-up timing.

The source's pitfall entries are not perfectly uniform. The translation does not add missing fields or impose a new template on entries that lack them.

## Relationship to Category B in `patterns.md`

Category B supplies the general diagnostic route:

- compare resource state before and after reclamation;
- compare with stock always-on supplies, clocks and bandwidth votes;
- declare requirements on the actual consumer;
- make the provider built in and bind it before reclamation;
- validate voltage values against the driver's supported representation;
- avoid global bypasses, module blacklists and timeout-based concealment.

The domain document expands this into several distinguishable mechanisms:

- **Provider availability:** required drivers absent from the early image, incomplete module delivery, Kconfig dependency propagation and missing serial-engine dependencies.
- **Consumer declaration:** firmware-retained resources or stock always-on supplies lack a genuine mainline consumer; placeholder nodes can become false consumers.
- **Binding and lifecycle timing:** providers may bind after `late_initcall` reclamation or relevant `sync_state` processing.
- **Resource representation and operating points:** regulator voltage steps and OPP tables can make requested values invalid even when the provider/consumer structure exists.
- **Diagnostic and workaround effects:** debug reads can affect state; global ignore parameters and partial module sets can conceal the underlying model.

These are related dimensions, not one interchangeable failure mechanism.

## Cross-document relationships

- `patterns.md` routes Category B investigations to this file and `display.md`.
- `display.md` identifies resource reclamation as a major risk of the borrowed-firmware-display route and explicitly points to this file.
- `display.md` also describes the OPP/`-ERANGE`/power-vote failure mechanism, which is developed in more detail here.
- The source establishes these particular cross-references. It does not establish a complete dependency graph across all experience documents.

## Terminology decisions

| Source expression | English rendering | Reason |
|---|---|---|
| 内核配置、资源归属与设备树数值 | Kernel Configuration, Resource Ownership and Device-Tree Values | Retains the three concerns in the source title. |
| 真正的消费者 | genuine consumer / actual consumer | Distinguishes the device that really requires a resource from a placeholder or inferred consumer. |
| 提供者 | provider | Preserves the provider/consumer relationship used in kernel resource management. |
| 固件留下的资源 | resources left active by firmware / firmware-retained resources | Describes inherited active state without implying that firmware remains the runtime owner. |
| 资源回收 | resource reclamation | Used for kernel disabling of resources deemed unused. |
| 借用固件画面 | borrowed firmware display | Preserves the source's distinction from native display takeover. |
| 慢启动态 | “slow-start state” | Retains the source's quoted descriptive label without elevating it into a formal kernel state. |
| 常开 / 启动时开启 | always-on / boot-on | Retains regulator/device-tree terminology. |
| 电源投票 | power vote | Preserves the resource/power-domain usage in the source. |
| 未解决 | Unresolved | Retains the explicit unresolved boundary. |

## First-pass observations

1. **Resource ownership is both structural and temporal.** A real consumer must be represented, and the provider must bind before the kernel reaches the relevant reclamation point. Eventual binding is not sufficient in the modular display-clock example.
2. **The source distinguishes missing ownership from invalid resource representation.** A consumer/provider relationship can be absent or incomplete; separately, a voltage or OPP value can be unrepresentable or outside the supported table.
3. **The early-image/module issue is broader than a missing file.** The source describes incomplete module delivery and dependency propagation that can make an entire provider chain unavailable at the required time.
4. **Temporary bypasses are diagnostic, not accepted final architecture.** Some parameters may briefly help determine whether reclamation is involved, but the source explicitly requires their removal after diagnosis.
5. **Observation can be intervention.** Debug-interface reads may wake subsystems, trigger bus transactions or hang when accessing powered-off hardware. The source therefore treats collection side effects as part of the evidence problem.
6. **The display document is a concrete consumer of this knowledge.** Borrowed firmware display inherits resource state that Linux must account for; the cross-reference is supported by both documents.
7. **The unresolved timing question remains separate.** The source records that early debugging parameters affect panel bring-up timing but does not establish why. No resource-reclamation explanation has been inferred for it.
8. **Completion is stronger than initial feature appearance.** The acceptance boundary includes configuration values, module delivery/loading, explainable deferred devices, completed sync state, no global bypasses and repeated cold boots.

## Ambiguities and limits preserved

- The source does not formally define “resource ownership” or specify a single ownership invariant.
- It does not define priority or exclusivity among Category B and other problem patterns.
- It does not fully formalise the relationship between every provider, consumer and resource in a graph.
- Some pitfall entries omit fields such as prevention or confirmation; no missing procedure has been invented.
- “Slow-start state” is a quoted descriptive phrase, not an independently established kernel state.
- The cause of the early-debug-parameter timing effect remains unresolved.
- Version-sensitive references (for example, mainline 7.x and 2026 Qualcomm symbols) are translated as source claims and have not been checked against current upstream state.

## Pass 3 source-based semantic review

The source and English translation were compared across the five recommended-route steps, four completion criteria, all 15 pitfall entries and the unresolved timing question. **No source-driven semantic correction was required.** The English artifact remains unchanged.

The comparison confirmed preservation of:
- the stable-kernel and provenance-bearing patch route, device-derived device-tree values, built-in/module/initramfs boundaries, firmware-retained resource requirements and distribution-configuration comparison;
- the completion conditions for actual `.config` values, delivered and loaded modules, explainable deferred devices, completed `sync_state`, absence of global bypasses and repeated cold boots;
- all 15 pitfalls, their source-specific markers and the fields actually present in each entry;
- the distinction between provider availability, genuine consumer declaration, provider binding before reclamation, invalid regulator/OPP representation, dependency propagation and observation side effects;
- the different timing points represented by `late_initcall`, `clk_disable_unused`, `sync_state`, provider binding and runtime resume;
- the stated diagnostic-only role of temporary ignore parameters and module pruning, and the instruction to remove such workarounds after diagnosis;
- the unresolved effect of `keep_bootcon`, `initcall_debug` and `ignore_loglevel` on panel bring-up timing, without assigning an unsupported cause;
- cross-document references to `patterns.md`, `display.md`, `verification.md` and `ubuntu-userspace.md`.

Technical identifiers, configuration symbols, commands, paths, interface names, numeric values, voltage examples, time intervals, kernel versions and applicability labels were retained. The source's varying pitfall-entry structure remains varying; no absent confirmation, prevention or cause field was supplied.

### Pass 3 distinctions and boundaries

- “Resource ownership” remains the source's diagnostic language, not a newly formalised ownership model or invariant.
- A resource lacking a genuine consumer is distinct from a consumer/provider relationship that exists but requests an unrepresentable voltage or unsupported operating point.
- Eventual provider binding is distinct from binding before resource reclamation; the modular display-clock example depends on that timing distinction.
- A provider being built in is not treated as a substitute for declaring the resource on its actual consumer.
- Temporary diagnostic bypasses are not elevated into accepted final configuration.
- Debug-interface reads are not assumed to be passive observations; the source explicitly identifies possible state-changing side effects.
- The 15 entries do not establish a complete taxonomy of kernel/resource failures.
- The unresolved early-debug-parameter timing question remains open.

### Pass 3 disposition

- Source-based semantic review: **completed**.
- Translation changes: **none required**.
- Technical correctness, current upstream status, cross-device generality, safety and real-device behaviour: **not validated**.
- Whole-library validation: **outstanding**.
- Next document: `experience/display.md`.

This review establishes translation fidelity only. It does not validate the kernel and hardware recommendations or turn the source's diagnostic model into a formal specification.

## Chinese reconstruction surface

A separate Chinese reconstruction was created at `reconstruction/zh/kernel-and-resources.md`, with a directory-level boundary and purpose in `reconstruction/zh/README.md`.

That surface records our observer position, source provenance, recovered structure, cross-document relationships and sufficiency boundary in Chinese. It is not a duplicate of the source and is not an author-authored interpretation. Its retrospective creation is explicit; it does not claim that the record existed at the time of the original observations.

The Chinese reconstruction and this English translation review have related content but different roles:
- the functional English translation expresses the source's operational instructions;
- the Chinese reconstruction preserves our descriptive understanding and its provenance;
- this review record documents translation decisions and review boundaries.

## Review boundary

This record documents the translation and first-pass structural reconstruction. It does not establish that:
- the kernel configuration advice is technically correct for current kernels;
- the reported symptoms and causes reproduce on other devices;
- the proposed fixes are safe or sufficient in every applicable environment;
- the source's upstream/version references remain current;
- the document defines a complete or formal resource-ownership model.

Those questions require a separate semantic/technical review, and where relevant, source-code inspection and real-device evidence. The Chinese source remains authoritative for this translation pass.
