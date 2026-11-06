# How to Judge Whether Something “Passes”

## Status Terms

| Status | Meaning | Required record |
|---|---|---|
| Awaiting investigation | Not yet investigated | — |
| Source lead available | Code exists upstream or in the community, but has not been built | Link |
| Offline pass | Compilation, QEMU, unit tests and script self-checks pass. This shows only that the code logic conforms to the design; it does not show that the hardware works | Command, input hash, output |
| Boots on real device | The image has started running on the device | Image hash, boot ID, logs |
| Passes on real device | The function has been verified on real hardware with a real result | Real result + hardware metrics + scenario + evidence location |
| Cold-boot pass | Also passes after a cold boot and a clean installation, without relying on state left over from before | Cold-boot logs |
| Publishable | All of the above, and the file is the exact one intended for release | Release-file hash |

When a person is not present, write only: “Machine metrics pass; awaiting visual confirmation.”

## These Do Not Count as a Functional Pass

A driver loading successfully; a sysfs node existing; a service being active; a D-Bus property changing; a sound card or PCM appearing; a remote processor showing as running; `hci0 up`; `/dev/fb0` accepting writes; an image existing in video memory; Fastboot returning OKAY; QEMU passing; injected input events; simulated test data; or the tested code and test data “agreeing” because they share the same incorrect assumption.

## Evidence by Subsystem

| Subsystem | Hardware metrics | Real result |
|---|---|---|
| Boot | USB disconnects within approximately 1 second, timestamps, automatic reboot at the scheduled time | — |
| Display | Frame count increases at the refresh rate, no underflows, topology matches the stock implementation or video-memory sampling contains multiple colours | A person sees the correct image |
| Touch | Firmware state, interrupt count, input events | Finger taps, swipes, multi-touch and all four corners |
| GPU | Renderer is the hardware GPU, pixel-by-pixel correctness, no GPU hangs | Complex application renders correctly |
| Wi-Fi | Board-matching logs, actual frequency, DHCP, ping, throughput | — |
| Bluetooth | Works after cold boot, correct address, can scan | Paired device actually works |
| Audio | Playback pointer advances, register readback, tuning-file version, loopback | Heard by a person; microphone tested with an external sound source and a silent control |
| Charging | Battery-level and voltage trends over 15–30 minutes, current | — |
| Rotation | Raw data, service output, compositor state | Actual device orientation and screenshot |
| Keys and screen lock | Input events, display power state, lock state | Human observation |
| Suspend | Successful-suspend count, clock changes, boot ID unchanged | — |

## Test Methods

- **Cold boot and control.** If a service, socket, udev rule, initramfs, or boot parameter changes, test with a cold boot and a control (remove the change and confirm that the problem returns, or disable the feature and confirm that it becomes unavailable). Toggling a setting while the system is running is not a substitute for a cold boot.
- **Clean installation.** No saved ALSA state, no services manually restarted, and no settings left over from a previous boot.
- **Positive control.** Known-correct input must also produce the correct result in your tool. Otherwise, fix the tool first.
- **Debugging code is also a variable.** If adding debug code makes the problem disappear, that means the debug code changed behaviour; add the same code to the control group as well.
- **Use real-device formats for test data.** Preserve at least one sample of raw output from the real device (line endings, real addresses and actual timing) as test data.
- **List differences between the test environment and the real device.** Assess each difference individually for whether it could invalidate the conclusion.
- **Stop immediately for safety problems; continue collecting evidence for other functional failures.** Report them together at the end, so that one failure does not waste the entire test window.

## Test Automation

Goal: every on-device test should be performed by scripts; the user should do only what must be done by a person.

**On-device test orchestration script**

- One command completes the sequence: check device identity, battery and debugging channel → start the candidate image → wait → collect → judge → restore the original system.
- Every step has a timeout. Output machine-readable results (for example, JSON), including version information, each metric, pass/fail result and evidence path.
- If a function fails, continue collecting evidence for the other functions; stop immediately only for safety-related failures.
- Before the user arrives, run the complete host-side checks (USB interface, address, Fastboot identity, line endings and command length). Problems in the host script must not waste the opportunity to test on the real device.

**Device-side self-checks**

- Write one self-check command for each function. It should output a conclusion and raw data, run automatically after boot, and send the result back to the host.
- Self-checks should read hardware metrics directly (counters, registers and specific log lines), not merely inspect service status.

**Automatic recovery**

- Every test image should include a timeout-triggered automatic reboot. Before a high-risk operation, first set a rollback timer on the device (for example, before switching USB roles).
- Automatic recovery itself must first be verified while the user is present before it is used for unattended testing.

