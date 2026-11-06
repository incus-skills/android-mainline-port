# Testing Judgement, Automation, Collaboration and Process

## Pitfalls

### Treating "it runs" as "it works"
**Marker:** [Frequent]

- **Symptom:** Display is marked "passed" because the shadow buffer contains an image; Bluetooth is marked "passed" because `hci0` is up, although no scan has been performed; an active service, successful probe or QEMU pass is recorded as functional success.
- **Resolution:** A function passes only when there is a user-perceptible result (visible, audible, tactile or connected) or an equivalent hardware measurement. Record this separately from visual confirmation by a person.
- **Applicability:** General.

### Treating all-green offline checks as "close to delivery"
**Markers:** [Frequent] [Time-consuming]

- **Symptom:** Dozens of offline checks, repeated-build comparisons and QEMU tests all pass, so a candidate is called "final"; after installation on the device, every feature newly added in that version fails.
- **Cause:** Time was spent on checks that easily produce pass records rather than on real-device evidence most likely to disprove the approach. Simulated data used invented addresses and line endings, or the simulated data and tested code shared the same incorrect assumption.
- **Resolution:** Label evidence that exists only offline as "offline passed". Test each new feature individually on a real device using a separate RAM image, and merge it only after it passes. Preserve at least one copy of the original data captured from the real device.
- **Applicability:** General.

### Using runtime switching tests in place of cold-boot tests
**Markers:** [Frequent] [Critical]

- **Symptom:** Switching a debug channel or changing a socket works while the system is running, but the release image stalls for 90 seconds on cold boot or the channel never starts.
- **Resolution:** After changing a unit, socket, udev rule, initramfs, generator or boot parameter, cold-boot an image in which the change is actually present. First reproduce the problem with the old version as a control. Before release, list every package that has changed the boot path since the last real-device verification; each needs a cold-boot record.
- **Applicability:** General.

### Wasting real-device opportunities on host-side script problems
**Marker:** [Frequent]

- **Common forms:** Hard-coding the host IP when DHCP assigns another address; simulating only LF line endings when the device uses CRLF; assuming USB disconnection guarantees a TCP FIN, then treating a 90-second absence of EOF on the old connection as proof that the device is still online; an echoed shell command containing the end marker truncating output early; `fastboot boot | tee` returning tee's exit status; a read-back command losing stderr and producing the hash of empty input (`e3b0c442…`); BusyBox line editing limited to 1023 bytes; a prompt followed by `\x1b[6n`; `nmcli -t` emitting colour codes under a PTY; host fastboot hanging after device re-enumeration (releasing and rebinding the USB port can recover it); polling immediately after Fastboot handoff colliding with the window in which ABL removes USB and sending an unintended command.
- **Resolution:** Before the user is present, run the complete host-side check against the real device: USB interface, address, route, Fastboot identity, line endings and command length. Delimit each command's output with explicit start and end markers. Treat a hash of empty input as failure; do not treat EOF as success. When the same class of host error occurs a second time, inspect every entry point for it.
- **Applicability:** General.

### Defects in the acceptance criteria, test data or test tools themselves

- **Symptom:** An orchestrator exception is recorded as a pass; a test wrapper reports failure as success; a probe kills its own workload and is then marked failed; a thermal-zone prefix table omits entries; or acceptance collection starts too early after boot (for example, within 45 seconds) and reports "still booting" as failure.
- **Resolution:** The earliest failing layer may be the tool itself. Treat every orchestrator exception as failure and preserve its stack trace. Give every check an input that should fail. After fixing a tool, re-evaluate the evidence already saved offline; do not rerun the device or relax the criteria.
- **Applicability:** General.

### The user's test window is too short, or notice is given too late

- **Symptom:** A 30-second button-test window expires before the user knows about it, and the result is recorded as a failure.
- **Resolution:** When user action is required, notify the user at least one minute beforehand and explain what to do and what to observe. Where possible, replace a short window with a long-running recorder (for example, several hours of evdev recording). Record the user's report of the physical result directly and separately from machine evidence.
- **Applicability:** General.

### Asking the user to do something that could be automated

