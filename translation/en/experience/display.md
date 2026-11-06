# Display

## Recommended Route

There are two ways to light the screen. Understand the difference before choosing:

| | Borrow the firmware display (simple-framebuffer + simpledrm) | Native takeover (DPU + DSI + panel driver) |
|---|---|---|
| Principle | The bootloader has already lit the panel and is continuously scanning out a memory buffer through the display controller. Linux only draws into that buffer. | The kernel display driver reinitializes the controller and interface, and the panel driver sends the power-on commands again. |
| First image | Fast (one or two days) | A few days if an existing panel driver is available; one or two weeks if a panel driver must be written. |
| Capabilities | No brightness control, genuine display-off, refresh-rate switching, external display, or suspend support; GNOME sees the connector type as Unknown. | All of these capabilities are available. |
| Main risk | Resource reclamation (see `kernel-and-resources.md`). | Incorrect parameters causing corrupted output; display-off/display-on lifecycle. |

**Recommendation:** If the upstream display driver supports the SoC and a panel driver can be obtained from community or vendor code, go directly to native takeover. Otherwise, first borrow the display to establish the installation path and desktop, while **starting native takeover in parallel**. Do not attempt product-level fixes such as brightness control or display-off on top of the borrowed display.

### Native Takeover Steps

1. On the vendor system, use the vendor driver's debugfs to save a "golden snapshot": clock tree, pipeline topology (mixer, DSC, number of interfaces), DSC parameters, PHY timings and panel power-on commands. Do not scan display registers through `/dev/mem`.
2. For the panel driver, prefer an existing community or upstream implementation. Mechanically extract the power-on commands from the vendor dtsi and check them against the original device tree from this device, item by item.
3. Initially load the display driver as a module (build `msm` as `=m`, do not load it at boot, and manually run `modprobe` once the debugging channel is available). This makes it possible to obtain logs before a hang.
4. Add the display subsystem to the SMMU identity-mapping list so that the firmware display survives until the moment the driver resets the hardware. There are already upstream precedents for some SoCs.
5. Complete takeover at the firmware's current refresh rate first; handle other refresh rates afterwards.
6. Only after acceptance, implement display-off, display-on, backlight association and DPMS.

## Completion Criteria

- A person can see the correct image. This is mandatory for the first successful display.
- Hardware indicators:
  - Display-interface frame counters increase at the expected refresh rate.
  - No underruns or DSI errors.
  - For native takeover: the topology in debugfs matches the golden snapshot, CRTC CRC is stable, and panel status-register readback is normal (for example, DCS `0x0A`).
  - For borrowed display: sampling the framebuffer region shows multiple colours, and every hop of the display-bus interconnect has a vote.
- Multiple cold boots meet the time budget. After multiple display-off/display-on cycles, both the image and touch input recover.
- The following do **not** count as passing: a process is running, `/dev/fb0` can be written, an image exists in framebuffer memory, or the connector reports `connected`.

## Pitfalls

### Treating the Borrowed Firmware Display as the Product Route

Tags: **[Frequent] [Time-consuming]**

- **Symptoms:** The desktop works, but there is no brightness slider; display-off merely produces a black image while the backlight remains on; settings show a 60 Hz refresh rate; suspend and external display have no usable foundation. Several more days are then spent dealing with resource reclamation just to keep the borrowed display alive.
- **Cause:** simpledrm owns only a memory buffer, not the hardware. GNOME does not provide internal-display functionality for a connector reported as Unknown.
- **Resolution:** Use the borrowed display only for first light and to establish the installation path. Start native takeover in parallel as early as possible.
- **Applicability:** General; 2026.

### All Software States Look Normal, but the Screen Is Black and Is Mistaken for "Lit"

Tags: **[Frequent] [Time-consuming]**

- **Symptoms:** The connector, fb0, fbcon and backlight registers all look normal, but the user sees a completely black screen. This can even lead to the incorrect conclusion that the panel is faulty.
- **Common overlapping causes:** The backlight chip's global-enable register was never written (mainline treats it as a read-only ID); a DSC video-mode panel loses lock because a non-continuous clock is used; the TE pin is not connected and vblank times out; DSC parameters do not match the panel decoder; interface active-width rounding differs; or a panel supply voltage does not fall on a PMIC step, causing the entire PMIC to disappear.
- **Confirmation:** For a completely black screen, check the backlight first (it does not depend on the panel). Compare the vendor display-driver source item by item before proceeding to black-box binary search.
- **Resolution:** Correct each item to match the vendor implementation.
- **Prevention:** "Lit" is established only by human vision or panel-side hardware indicators.
- **Applicability:** General; 2026.