**Ways to Use Machines Instead of People**

| What must be confirmed | Automation method | What still requires a person |
|---|---|---|
| Display image | Display-interface frame count, underflow flags, CRC, video-memory sampling, compositor screenshot compared with expected pixels | First light-up, colour and flicker |
| Touch and keys | Inject events with uinput to verify the software path; record coordinates and timing of real events | Real finger and real key (at least once) |
| Sound | Loopback recording to detect test-tone frequency, amplifier electrical feedback, playback pointer and error count | First audible output and whether the volume is appropriate |
| Microphone | Device plays a test sound and records it; compare sound-present and silent controls | — |
| Network | Automatically connect to a test hotspot; test DHCP, ping, throughput and reconnection | — |
| Bluetooth | Automatic scanning; another host's script can perform pairing | Headphones or keyboard/mouse actually work (at least once) |
| Applications | Launch common applications one by one; use AT-SPI to find windows, take screenshots, and detect crashes or blank windows | Final overall visual inspection |

**Specific desktop-automation method (GNOME):** Operate through the debugging channel. Take screenshots through the desktop portal (`xdg-desktop-portal`) Screenshot interface, with permission granted in advance to avoid a prompt. Use Mutter's RemoteDesktop virtual input for clicks, swipes and typing; there is no need to use uinput or open a second Shell. When the screen is locked, screenshots are entirely black, so unlock it before testing (a test device may have automatic locking disabled; this must not go into the product). If there is no window-listing interface, use screenshots to judge. Frame-rate and rendering tests can use headless GNOME (`gnome-shell --headless --virtual-monitor`).

| What must be confirmed | Automation method | What still requires a person |
|---|---|---|
| Charging | Record battery level, voltage and current at intervals; calculate trends automatically | Physical charger plug/unplug |
| Rotation | Automatically record raw data and compositor state | Physically place the device in all four orientations |
| Boot performance | Multiple cold boots; automatically check time budgets | — |

Note: injected events and screenshots prove only the software path. They cannot replace the first acceptance of a real finger or a real displayed image.

**Cold-boot acceptance tool**

- Turn cold-boot criteria into a script: boot-time stages, display and GPU hardware metrics, failed services, whether modules came from the current kernel, and memory bandwidth. Run it after every kernel or boot-path update.
- Wait until the system is fully up before collecting data (for example, 45 seconds after boot). Collecting too early can falsely report “still booting” as a failure.

**Regression**

- After every build, automatically run a cold-boot regression set: boot-time budgets, each function's self-check, and items that passed previously. Compare the results with the previous version and flag regressions immediately.
- Host-side unit tests and QEMU tests verify logic; test data should come from raw output captured from a real device.

**Test the Tests**

- Prepare an input that should fail for every check, and confirm that it really reports failure.
- Empty output, timeout and command-not-found all count as failures.
- Simulated test data must not share the same assumption as the code being tested.

**User Participation**

- Retain only tasks requiring human eyes, ears, fingers, physical placement or plugging/unplugging.
- Combine user tasks into one checklist. Tell the user in advance how much time is needed and what to look at. Give each item a clear observation point and response format (for example, “yes/no”).
- Record the user's answers in the experiment record, separately from machine metrics.
- Do not repeatedly ask the user to perform gestures or key presses that have already been confirmed.

## Boot-Time and Performance Budgets

A single check cannot reveal an occasional slow boot; examine multiple cold boots. Define a set of budgets for each device and check them on every update. For example:

- The borrowed-display driver takes over within 1 second after boot; the related clocks remain occupied continuously from initramfs onward.
- Switching to the real root filesystem, starting the display manager and loading GPU firmware each have an upper time limit (for example, 10 seconds, 20 seconds and 20 seconds).
- The GPU has no errors during the first few minutes after boot.
- A simple memory-copy test reaches the expected bandwidth (to detect a memory path running at reduced speed).
- Refresh rate and desktop colours are correct.
- Time taken for an ordinary reboot, Linux-to-Android switch and Android-to-Linux switch.

## Experiment Record

Before each experiment, write four sentences:

1. What exactly is being changed.
2. If the result is A, what has been ruled out.
3. If the result is B, what has been ruled out.
4. Which machine metric determines whether it passes.

Before judging something “ineffective”, first explain why the other conditions are known to be good. If that cannot be explained, write: “No improvement observed under the current unknown conditions.”

## First-Release Standard

Someone else must be able to build, install and use the result by following the documentation. Not every peripheral has to be complete, and not everything has to be submitted upstream.