- **Symptom:** The user is repeatedly asked to take screenshots, open applications or click controls.
- **Resolution:** Automate desktop tests through the debug channel. Capture screenshots through the desktop portal (`xdg-desktop-portal`) Screenshot interface and pre-authorise it to avoid prompts. Inject input using Mutter RemoteDesktop virtual input rather than uinput. A locked screen produces an entirely black screenshot, so unlock it before testing (automatic locking may be disabled on a test machine, but not in the product). If newer GNOME versions have no window-listing interface, use screenshots to judge the result. Injected events prove only the software path; confirm at least once with a real finger.
- **Applicability:** GNOME; 2026.

### Two tasks operating on the same device at the same time

- **Symptom:** Another task reboots the device and contaminates an experiment; a reported "USB reconnection succeeded" was actually caused by another task's reboot.
- **Resolution:** There must be only one operator at a time. Before any device command, check whether another process is using the device. Bind every evidence record to a boot ID. Host logs without a boot ID cannot be used for attribution.
- **Applicability:** General.

### Fixing the terminal symptom again when the same failure occurs a second time
**Markers:** [Frequent] [Time-consuming]

- **Symptom:** The same class of failure recurs, and each time another fix is added at the final layer (for example, fixing a protocol-layer timeout that is actually caused by a power problem). The source describes this as the most common source of large time losses.
- **Resolution:** Stop and trace upward from the physical layer, finding the earliest point of failure: power and clocks, bus voltage levels, protocol, driver, userspace ownership, output, and the observation method itself. Record the hypotheses already excluded and the next experiment capable of falsifying one.
- **Applicability:** General.

### Running an "exclusion" experiment in a boot that is already hung

- **Symptom:** Multiple control experiments all appear to have "no effect", and an incorrect conclusion is written into a code comment or document.
- **Resolution:** Start every exclusion experiment from a clean new boot and record the elapsed time since boot.
- **Applicability:** General.

### Deciding too early that "this change made no difference"

- **Cause:** Another error is present at the same time, making a correct fix appear ineffective.
- **Resolution:** Before declaring a change ineffective, explain why all other conditions are known to be correct. If that cannot be explained, write: "No improvement was observed under the current unknown conditions."
- **Applicability:** General.

### Conclusions are lost during context compression or a handoff, and the same mistake is reintroduced

- **Resolution:** Put rejected approaches and reverted changes in a NO-GO record, and check it before testing on the device.
- **Applicability:** General.

### A sub-agent goes in circles, over-designs, or leaves unverified changes in the tree

- **Symptom:** An open-ended task runs for two hours and produces code that cannot be verified; a task to "cache the distribution layer" produces hundreds of lines of cache code even though caching was not the bottleneck.
- **Resolution:** The task brief should specify one approach, the files allowed to change, one verification command and pass criterion, and a stopping condition. Check direction shortly after work begins and stop immediately if it has drifted. Treat a sub-agent's claim that something was "tested" as unverified unless the test method matches the real method of use.
- **Applicability:** General.

### Asking questions instead of measuring

- **Symptom:** Waiting several hours for the user to answer a question that could have been answered directly through read-only remote diagnostics.
- **Resolution:** Measure it yourself first when possible.
- **Applicability:** General.

### Designing without first examining comparable projects

- **Symptom:** A self-designed process (for example, an installation method) encounters a problem already solved by a comparable project; a pitfall recorded in another project's retrospective is repeated.
- **Resolution:** Before designing any process, read the installer, retrospective and technical-debt records of projects using the same SoC and projects by the same author.
- **Applicability:** General.

### Compare the "foundation" before reusing community work

- **Symptom:** A direct switch to a community kernel is considered, but its production image depends on the vendor device tree plus overlays, global resource-retention parameters and a custom init; its standalone device tree is only a skeleton.
- **Resolution:** First compare the device-tree approach, resource-management model, init and installation method. If the foundations differ, import complete feature families (display, charging, camera), while retaining this device's own board-level values. If the foundations match, switch the baseline directly.
- **Applicability:** General.

### Internal information leaks into external documentation, or statements become inconsistent

- **Symptom:** The README contains internal codenames and experiment numbers; release notes consist entirely of hashes; or three documents describe the same feature differently.
- **Resolution:** External documentation should be written only for people encountering the project for the first time. Record each conclusion in one place only. The main agent must personally review the documentation before release.
- **Applicability:** General.

### Inventing a root cause from a single data point
**Marker:** [Frequent]