### Inconsistent DSC Parameters: Stripes or Corruption, Even Though Colour Bars Pass

Tags: **[Frequent] [Time-consuming]**

- **Symptoms:** Horizontal stripes or block-shaped corruption appear. A solid-colour test pattern may look normal.
- **Cause:** DSC rate-control (RC) parameter-table version (1.1 versus 1.2), `first_line_bpg_offset`, `initial_lines`, slice-width rounding, or `slice_per_pkt` does not match the panel decoder. The mainline driver may overwrite the RC values supplied by the panel driver with its own defaults.
- **Confirmation:** Use a stress pattern that crosses slice boundaries (colour bars can pass even when RC is wrong). Compare every derived value with the panel's PPS data.
- **Resolution:** Set the actual panel/vendor values and confirm that the driver does not overwrite them.
- **Applicability:** Qualcomm DPU + DSC panels; 2026.

### Incorrect Pipeline Topology: Zero Errors but the Entire Screen Is Corrupted

Tag: **[Time-consuming]**

- **Symptoms:** All components are active, PPS has been sent and there are no errors, but the image is corrupted. Repeated single-variable parameter tuning has no effect.
- **Cause:** The selected topology uses four mixers, but without virtual planes only two can be fed. The other half contains garbage data, which the compressor encodes normally.
- **Confirmation:** First inspect `mixer=`, `dsc=` and `stage=` in `/sys/kernel/debug/dri/0/state`, and compare them with the vendor topology.
- **Resolution:** Do not select a four-mixer topology when virtual planes are unavailable.
- **Prevention:** When the topology is wrong, other correct fixes (such as `slice_per_pkt`) can also appear ineffective. Before declaring a change ineffective, first verify that the other dimensions are correct.
- **Applicability:** Qualcomm DPU; 2026.

### Mode Validation Rejects High Refresh Rates, or Reports Zero Available Modes

- **Symptoms:** `MODE_CLOCK_HIGH`; the connector has no modes; or a refresh rate (for example, 144 Hz) cannot be selected even though a community version can use it.
- **Cause:** Mode validation uses the bootloader's leftover clock as its upper limit; or the display-core clock calculated from the resolution is slightly above the SoC limit (for example, 577 MHz is required while the limit is 575 MHz). A community implementation may simply remove this check.
- **Resolution:** Use the highest frequency the driver can actually reach, rather than the leftover value. For high refresh rates, first complete takeover at the firmware's default refresh rate, then evaluate a correction based on the actual bandwidth after DSC compression. Do not raise the clock beyond the OPP table just to pass validation; this causes `-ERANGE` and loss of power votes.
- **Applicability:** Qualcomm DPU; 2026.

### Enabling Only the Display-Subsystem Node, with No Components, Causes a Display-Driver NULL Pointer

- **Symptoms:** Enabling the MDSS/DPU node causes an early-boot hang, with no USB.
- **Cause:** When the DPU has no components, it passes an empty list to the component framework, causing a NULL pointer dereference.
- **Resolution:** Return `-ENODEV` when there are no components (a one-line upstream-level fix). Build the display driver as a module initially and load it manually to capture the oops.
- **Applicability:** Mainline msm driver; 2026.

### Enabling the Display-Subsystem Node Immediately Corrupts the Firmware Display

- **Symptoms:** The image is corrupted before the driver has reset the hardware.
- **Cause:** Early in boot, the SMMU connects the display stream to a new address domain, so the memory being scanned by the firmware begins to fault.
- **Resolution:** Add the SoC's display-subsystem compatible to the SMMU identity-domain list.
- **Applicability:** Qualcomm; 2026.

### Display Fails to Turn Back On After Being Turned Off

Tags: **[Frequent] [Time-consuming]**

