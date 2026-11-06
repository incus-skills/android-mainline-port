# Translation Review: GPU and Graphics Stack

## Source provenance

- Source: experience/gpu.md
- Source branch: main
- Source blob SHA: 64079b8f8e5913de51114f4459bc1bc628e14791
- English artifact: translation/en/experience/gpu.md
- Reconstruction pass: Pass 1 — initial structural and linguistic reconstruction
- Review scope: source structure, operational meaning, terminology, explicit relationships, ambiguities and evidence boundaries
- Technical validation: not performed

## Document role

The document is a domain-specific experience record for the Adreno GPU and graphics stack. It is not a GPU driver specification or a general graphics architecture. It gives a recommended investigation and integration route, observable completion criteria, confirmed problem entries and a short unresolved-work list.

Its route crosses several layers:

1. Kernel support through drm/msm, clocks, GMU, SMMU and OPP data.
2. Firmware, including sqe, GMU and zap shader, with device-specific signing constraints.
3. Mesa recognition and isolated installation of a locally built userspace stack.
4. Sandboxed application graphics libraries.
5. Backports and technical-debt tracking while new-generation Mesa support matures.

The document's operational concern is not merely whether a driver loads. It distinguishes hardware acceleration, correct rendering, sustained operation, power transitions, compositor use and application-level rendering paths.

## Internal structure

### Recommended route

The five numbered steps form a layered route from kernel and firmware through Mesa and application packaging. The fifth step is conditional: it addresses the case where Mesa support for a new GPU is immature and requires upstream fixes to be tracked and backported.

This is a recommendation, not a complete dependency graph or mandatory state machine. The source does not specify a universal order for every firmware or platform combination.

### Completion criteria

Four different outcomes are required:

- Hardware renderer identity rather than software fallback.
- Pixel-correct output.
- Stability through repeated power transitions and an extended run without GMU errors or hangchecks.
- Actual GPU use by the desktop compositor and correct rendering in complex applications, including both GTK GL and Vulkan paths.

These criteria prevent a successful render test from standing in for the complete graphics experience.

### Pitfall records

The entries cover distinct failure surfaces:

- Device identification and software-renderer fallback.
- Display-resource reclamation appearing as a GMU/GPU timeout.
- Mesa shared-context concurrency.
- Turnip buffer-to-image row-stride handling.
- Incomplete texture-format paths.
- Kernel GMEM base-address reporting.
- Vulkan hangs.
- False-positive tests under software rendering.
- Frame-capture and compositor/login-screen limitations.

The entries do not all use an identical field set. Some have symptoms, cause, confirmation, resolution and applicability; others omit fields not supplied by the source. The translation does not manufacture missing fields.

## Explicit relationships to other documents

### kernel-and-resources.md

The GPU document explicitly refers to this file in the resource-reclamation entry. It states that when the firmware display is borrowed, reclamation of the display-bus clock can slow the memory path and manifest as a GMU HFI timeout or hangcheck.

The entry also identifies two alternative GPU power-related possibilities: an MX dependency vote using the wrong OPP table, or a power domain entering retention. Its prescribed diagnostic order is to rule out display-side resource problems first, then inspect GPU power votes.

This is an explicit cross-document link and an explicit diagnostic ordering. It does not establish that every GMU timeout is caused by display resource reclamation.

### ubuntu-userspace.md

The recommended route explicitly directs the reader to this document for Snap and Flatpak GL libraries. This establishes a userspace packaging concern distinct from the kernel/Mesa driver path.

### touch-and-input.md

There is no explicit reference to touch in this GPU source. The preceding display document requires high-refresh-rate testing together with touch input and links to touch-and-input.md. This remains a relationship in the wider display/GPU testing context only if later source evidence establishes it; it is not a GPU-document claim.

## Observer-reconstructed correspondences

The following are useful cross-domain readings, not source-defined categories or a new taxonomy:

- **Resource continuity:** display-bus clock reclamation can affect the GPU memory path even though the visible symptom is reported by GMU/GPU monitoring.
- **Layered responsibility:** kernel support, firmware acceptance, Mesa device recognition, sandbox library selection and application rendering can fail independently.
- **Representation versus reality:** a test may report success while Mesa is using softpipe/llvmpipe; the renderer identity is part of the acceptance evidence.
- **Measurement boundary:** a frame-capture tool may fail because it cannot handle a compressed format, while the display itself may still be functioning. The cursor plane is not a substitute for the primary image.
- **Concurrency and lifecycle:** shared GL contexts and power transitions expose failures that a single-context or short-lived render test may miss.
- **Upstream lineage and local debt:** commit-specific backports are tracked as technical debt so they can be replaced when upstream releases contain the fixes.

These correspondences are observations made during reconstruction. The source does not name them as a formal set of patterns.

## Important distinctions preserved

