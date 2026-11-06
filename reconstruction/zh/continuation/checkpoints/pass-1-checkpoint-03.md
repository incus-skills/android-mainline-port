# Pass 1 — Checkpoint 03: GPU and Graphics Stack

## 1. Position and scope

- Traversal: **Pass 1 — initial structural and linguistic reconstruction**
- Previous checkpoint: [Pass 1 / Checkpoint 02](pass-1-checkpoint-02.md)
- Completed source: experience/gpu.md
- Source branch: main
- Source blob SHA: 64079b8f8e5913de51114f4459bc1bc628e14791
- English translation: translation/en/experience/gpu.md
- Translation review: translation/reviews/experience/gpu.md.md
- Chinese descriptive reconstruction: reconstruction/zh/gpu.md
- Next source: experience/touch-and-input.md
- Technical validation: not performed

This checkpoint records movement within Pass 1. It does not begin a new pass or expand the declared first-pass purpose.

## 2. Reconstructed document role

The GPU document is a domain-specific experience record for the Adreno GPU and graphics stack. It is organised as a recommended route, completion criteria, concrete failure records and unresolved issues.

Its route spans kernel support, firmware, Mesa recognition and installation, sandbox application libraries, and commit-specific backports for immature new-GPU support. It does not define a universal graphics architecture or formal GPU power state machine.

## 3. Material distinctions recovered

The document distinguishes:

- Kernel driver support from firmware availability and device-specific signing acceptance.
- Firmware and kernel readiness from Mesa's recognition of the complete chip ID.
- Host Mesa from Snap/Flatpak-provided GL libraries.
- Hardware acceleration from software fallback.
- Renderer identity, pixel-correct output, sustained stability, compositor use and complex application behaviour as separate acceptance dimensions.
- GL and Vulkan as distinct rendering paths.
- A rendering/capture-tool limitation from an actual failure to render.
- Confirmed fixes and local workarounds from unresolved new-GPU behaviour.

The pitfall entries are not structurally uniform. Their omissions have not been filled by inference.

## 4. Explicit cross-document relationships

### kernel-and-resources.md

The GPU document explicitly links to this file when describing a GMU/GPU timeout that can result from display-resource reclamation. Borrowing the firmware display can allow the display-bus clock to be reclaimed, slowing the memory path and presenting as a GMU HFI timeout or hangcheck.

The same entry also names alternative GPU-side power conditions: an MX dependency vote using the wrong OPP table, or a power domain entering retention. Its diagnostic instruction is to rule out display-side resource problems before investigating GPU power votes.

This is a diagnostic possibility and ordering, not a universal causal explanation for GMU timeouts.

### ubuntu-userspace.md

The recommended route explicitly refers to this document for the separate GL libraries used by Snap and Flatpak.

### touch-and-input.md

The GPU source does not directly refer to touch. The preceding display document links high-refresh-rate testing with touch input and points to touch-and-input.md. Whether further explicit relationships exist is left for examination of that source.

## 5. Observer-reconstructed correspondences

The reconstruction identifies, without elevating them into source terminology:

- Resource continuity across display and GPU memory paths.
- Layered failure responsibility across kernel, firmware, Mesa, sandbox libraries and applications.
- The difference between a test result and evidence that hardware acceleration was actually used.
- Measurement limitations in frame capture and the distinction between cursor and primary planes.
- Concurrency and power-transition conditions that short or single-context tests may not expose.
- The relationship between upstream commit lineage, local backports and technical-debt retirement.

These are observer correspondences, not an additional formal classification.

## 6. Unresolved and evidence boundaries

The source explicitly leaves new-GPU Vulkan stability, sustained load, frequency scaling and power management unresolved.

It does not establish:
- A universal firmware selection/signing procedure.
- A formal GPU power-transition state model.
- A complete test matrix.
- A general maturity threshold for Mesa support beyond replacing local backports when an upstream release includes the fix.

This pass has not validated the technical correctness or current status of kernel, firmware, Mesa, Turnip, GNOME, QEMU or device-specific claims. These questions belong to a separate technical evidence pass and do not prevent continuing the structural and linguistic traversal.

## 7. Why touch and input is next

The experience-library index places experience/touch-and-input.md immediately after experience/gpu.md. The display document has already identified a cross-subsystem acceptance relationship: high-refresh-rate display operation must be tested together with touch input.

The next document can therefore be examined on its own terms while checking whether it makes that relationship explicit, adds further links, or reveals distinct constraints. The prior display statement is a reason to inspect it, not a conclusion about what the touch document must contain.

## 8. Re-entry sequence

1. Read reconstruction/zh/continuation/current-position.md.
2. Read this checkpoint, then Checkpoint 02 and Checkpoint 01 as needed.
3. Read reconstruction/zh/README.md and translation/INVENTORY.md.
4. Revisit the relevant source and reconstruction records for patterns.md, kernel-and-resources.md, display.md and gpu.md as needed.
5. Independently inspect experience/touch-and-input.md. Distinguish what it explicitly states from expectations created by the preceding documents.

## 9. Position change

Pass 1 remains the same declared traversal. The document position has advanced from display.md (Checkpoint 02) through gpu.md (Checkpoint 03). The next position is touch-and-input.md. Pass number, document position and session checkpoint number remain separate coordinates.
