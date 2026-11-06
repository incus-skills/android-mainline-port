# Problem Patterns: First Identify the Category of a Fault

Most pitfalls in the domain files can be abstracted into the following categories. When a new problem occurs, first compare it with "How to recognise it", then investigate using "General handling", and finally consult the corresponding file for specific entries. A symptom may belong to more than one category.

| Category | In one sentence |
|---|---|
| A | The bootloader changed something you thought had not changed |
| B | A resource has no actual owner |
| C | Timing or lifecycle is wrong |
| D | Residual state distorts the result |
| E | The measurement method itself is wrong |
| F | Passing at a lower layer does not mean the user can use it |
| G | Upstream or the distribution has already fixed it |
| H | Another implementation cannot be copied wholesale |
| I | The recovery path itself is unreliable |

---

## A. The bootloader changed something you thought had not changed

**How to recognise it**: With the same kernel and device tree, `fastboot boot` returns OKAY but nothing happens; code matching by `compatible` stops working entirely; command-line changes have no effect; systemd identifies the wrong device model; slot behaviour differs from expectations.

**General handling**:

1. Capture the state as emitted by the bootloader: the stock system's `/sys/firmware/fdt`, the new system's `/proc/device-tree`, `/proc/cmdline`, `/proc/bootconfig`, `fastboot getvar all`, and failure-reason queries.
2. Compare these item by item with the inputs you packaged. The differences show what the bootloader did.
3. Reproduce the behaviour offline using the implementation actually used by the bootloader; first run a positive control using the stock system.
4. Prefer fixing this at packaging time (injecting symbols, validating identity), or restore the original value early in the kernel. Do not use any property that may be rewritten as a basis for identifying the device.

**Common manifestations**: Silent rejection due to a missing symbol table; device-tree identity mismatch stopping at Fastboot; root `compatible` being rewritten; a hypervisor node causing a false virtual-machine identification; command line sourced from the device tree and subject to a length limit; serial number present only in bootconfig; slot state involving more than a property bit. See `bootloader-and-boot.md`, `usb-and-storage.md`.

## B. A resource has no actual owner

**How to recognise it**: A feature disappears seconds to tens of seconds after boot; several apparently unrelated subsystems time out together (display, GPU, touch); some boots work while others are globally slow; a peripheral enumerates but its bus has no power; adding a power rail makes the entire PMIC disappear; `sync_state` remains pending and the SoC runs at its highest voltage.

**General handling**:

1. Export `clk_summary`, `pm_genpd_summary`, `interconnect_summary` and `regulator_summary` once before and once after resource reclamation. Check what gets turned off after reclamation.
2. Compare the results against the stock system's always-on power, clock and bandwidth votes; identify the differences.
3. Put the requirement in the actual consumer node, and ensure the provider driver is built in and binds before the reclamation point.
4. Mechanically check voltage values against the driver's voltage table, using the actual values from stock registers.
5. Do not mask the problem with global ignores, module blacklisting, or timeout-based release.

**Common manifestations**: Borrowed firmware display being reclaimed; display clock controller being a module and causing a slow boot state; a fake consumer preventing `sync_state` from ever completing; a missing always-on power rail leaving the keyboard bus low; a voltage value not aligned to PMIC steps. See `kernel-and-resources.md`, `display.md`.

## C. Timing or lifecycle is wrong

**How to recognise it**: The fault is intermittent; it depends on refresh rate, screen-off state, suspend, hot-plugging, or whether a peripheral is connected at boot; manually restarting a service restores operation; "works when run manually, fails as a service"; adding a delay appears to help.

**General handling**:

1. Record the timing of related events together (panel callbacks, display kickoff, first interrupt, service startup, device appearance).
2. Compare against the stock driver's lifecycle: what happens, and in what order, during screen off, screen on, suspend and wake.
3. Do not perform blocking waits in callbacks. Use event-driven dependencies (wait for the device to appear) instead of fixed delays.
4. Compare the execution environments of the manual and service paths (standard input/output, permissions, namespaces, `systemctl show`).
5. Do not treat periodic keep-alive, automatic reset, fixed delays, or automatic service restarts as fixes.

**Common manifestations**: TDDI touch firmware being lost after screen-off; synchronously waiting for the first frame inside a panel callback; touch commands sent too early; keyboard power state not written as the stock implementation does; charging-control handoff during suspend; PCIe link retained while its power domain is turned off; socket bound to a network interface that does not yet exist at boot; oneshot service running repeatedly; `btmgmt` receiving `/dev/null` as standard input; Fastboot not responding while a background zero-write is in progress. See `touch-and-input.md`, `power-and-time.md`, `wireless.md`, `ubuntu-userspace.md`.

## D. Residual state distorts the result

**How to recognise it**: It works on the development machine but fails after a clean installation; behaviour differs after reinstalling the same version; a fix "suddenly stops working"; old data appears in the new system.

**General handling**:

1. List every possible source of residual state: saved ALSA state, user configuration, previously installed packages and kernels, old sets of boot images, old partition data, old `OUT` directories, old local-data copies on the device, and moved tags.
2. Reproduce on a clean system, with a fresh boot and empty directories.
3. Accept the fix only under conditions where no residual state is present.

