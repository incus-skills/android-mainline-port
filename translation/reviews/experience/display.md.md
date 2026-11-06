# Reconstruction and translation review: `experience/display.md`

## Source

- Repository: `incus-skills/android-mainline-port`
- Source branch: `main`
- Source path: `experience/display.md`
- Source blob SHA: `6cd5016457d4cbab87fae56a4dd5e3c1204bbbdd`
- Related diagnostic source: `experience/patterns.md`, blob SHA `c485e34f6fb06808d7c2678b5a2280494e5f71c0`
- Related resource source: `experience/kernel-and-resources.md`, blob SHA `df43365853a42d05ed2d3824c4fae759c28aa985`
- English artifact: [`translation/en/experience/display.md`](../../en/experience/display.md)
- Chinese reconstruction: [`reconstruction/zh/display.md`](../../../reconstruction/zh/display.md)
- Review status: `reviewed` in Pass 3; external technical validation remains outstanding.

## Translation intent and boundary

This is a functional English translation of the display experience document. It preserves the two display routes, their trade-offs, the conditional recommendation, native-takeover sequence, completion criteria, pitfall entries, applicability labels and unresolved issues.

Kernel symbols, commands, paths, interface names, protocol values, register identifiers, error codes, frequencies and other technical identifiers are retained. The translation does not validate current upstream behaviour, vendor implementations, device-specific findings or version-sensitive claims.

## Reconstructed document role

This is a domain-specific experience document for display bring-up and integration in Android mainline porting. It covers more than display-driver implementation: route selection, firmware-state continuity, display-pipeline configuration, panel sequencing, power/resource management, lifecycle behaviour, desktop integration and acceptance evidence.

The source presents practical recommendations and cases. It does not define a formal display state machine, a complete dependency graph, or a universal implementation architecture.

## Reconstructed structure

1. **Recommended route** — compares borrowed firmware display (`simple-framebuffer` + `simpledrm`) with native takeover (DPU + DSI + panel driver), then gives a conditional route recommendation.
2. **Native takeover steps** — capture a vendor golden snapshot; recover panel commands; use modular loading for initial diagnosis; preserve the firmware display through SMMU identity mapping until reset; establish the firmware refresh rate before adding modes; then implement lifecycle features.
3. **Completion criteria** — combines human-visible output, mode-specific hardware indicators, repeatable cold boots and display/touch recovery after repeated cycles. It explicitly rejects several software-only indicators as proof.
4. **Pitfalls** — records failures across resources, panel and DSC configuration, pipeline topology, mode validation, SMMU, lifecycle, backlight, GNOME integration, diagnostics and non-failure warnings.
5. **Unresolved** — retains a same-SoC display recovery problem and an incomplete high-refresh clock calculation.

Pitfall entries have different levels of detail. The translation does not impose a uniform template or invent missing fields.

## Relationship to problem patterns

The document gives concrete display-domain instances of several general diagnostic concerns:

- **Pattern B — resource has no actual owner:** borrowed firmware display can fail when Linux reclaims clocks, power domains or interconnect bandwidth that remain necessary for firmware scanout. The document points directly to `kernel-and-resources.md`.
- **Pattern C — timing and lifecycle are wrong:** display-off/display-on, DPMS, suspend/resume, power-event ordering, vblank and backlight restoration all depend on sequencing and lifecycle.
- **Pattern E — the measurement method is wrong:** connector state, framebuffer writability and software component status are insufficient. Acceptance requires visible output and appropriate hardware indicators; static-screen testing can conceal failures.
- **Pattern F — low-level success does not mean the user can use it:** GNOME connector classification, brightness controls, login visibility and desktop behaviour form part of the user-facing outcome.
- **Pattern H — another implementation cannot simply be copied wholesale:** vendor and community implementations provide evidence and candidate code, but panel commands, topology, parameters and board-specific values must be checked against the actual device.

These are observational correspondences with the general pattern descriptions, not an exclusive classification. A single symptom can involve more than one concern, as the source itself states for the general pattern library.

## Relationship to kernel and resource material

The source establishes several specific links and shared mechanisms:

1. The borrowed-display route relies on firmware scanout continuing while Linux draws into the existing framebuffer. It does not give simpledrm ownership of the display hardware.
2. Linux can reclaim resources that have no declared consumer. The display document identifies this as the main risk of borrowed display and refers to the kernel/resource document.
3. The native-takeover sequence adds the display subsystem to the SMMU identity list so firmware scanout survives until the display driver resets the hardware. This is a distinct address-translation continuity issue, not the same mechanism as clock or power-resource reclamation.
4. The black-screen pitfall includes a panel supply voltage that does not match PMIC regulator steps, which can cause the PMIC's regulators to disappear. The kernel/resource document develops this representation failure.
5. High-refresh mode validation must not be bypassed by requesting clocks outside the OPP table. The source connects this to `-ERANGE` and lost power votes; the kernel/resource document describes that failure mechanism in more detail.
6. The display document adds the subsystem-specific symptoms, diagnostic signals and acceptance conditions. It does not establish that every display failure is a resource-ownership problem.

## Internal distinctions preserved

