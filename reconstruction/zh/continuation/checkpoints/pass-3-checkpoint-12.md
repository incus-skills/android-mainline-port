# Pass 3 — Checkpoint 12: `experience/kernel-and-resources.md` Semantic Review

## Position

- Pass: **Pass 3 — technical and semantic review of the operational English translation set**
- Document: `experience/kernel-and-resources.md`
- Source branch: `main`
- Source blob SHA: `df43365853a42d05ed2d3824c4fae759c28aa985`
- English translation: `translation/en/experience/kernel-and-resources.md`
- Translation review record: `translation/reviews/experience/kernel-and-resources.md.md`
- Previous document: `experience/bootloader-and-boot.md`
- Next document: `experience/display.md`
- Technical correctness validation: not performed.

## Review result

The source and English translation were compared across the five recommended-route steps, four completion criteria, all 15 pitfall entries and the unresolved timing question. **No source-driven semantic correction was required.** The English artifact remains unchanged.

The comparison confirmed preservation of:
- the stable-kernel and patch-provenance route, device-derived device-tree values, built-in/module/initramfs boundaries, firmware-retained resources and distribution-configuration alignment;
- completion conditions for actual `.config` values, delivered and loaded modules, explainable deferred devices, completed `sync_state`, absence of global bypasses and repeated cold boots;
- all 15 pitfalls, their source-specific markers and the fields actually present in each entry;
- provider availability, genuine consumer declaration, provider binding before reclamation, invalid regulator/OPP representation, dependency propagation and observation side effects as distinct concerns;
- timing distinctions represented by `late_initcall`, `clk_disable_unused`, `sync_state`, provider binding and runtime resume;
- the diagnostic-only role of temporary ignore parameters and module pruning;
- the unresolved effect of `keep_bootcon`, `initcall_debug` and `ignore_loglevel` on panel bring-up timing;
- cross-document references to `patterns.md`, `display.md`, `verification.md` and `ubuntu-userspace.md`.

Technical identifiers, configuration symbols, commands, paths, interface names, numeric values, voltage examples, time intervals, kernel versions and applicability labels were retained. The source's varying pitfall-entry structure remains varying; no absent confirmation, prevention or cause field was supplied.

## Distinctions and boundaries preserved

- “Resource ownership” remains the source's diagnostic language, not a newly formalised ownership model or invariant.
- A missing genuine consumer is distinct from an existing provider/consumer relationship with an unrepresentable voltage or unsupported operating point.
- Eventual provider binding is distinct from binding before resource reclamation.
- A built-in provider does not substitute for declaring the resource on its actual consumer.
- Debug-interface reads are not assumed to be passive observations.
- The 15 entries do not establish a complete taxonomy of kernel/resource failures.
- The unresolved early-debug-parameter timing question remains open.

## Pass 3 disposition

- Source-based semantic review: **completed**.
- Translation changes: **none required**.
- Technical correctness, current upstream status, cross-device generality, safety and real-device behaviour: **not validated**.
- Whole-library validation: **outstanding**.
- Next document: `experience/display.md`.

This review establishes translation fidelity only. It does not validate the kernel and hardware recommendations or turn the source's diagnostic model into a formal specification.