**Common manifestations**: Only Dummy Output after a clean installation; sparse skipping blocks that retain old data and logs; ccache becoming invalid after changing paths; sensor configuration needing to be assessed after a fresh boot; importing an outdated local-data copy; package version lower than an internal version on the test machine. See `audio.md`, `usb-and-storage.md`, `build-and-release.md`, `sensors-camera-media.md`.

## E. The measurement method itself is wrong

**How to recognise it**: The conclusion comes from only one data point; the clock, scenario or environment on which the criterion depends has not been verified; offline tests are all green but the real device fails; something "gets fixed" and then immediately fails again.

**General handling**:

1. First establish whether the measurement is trustworthy: which clock, which scenario (lighting, load), and which environment (QEMU or real device, switching while running or cold boot, presence or absence of a session environment).
2. Before reporting a root cause, try to disprove it using a second data point or a control group.
3. Prepare an input that should fail for every check, and confirm that it actually reports failure.
4. After fixing the tool, reassess the saved evidence. Do not rerun the device or relax the criteria.

**Common manifestations**: Using a monotonic clock to calculate suspend duration; reading only the first segment of s2idle; an incorrect explanation of "the image is too large"; a dark scene mistaken for a display defect; Mesa falling back to software rendering in QEMU; using a runtime switch as a substitute for cold boot; automatic brightness changing backlight readings; newline and address problems in host scripts. See `testing-and-process.md`, `power-and-time.md`, `gpu.md`.

## F. Passing at a lower layer does not mean the user can use it

**How to recognise it**: The driver is loaded, a device node appears, a service is active, or a script retrieves data, but the user says "it doesn't work".

**General handling**:

1. Write out the complete path from hardware to the user-facing entry point (for example, camera: sensor → driver → daemon → PipeWire → application).
2. Verify each layer. The final layer must be the entry point the user actually uses.
3. Close a user-reported problem only after verification succeeds at the user-facing entry point.

**Common manifestations**: Frame capture succeeds but the camera is absent from applications; `hci0 up` but pairing does not work; an image exists in the shadow buffer but the screen is black; hotspots are found but cannot be connected to; the app store opens but all Chinese text appears as square glyphs. See `testing-and-process.md`, `sensors-camera-media.md`, `wireless.md`.

## G. Upstream or the distribution has already fixed it

**How to recognise it**: The problem is in a distribution-provided component or upstream driver; a related commit exists in a newer upstream or community version; you are using an older pinned version.

**General handling**:

1. First check upstream `main`, `stable`, newer distribution versions and issue lists.
2. Update directly if possible (for example, `snap refresh`). Otherwise backport the commit, record the technical debt, and switch back after the upstream release.
3. If upstream has not fixed it, make the smallest correction and prepare to submit it upstream.

**Common manifestations**: Square glyphs for Chinese in the app store (fixed in a newer snap); Turnip NV12 stride (fixed upstream in stable); frequency calculation and scanning during recovery (community FROMLIST); device-model reference leak. See `ubuntu-userspace.md`, `gpu.md`, `wireless.md`.

## H. Another implementation cannot be copied wholesale

**How to recognise it**: You followed a reference device or community implementation, but behaviour differs on the local device; the community says a feature is "supported", but it does not work locally.

**General handling**:

1. Separate what can be carried over (driver code, protocols, userspace) from what cannot (voltages, GPIOs, timing, board data, calibration, partition numbers, keep-alive policies).
2. Compare the foundations first (device-tree approach, resource management, init, installation method), then decide whether to change the baseline or import features individually.
3. For everything that cannot be carried over, return to evidence from the local stock device.

**Common manifestations**: Polarity and transmit/receive pins reversed in a reference DTSI; board data from another device; keep-alive policy for a different keyboard protocol; a feature made to work by the community through a global keep-alive parameter. See `kernel-and-resources.md`, `touch-and-input.md`, `wireless.md`, `testing-and-process.md`.

## I. The recovery path itself is unreliable

**How to recognise it**: The device does not roll back when the timer expires; normal reboot hangs during shutdown; the device becomes unreachable and can only be recovered by someone pressing a button.

**General handling**:

1. Put a time limit on every step in the recovery path (`sync` with a timeout, `reboot -f -n`), and test the final combination in practice.
2. When unattended, do not unload or rebind real hardware drivers. Before rebooting, check for tasks in D state; if present, use SysRq.
3. Test the watchdog with a controlled failure-to-feed test, and confirm that it really resets the device.
4. Always retain an accessible debugging channel in product images.

**Common manifestations**: Unbounded `sync` in an expiry path; shutdown hanging after a video-driver unload deadlock; GPU hanging after a candidate kernel writes directly to the slot; ordinary reboot hanging during shutdown after the rollback slot has already been overwritten by the same candidate; a production image with no debugging channel; systemd trying to open the hypervisor watchdog. See `bootloader-and-boot.md`, `sensors-camera-media.md`, `testing-and-process.md`.