- **Borrowed display versus native takeover:** these are different operating routes, not simply two quality levels of the same implementation. The borrowed route preserves a firmware-established scanout path; native takeover reinitializes the display hardware.
- **First light versus product capability:** initial visible output is an early milestone, not completion of native display integration.
- **Software state versus actual display:** connected connectors, writable framebuffer nodes and active processes do not prove visible or stable output.
- **Pipeline configuration versus resource availability:** incorrect mixer topology or DSC values differ from missing providers, invalid voltage steps or reclaimed resources.
- **Static output versus lifecycle correctness:** a static login screen can conceal failures that appear only when new frames are committed or the display is cycled.
- **Observed warning versus functional failure:** the frame-done/pending warning is described as harmless in the stated upstream ordering case; it should not be collapsed into the genuine timeout cases.
- **Device-specific success versus general unresolved support:** one device's stable 144 Hz result does not resolve the broader clock-calculation problem for some DSC panels.

## First-pass observations

1. The source recommends native takeover when upstream SoC support and a reusable panel driver are available. Otherwise, borrowed display is an interim route, with native work started in parallel.
2. The borrowed route is deliberately bounded: it helps establish first light, installation and desktop access, but lacks capabilities required for a complete product display path.
3. Native takeover begins with evidence capture from the vendor system. The golden snapshot preserves topology, clocks, DSC, PHY and panel command information for comparison.
4. Initial modular loading is a diagnostic and recoverability choice: it allows the display driver to be loaded after a debugging channel is available.
5. SMMU identity mapping preserves the firmware's existing scanout until the native driver reaches the hardware-reset point. This is separate from declaring Linux consumers for resources that must survive reclamation.
6. Acceptance is deliberately multi-layered: human-visible image, hardware counters/registers, repeated cold boots and recovery after display/touch cycles.
7. The source treats observation and test design as consequential. Reading state may not be sufficient, and static test scenes can miss lifecycle failures; the acceptance procedure must provoke new frames and transitions.
8. Some pitfall entries describe confirmed cases, while others give general or device-specific mechanisms. Their stated applicability is retained rather than generalized.
9. The final 144 Hz note records a successful device result, while the unresolved section still identifies a broader clock-calculation gap. These statements are not contradictory.
10. The document explicitly connects display testing with touch testing at high refresh rates, creating a cross-domain validation dependency without defining a full subsystem dependency graph.

## Ambiguities and limits preserved

- The source does not define a formal state machine for display lifecycle or a complete ordering model for every power event.
- It does not provide a universal rule for selecting borrowed display versus native takeover beyond the stated conditions.
- It does not formalize the complete SMMU identity-domain mechanism or its scope across SoCs.
- The exact hardware and software details behind some pitfalls are device- or implementation-specific; the stated applicability labels are preserved.
- The source does not establish that all GNOME, Mutter, DPU or mainline behaviours apply beyond the stated versions and environments.
- The same-SoC `dsi_err_worker status=4` recovery problem remains unresolved.
- A more accurate display-core clock calculation remains unresolved for some DSC panels.
- No technical claim has been independently validated in this pass.

## Sufficiency boundary

The document is sufficiently reconstructed for the current purpose when its route distinction, operational sequence, acceptance model, diagnostic categories, cross-document relationships and unresolved boundaries can be carried into the next source document without treating the display-specific cases as a universal architecture.

Technical validation, current upstream comparison and device-specific reproduction remain separate work.


## Pass 3 source-based semantic review

The complete Chinese source and English translation were compared section by section against the Phase 4 review dimensions.

One wording correction was made in the borrowed-display capability table: "prerequisites for suspend" was changed to "suspend support". The source lists this alongside missing display capabilities; the former English phrase could be read as saying that prerequisites are absent rather than that suspend capability is unavailable.

The comparison confirmed preservation of:
- the distinction between borrowed firmware scanout and native DPU/DSI/panel takeover, including the conditional route recommendation and parallel start of native work when borrowing is necessary;
- all six native-takeover steps, their ordering, diagnostic rationale and technical identifiers;
- human-visible first light, mode-specific hardware indicators, cold-boot repetition, lifecycle recovery and explicit non-acceptance indicators;
- the distinct pitfall cases, their symptoms, causes, confirmation methods, remedies, prevention notes, tags and applicability labels;
- the distinction between SMMU identity mapping for firmware scanout continuity and Linux resource-consumer declaration/resource reclamation;
- the distinction between static output and lifecycle correctness, and between harmless accounting warnings and actual functional failures;
- the device-specific 144 Hz result alongside the broader unresolved clock-calculation issue;
- the explicit high-refresh touch-testing reference and related resource/pattern relationships.

The source's uneven pitfall-entry structure remains uneven. No formal display state machine, complete dependency graph, universal architecture or technical validation claim has been added. The unresolved same-SoC `dsi_err_worker status=4` issue and high-refresh display-core clock calculation remain open.

### Pass 3 disposition

- Source-based semantic review: **completed**.
- Translation changes: **one wording clarification** ("suspend support").
- External technical correctness, current upstream behaviour, vendor implementation accuracy and real-device behaviour: **not validated**.
- Whole-skill validation: **outstanding**.
- Next document: `experience/gpu.md`.