- Hardware renderer recognition is not the same as correct output; correct output is not the same as sustained stability.
- Kernel driver support, firmware availability/signing and Mesa recognition are separate requirements.
- A locally built Mesa is isolated under a separate prefix and enabled conditionally; it is not presented as a replacement for the distribution installation.
- Snap/Flatpak GL libraries are separate from the host Mesa path.
- A GMU timeout may have a display-resource cause or a GPU power-vote cause; the symptom alone does not decide between them.
- GL and Vulkan are separate rendering paths and must both be exercised where required.
- A test that passes under software rendering is not evidence of hardware acceleration.
- A capture-path limitation is not automatically a rendering failure.
- Workarounds and backports have explicit costs or retirement conditions where the source provides them.

## Unresolved and underspecified areas

- New-GPU Vulkan stability, sustained load, frequency scaling and power management remain explicitly unresolved.
- The source does not provide a universal firmware-selection/signing procedure for every device.
- It does not define a formal state model for GPU power transitions or a complete test matrix.
- It does not state a general criterion for when Mesa support is mature enough to remove local backports beyond replacing them with an upstream release.
- Several pitfall entries omit confirmation steps or applicability details; those omissions are preserved rather than filled by inference.

## Translation decisions

- “固件” is rendered as “firmware”; specific components such as sqe, GMU and zap shader are retained.
- “免 zap 路径” is rendered as a path that does not require zap, without implying that it is universally available.
- “共享上下文竞态” is rendered as “shared-context race”.
- “buffer→image 上传的行距单位错误” is rendered as a row-stride unit error during buffer-to-image upload.
- “技术债” is rendered as “technical debt”, preserving the source's requirement to track each backport and later replace it.
- “通过” in test contexts is rendered as “pass” or “passes”; the translation preserves the distinction between software test success and hardware-backed acceptance.

## Review boundary

This record assesses whether the English text preserves the source's structure and operational distinctions. It does not validate the technical correctness or current upstream status of the kernel, firmware, Mesa, Turnip, GNOME, QEMU or device-specific claims. Those require a separate technical evidence pass.


## Pass 3 — technical and semantic review

- Source revision reviewed: `experience/gpu.md` on `main`, blob SHA `64079b8f8e5913de51114f4459bc1bc628e14791`.
- English artifact reviewed: `translation/en/experience/gpu.md`.
- Review scope: all five recommended-route steps, four completion criteria, all nine pitfall entries, the unresolved-work section, technical tokens, structure and explicit cross-document references.

### Review findings and translation refinements

Four wording refinements were made to restore the source's stated force and scope:

1. The firmware-signing constraint now states that **only the device's original factory-signed file can be used** on the described retail-device path. The previous “may be required” weakened the source's explicit exclusivity.
2. The GMU HFI timeout/hangcheck symptom is stated as unrelated to GPU code, without the additional hedge “apparently”.
3. The alternative causes in that entry now retain the source's direct statements: the MX dependency vote uses the wrong OPP table, or the power domain is placed in the retention level. The previous modal “may” weakened the cause statements.
4. The shared-context race now states that it reproduces every time under the described freedreno/threaded-context conditions, preserving the force of “必现”.

No other source-driven semantic correction was identified.

### Confirmed preservation

- The five-step route retains the kernel, firmware, Mesa, sandbox-library and immature-new-GPU/backport layers without turning the recommendation into a universal dependency graph.
- The zap-free rendering route remains conditional on platform support; it is not generalised.
- All four completion dimensions remain distinct: hardware renderer identity, pixel-correct output, sustained rendering through power transitions, and compositor/application rendering including GTK GL and Vulkan.
- All nine pitfalls retain their differing field structures, symptoms, causes, confirmation steps where supplied, resolutions, applicability and time markers. No absent fields were invented.
- Full chip IDs, `drm/msm`, sqe, GMU, SMMU, OPP, zap, Mesa, Turnip, NV12, BGRx, NEON, `GALLIUM_THREAD=0`, `FD_MESA_DEBUG=sysmem`, `LD_PRELOAD`, `vkcube`, `gnome-shell --headless --virtual-monitor`, error/function identifiers and version/applicability references remain represented.
- The explicit references to `kernel-and-resources.md` and `ubuntu-userspace.md` are retained. The source does not explicitly link this GPU document to `touch-and-input.md`; no such link was added.
- The unresolved scope remains new-GPU Vulkan stability, sustained load, frequency scaling and power management.

### Structural and observer boundaries

The display-resource reclamation relationship is explicitly connected to the kernel/resource document, with GPU power-vote alternatives and diagnostic ordering preserved. It does not imply that every GMU timeout has one cause. The relationship between renderer identity and test validity, and between capture-path limitations and actual rendering, remains an observer-level correspondence rather than a source-defined taxonomy.

The source does not define a universal firmware-signing procedure, GPU power-state model, complete test matrix, or general maturity threshold for retiring Mesa backports. Those absences remain visible rather than being filled by inference.

### Pass 3 disposition

- Source-based technical and semantic translation review: **completed**.
- Translation changes: **four wording refinements**; no source document modified.
- External technical correctness, current upstream status, cross-device generality, firmware validity and real-device behaviour: **not validated**.
- Whole-skill validation: **outstanding**.
- Next document: `experience/touch-and-input.md`.

This review establishes source fidelity only; it does not validate the GPU, firmware, Mesa, Turnip or device-specific technical claims.