- **Common forms:** A single failure is attributed to a conspicuous property (for example, image size), then disproved by changing another variable; a clock that does not run during suspend is used to calculate sleep duration, producing a claim of "spontaneous wake"; or a display defect is inferred under dark-scene or unusual-load conditions when the control group shows the same result. Once an incorrect conclusion is handed to a sub-agent to "fix", the wasted effort multiplies.
- **Resolution:** Before reporting a root cause, try to disprove it with a second data point or a control group. First establish that the measurement method itself is trustworthy (clock source, lighting and scene).
- **Applicability:** General.

### Unloading or rebinding a real hardware driver while unattended makes the device unreachable
**Marker:** [Critical]

- **Symptom:** Driver unloading hangs, then a normal reboot stalls during shutdown. The debug channel has already disconnected, leaving only a user pressing a button.
- **Resolution:** Never unload or rebind a real hardware driver while unattended. If a module must be replaced, stage it before reboot and validate it in a new RAM boot. Before rebooting, check for tasks in D state; if any exist, use SysRq to reboot.
- **Applicability:** General.

### Writing a candidate kernel directly to the boot slot; a GPU hang then stalls ordinary shutdown
**Marker:** [Critical]

- **Symptom:** A package-manager kernel upgrade on an installed system writes the candidate to the active slot through a package hook. During application-coverage testing, the GPU reports `timeout waiting for GMU OOB set GPU_SET`; subsequent hang recovery attempts fail. An ordinary `reboot` then stalls during DRM teardown. The USB debug channel is gone, no usable hardware watchdog is available, and the device remains unreachable for hours until someone holds the power button. The fallback slot had also been replaced by the same candidate when a rebuilt package of the same version was reinstalled.
- **Cause:** Three existing rules in the experience library—boot candidates from RAM first with panic-on-hang parameters; check for D-state tasks and use SysRq before reboot; and retain a known-good fallback—were not included in the task instructions given to the operator. The operator continued after the first hardware failure instead of stopping.
- **Confirmation:** On the host, the device's USB network interface remains present but has no traffic; neither ADB nor Fastboot can find the device. After a forced reboot, the previous boot's log stops during GPU recovery and shutdown.
- **Resolution:** Hold the power button to force a reboot. Then validate the candidate by booting from RAM with `softlockup_panic=1 hung_task_panic=1 oops=panic panic=10` and a time limit for automatic reboot, before restoring the known-good fallback slot.
- **Prevention:**
  - Make these rules mandatory in every task brief involving the device; keeping them only in the experience library is insufficient.
  - If the GPU or a driver has hung, or a D-state task exists, use only a time-limited `sync` followed by SysRq (`s`, `u`, `b`), or `systemctl reboot -ff`.
  - Stop testing at the first hardware failure, collect evidence, force a reboot and report. Do not continue testing during the same boot.
  - Arm hung-task panic and automatic reboot after panic at runtime in debug builds as well.
  - Do not rotate the fallback slot when reinstalling a rebuilt package with the same kernel version.
- **Note:** `panic_on_oops` is a sysctl and does not work when placed on the kernel command line; use `oops=panic`. `hung_task_panic` and `softlockup_panic` also require their corresponding detectors to be built into the kernel (`CONFIG_DETECT_HUNG_TASK`, `CONFIG_SOFTLOCKUP_DETECTOR`). Before device testing, confirm that `/proc/sys/kernel/hung_task_panic` exists; otherwise this safety net is absent.
- **Applicability:** General.

### Closing a user-level problem because the lower layer passed

- **Symptom:** A camera capture script obtains a frame, so "camera unavailable" is closed even though the user still cannot access a camera in an application.
- **Resolution:** Close a user-reported problem only after acceptance through the actual entry point the user uses.
- **Applicability:** General.

### Desktop automatic behaviour interferes with testing

- **Symptom:** A backlight-register check fails because GNOME automatic brightness rewrites the value; a GUI test cannot launch an application because it runs from a shell without the session environment.
- **Resolution:** Disable interfering automatic behaviour before testing, then restore it afterwards. For GUI tests, obtain the environment from the session (using `systemctl --user show-environment`) or launch through a desktop-automation tool.
- **Applicability:** GNOME; 2026.

### Reviewing community contributions

- **Practice:** Merge as much as possible and make small fixes in your own commits. Reject material unrelated to project scope or carrying legal or ethical risks (for example, impersonating a vendor updater to bypass download restrictions). After merging, record the author and source.
- **Applicability:** General.