- **Symptoms:** After display-on, DSI command DMA reports `-110`, `CTL_FLUSH` hangs, repeated vblank timeouts cause corruption, or `dsi_err_worker status=4` appears. Testing on a static login screen may nevertheless appear to "pass".
- **Cause (one confirmed case):** The VBIF AXI halt handshake is missing before the display power domain is powered off; CTL soft reset never completes and requires an override; during recovery, clocks are enabled before votes are restored.
- **Confirmation:** Compare the power-event hooks against the vendor driver one by one. Start every controlled comparison from a clean boot. An "exclusion" experiment in a boot that has already hung is meaningless.
- **Resolution:** Restore the vendor ordering for the handshake and reset. During recovery, restore votes before enabling clocks.
- **Prevention:** Display acceptance must inject new frames (move the mouse or open a window). Any commit or vblank timeout is a failure.
- **Applicability:** Qualcomm DPU; 2026.

### The Screen Briefly Goes Black During Takeover

- **Symptoms:** The image disappears for about one second when the native driver loads.
- **Cause:** Upstream does not provide seamless takeover. The driver removes simpledrm through aperture removal and then resets the hardware.
- **Resolution:** This is expected behaviour; document it.
- **Applicability:** Mainline; 2026.

### GNOME Test Cleanup Code Moves the Login Area Off-Screen

- **Symptoms:** Software checks are all green, but the user sees a black screen or no login box.
- **Cause:** The test script calls `org.gnome.ScreenSaver.SetActive(false)`. When gnome-shell hides the lock screen, it shifts the login area off-screen.
- **Resolution:** Do not call `SetActive(false)`. Wake the display with a single keypress that contains no text. Before calling a GNOME D-Bus interface, read the gnome-shell source to confirm its semantics.
- **Applicability:** GNOME; 2026.

### The System Restores the Previous Shutdown Brightness, so the Screen Is Dim at Boot

- **Cause:** systemd-backlight saves and restores a very low brightness value.
- **Resolution:** Use a one-shot service to write the default brightness before the display manager starts. Do not use a dconf lock.
- **Applicability:** systemd systems.

### Multiple Backlight Chips

- **Approach:** Represent each chip as an LED subdevice, aggregate them into one backlight device with `led-backlight`, and associate that device with the panel node. Only then will the desktop provide a brightness slider.
- **Applicability:** General.

### Corruption or Heavy Underruns When Turning the Display Off

Tag: **[Time-consuming]**

- **Symptoms:** The image becomes corrupted and the underrun counter rises sharply at the moment DPMS turns the display off; it may work normally after the display is turned back on.
- **Cause:** In a dual-INTF configuration, CTL is cleaned up before the interface is stopped; the shutdown order differs from the vendor implementation; or the DSI PHY state is not saved and restored.
- **Resolution:** Follow the vendor and community-corrected order: stop INTF before clearing CTL. Import the DPU shutdown-order and PHY save/restore patches. For acceptance, run dozens of DPMS cycles and count underrun events.
- **Applicability:** Qualcomm DPU with dual DSI; 2026.

### frame-done or Pending Timeout Warnings, but the Image Is Completely Normal

- **Cause:** Upstream `dpu_crtc_commit_kickoff()` sets the timer only after encoder kickoff. The first vsync after a restart may clear the state first, producing a harmless accounting warning.
- **Resolution:** Do not treat this as a display failure. If needed, correct the ordering to match upstream while retaining detection of genuine timeouts.
- **Applicability:** Mainline msm; 2026.

### Night Light (Colour-Temperature Adjustment)

- **Approach:** Qualcomm requires DSPP GC/GCv2 and REGDMA support. A community implementation already exists; import it with its provenance. GNOME 50 Night Light does not take the defective path involving Mutter's empty LUT, so the distribution's Mutter does not need to be modified.
- **Risk:** If REGDMA reset is not acknowledged, the original implementation waits forever. Add a timeout to the exit path, and on timeout retain mappings still in use by DMA rather than releasing them.
- **Applicability:** Qualcomm DPU; 2026.

### High Refresh Rates Ultimately Work

- **Additional evidence:** After the community corrected mode validation, dual-INTF 144 Hz ran stably on the device. Switching among 60/90/120/144 Hz and CRC validation both passed. High refresh rates must be tested together with touch input (see `touch-and-input.md`).
- **Applicability:** Qualcomm DPU + DSC panels; 2026.

## Unresolved

- `dsi_err_worker status=4` after display-off/display-on and suspend/resume: the community implementation for the same SoC has not solved it either.
- Some DSC panels need a more accurate display-core clock calculation for high refresh rates.
