# GPU and Graphics Stack

## Recommended Route

1. **Kernel:** Use upstream drm/msm support for the relevant Adreno generation, including the GPU clock controller, GMU, SMMU and OPP table. Newer GPUs often appear first in linux-next, Qualcomm review series or community kernels. Import them with their provenance, then replace them with the official upstream versions once merged.
2. **Firmware:** The relevant files are sqe, GMU and zap shader firmware. The secure world on retail devices usually rejects a zap signed with a generic key, so the device's original vendor-signed file may be required. Some platforms can first validate rendering through a path that does not require zap.
3. **Userspace:** First check whether the distribution's Mesa recognises the GPU (use the complete chip ID, not just the series number). If it does not, build Mesa yourself and install it under a separate prefix. Enable it through environment variables only when the GPU driver is loaded; removing it should return the system to the distribution version.
4. **Sandboxed applications:** Snap and Flatpak use their own GL libraries; see ubuntu-userspace.md.
5. **Immature Mesa support for a new GPU:** Video textures, multiple contexts and the Vulkan path are the most likely areas to fail. Track fixes in upstream stable and main, backport them by commit, and record each backport as technical debt. Replace the backports with the upstream release when available.

## Completion Criteria

- The renderer name identifies the hardware GPU, not llvmpipe or softpipe, and rendered output matches the expected result pixel for pixel.
- Rendering continues after repeated power transitions; there are no GMU errors or hangchecks over an extended run (for example, more than 200 seconds).
- The desktop compositor actually uses the GPU, with a frame rate close to the display refresh rate.
- Complex applications such as the browser, video player and Settings render correctly and do not crash. Test both GTK's GL and Vulkan renderers.

## Pitfalls

### The Distribution's Mesa Does Not Recognise a New GPU, so the Desktop Falls Back to Software Rendering

- **Symptoms:** Clicking and typing are noticeably delayed; the renderer is llvmpipe.
- **Cause:** The Mesa device table matches the complete chip ID (for example, it contains 0x44050000, while the device reports 0x44050001).
- **Resolution:** Build Mesa yourself and add the one-line device-table fix (upstream usually adds it soon). Check the complete chip ID, and do not mistake the APT candidate version for the installed version.
- **Applicability:** Adreno 8xx; 2026.

### A GMU or GPU Timeout Is Actually Caused by Display-Resource Reclamation

- **Symptoms:** GMU HFI timeout or hangcheck, apparently unrelated to GPU code.
- **Cause:** When borrowing the firmware display, the display-bus clock can be reclaimed, slowing the memory path (see kernel-and-resources.md). Alternatively, an MX dependency vote may use the wrong OPP table, or a power domain may be placed in retention.
- **Resolution:** Rule out display-side resource problems first, then investigate GPU power votes.
- **Applicability:** Qualcomm; 2026.

### Creating Multiple GL Contexts Concurrently Causes a Crash (Shared-Context Race)

Tag: **[Time-consuming]**

- **Symptoms:** Opening a Settings page containing several video demonstrations immediately crashes with SIGSEGV in _mesa_release_pending_resource ← delete_buffers.
- **Cause:** The generic Mesa GL frontend adds a new context to the shared list before its state tracker is ready. Under threaded context operation, traversal of that list can encounter a null pointer. freedreno always enables threaded context, so the failure is reproducible; the distribution's software renderer does not reproduce it.
- **Confirmation:** Find the context being created in the core dump. Write a stress test that creates and destroys contexts concurrently; the original version should crash and the fixed version should not.
- **Resolution:** Add the new context to the list only after it is ready, then submit the minimal fix upstream. GALLIUM_THREAD=0 is a workaround, but carries a performance cost.
- **Applicability:** General GL frontend issue in Mesa 26.x and earlier; 2026.

### Diagonal Stripes in NV12 Video and Camera Preview with the GTK Vulkan Renderer

Tag: **[Time-consuming]**

- **Symptoms:** NV12 video and camera preview in GTK applications show diagonal stripes; the GL renderer works normally.
- **Cause:** On A8xx, Turnip uses the wrong unit for the row stride when uploading a buffer to an image. Upstream stable already contains the fix, but the locally built version lacks that commit.
- **Confirmation:** Use a headless reproducer to run all modes. Temporarily change the stride multiplier and compare both the positive and negative causal outcomes.
- **Resolution:** Backport the upstream fix. Also check whether the Snap GPU provider has been updated. Until fixed, the camera can temporarily output BGRx as a workaround (with significant CPU overhead; NEON conversion can halve it). Return to NV12 after the fix.
- **Applicability:** Mesa Turnip A8xx; 2026.

### Mesa Assertion Terminates the Video Player During Playback

- **Symptoms:** SIGABRT after an assertion in fd6_texture.cc with CHIP = A8XX.
- **Cause:** Some texture-format paths for the new GPU are not yet complete.
- **Resolution:** Check whether upstream already has a fix. If not, report the issue upstream and document the affected applications in the known-issues list.
- **Applicability:** freedreno A8xx; 2026.

### An Incorrect GMEM Base Address for One GPU Generation Causes Corruption in Browsers and Electron Applications

- **Cause:** The kernel skips GMEM range setup for one GPU generation but returns an incorrect base address.
- **Confirmation:** Reduce the real application's calls to a minimal native GLES reproducer; compare with a driver debug switch (for example, FD_MESA_DEBUG=sysmem); use LD_PRELOAD to intercept parameter queries and vary one factor at a time.
- **Resolution:** Apply the one-line kernel fix (already available upstream), then remove the application-level workaround parameter.
- **Applicability:** Adreno 7xx; 2026.

### Vulkan Hangs on a New GPU

- **Symptoms:** Running vkcube triggers hangcheck detected gpu lockup.
- **Resolution:** Do not enable Vulkan applications by default in the first release. GTK can use the GL renderer initially.
- **Applicability:** Newer Turnip generations.

### A Test "Passes" Under QEMU or Software Rendering

- **Cause:** Mesa automatically falls back to softpipe or llvmpipe, so pixel checks can still pass.
- **Resolution:** Assert that the renderer name identifies the hardware GPU, and include a negative test that requires hardware GPU rendering.
- **Applicability:** General.

### The Frame Test Cannot Capture an Image

- **Cause:** The GDM login screen does not produce application frames; a linear frame-capture tool rejects Qualcomm's compressed format.
- **Resolution:** Run frame tests with headless GNOME (gnome-shell --headless --virtual-monitor). Do not mistake the cursor plane for the primary image.
- **Applicability:** GNOME; 2026.

## Unresolved

- Vulkan stability on new GPUs, sustained-load behaviour, frequency scaling and power management.
