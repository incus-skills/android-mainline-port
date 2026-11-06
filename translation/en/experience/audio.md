# Audio

## Recommended Route (Qualcomm AudioReach / ADSP)

1. **Prerequisites:** ADSP firmware loading (PAS), QRTR, PD mapper and GPR must work. If PD mapper reports "no support for the platform", first check whether the root `compatible` was rewritten.
2. **Establish the topology:** Use the vendor runtime device tree and mixer configuration to identify the speaker backend (TDM or I2S), amplifier models and count, and microphone types (digital microphones connected directly to LPASS, or analogue microphones through a codec and SoundWire).
3. **Reuse:** Prefer a same-SoC backend and amplifier driver that the community has already brought up. Compile the topology with a fixed version of audioreach-topology. Vendor ACDB data can be parsed to recover backend parameters.
4. **Proceed layer by layer:** `open` → `hw_params` → `prepare` → DSP accepts the playback graph → amplifier starts → inaudible all-zero playback test → electrical loopback → listening by a person.
5. **UCM and PipeWire:** Validate on a fresh system. Multi-channel speakers require PipeWire upmix configuration.
6. **Volume:** Use conservative gain for first sound; determine product gain separately.

## Completion Criteria

- **Playback:** The pointer advances normally, error counters remain zero, amplifier registers read back correctly (power on, DAC running, chip ID), tuning-file versions are correct, and no amplifier or DSP errors occur during a single boot.
- **Machine evidence:** Electrical loopback or the device's own microphone records the test tone.
- **Human listening:** Play an obviously audible test tone during an agreed time window and have the user confirm it. For multiple speakers, confirm each output separately.
- **Microphone:** The appearance and disappearance of an external sound source must correspond to the recording, with a no-source control.
- **All checks must pass on a fresh installation, without saved ALSA state.**

## Pitfalls

### Verify the Only Difference from the Vendor Configuration First (for example, TDM Bit-Clock Polarity)

Tag: **[Frequent] [Time-consuming]**

- **Symptoms:** The playback graph opens and TDM parameters are accepted, but the DSP fails to start the port or the amplifier produces no sound.
- **Cause:** Bit-clock (BCLK) polarity or clock properties differ from the vendor configuration. This difference is easy to dismiss as "affecting only the data" and investigate too late.
- **Resolution:** Compare the target device's vendor configuration with the new configuration item by item. Test the "only difference" first, changing one variable at a time.
- **Applicability:** TDM amplifiers; general method.

### Audio Configuration Selected by Root Compatible Never Matches, Yet the Test Reports Success

Tag: **[Time-consuming]**

- **Symptoms:** The amplifier powers on, PCM opens and the test reports "pass", but the user hears nothing.
- **Cause:** The quirk selector uses the root `compatible`, which the bootloader rewrites. The acceptance test checks only the return value from `prepare` and ignores asynchronous logs.
- **Resolution:** Select quirks by sound-card name instead. Treat any amplifier, APM or ADSP error during the boot as a failed acceptance test.
- **Prevention:** When a selector fails, search the entire subsystem for other conditions using the same assumption.
- **Applicability:** General.

### Remote-Processor Crash Reason Is Lost

- **Symptoms:** ADSP repeatedly restarts, but the cause is not visible.
- **Cause:** The message field length declared for reporting the crash reason is incorrect, so the reason is discarded.
- **Resolution:** Fix crash-reason reporting first, then use the crash address (and disassemble the Hexagon code) to locate the fault. Do not start by changing formats or timing.
- **Applicability:** Qualcomm remoteproc; 2026.

### Frontend and Backend Have No Common Format or Channel Count

- **Symptoms:** `open` returns `EINVAL`; `prepare` reports `Invalid channels (8)`; only debug-level logs are emitted.
- **Cause:** The frontend offers only S32 while q6apm supports only S16/S24; the mainline AudioReach frontend rejects more than four channels.
- **Resolution:** Use a two-channel S16 frontend, then use a channel-mixing module inside the DSP to expand it according to the vendor matrix for the multiple amplifiers.
- **Prevention:** Offline checks must cover platform PCM capability constraints. Check topology formats and channel counts at build time.
- **Applicability:** Qualcomm AudioReach; 2026.

### Amplifier Starts Before the Bit Clock Is Present

- **Symptoms:** Amplifier startup fails and the boost converter repeatedly resets.
- **Cause:** The amplifier starts during the pre-power-up phase of DAPM, but the TDM bit clock does not exist until `trigger`. The bit-clock ID used for the PRM vote may also be wrong.
- **Resolution:** Unmute the amplifier at `trigger` (`mute_unmute_on_trigger`) and vote for the correct TDM bit-clock ID.
- **Applicability:** TDM amplifiers; 2026.

### An Upstream Topology Fragment Uses Port Number 0, So the Link Is Dropped

- **Symptoms:** ADSP refuses to open the playback graph (error 1).
- **Cause:** A graph variant not used by any upstream device was selected, and it specifies port number 0.
- **Resolution:** Correct the topology and add a topology-structure checker that validates ports and connections during the build.
- **Applicability:** audioreach-topology; 2026.

### Per-Amplifier Tuning Files Are Never Loaded

Tag: **[Time-consuming]**

- **Symptoms:** Logs show that a generic tuning file is loaded; its version or algorithm ID does not match the wmfw. The warning is ignored for a long time.
- **Cause:** Mainline `wm_adsp` adds the amplifier prefix to the filename only for ACPI devices. Device-tree devices always request the generic file; the filename is converted to lowercase. The fix was not exported into the patch series and was lost when the kernel branch was rebuilt.
- **Resolution:** Fix the driver so device-tree devices also request the prefixed filename. During acceptance, read the logs to confirm the actual filename requested.
- **Prevention:** Treat firmware-version or algorithm-mismatch warnings as real problems. Keep every fix in the patch series.
- **Applicability:** DSP-equipped amplifiers such as Cirrus; 2026.

### Volume Is Significantly Too Low

Tag: **[Frequent]**

- **Symptoms:** Maximum volume is only a fraction of the vendor system's level.
- **Cause:** The low gain chosen for safe first sound was carried into the release. The amplifier driver's default gain is low (for example, −24 dB); the vendor system can go higher because it has ADSP speaker protection; UCM does not expose hardware gain, so the desktop slider changes only software volume.
- **Confirmation:** Read the actual amplifier gain register and compare it with the value during vendor playback.
- **Resolution:** Keep safe bring-up gain and product gain as separate configurations. If gain is suspected, directly test the vendor default or safe limit once; do not increase it in repeated 6 dB steps. Without speaker protection, raising gain creates an overheating risk. The user decides, and the risk must be documented.
- **Applicability:** Multi-amplifier tablets; 2026.

### Only Half of the Speakers Produce Sound

- **Symptoms:** During stereo playback, the lower pair is almost silent.
- **Cause:** PipeWire does not send stereo audio to the rear two channels by default.
- **Resolution:** Enable upmixing in the PipeWire configuration (for example, `channelmix.upmix`).
- **Applicability:** Four-speaker systems.

### A Fresh Installation Shows Only "Dummy Output"

Tag: **[Frequent]**

- **Symptoms:** Audio works on the development machine, but a new user has no audio device after installation.
- **Cause:** The capture frontend route is placed in the UCM device section instead of the verb section. PipeWire probes by opening all PCMs; if the capture endpoint cannot be opened, the entire profile becomes unavailable. The development machine hides the problem because it has saved ALSA state. Setting a control that does not exist can also interrupt the entire sequence.
- **Resolution:** Put the frontend route in the verb. Protect every control setting with `ControlExists`.
- **Prevention:** Validate UCM and every feature that depends on saved state on a fresh system without `/var/lib/alsa/asound.state`.
- **Applicability:** UCM + PipeWire; general.

### External-Display Audio Makes the Built-In Microphone Disappear from the Entire Sound Card

- **Cause:** An output in the same profile fails to open, so PipeWire discards the entire profile.
- **Resolution:** Do not include unverified external-display outputs in the default verb.
- **Applicability:** PipeWire; 2026.

### Microphone Reads Return No Data; Every Combination Fails

Tag: **[Time-consuming]**

- **Symptoms:** The first `arecord` read returns EIO, the kernel emits no logs, and every microphone-channel combination fails.
- **Cause:** The mainline DSP reads only fixed DMA channels (the channel mask is determined by the channel count). Copying vendor extractor indices that depend on a channel-mapping control absent from mainline therefore fails. Another possibility is misidentifying an analogue microphone as digital.
- **Resolution:** Compare item by item with a working configuration on the same SoC. Use the channels that the mainline DSP actually reads. As a negative control, change only the channel selection back; the error should reappear.
- **Prevention:** If every combination fails, look for a shared fixed parameter that is wrong.
- **Applicability:** Qualcomm LPASS; 2026.

### Microphone Acceptance Criterion Is Designed Incorrectly

- **Symptoms:** Recording is reported as "failed" in a quiet room.
- **Cause:** The criterion requires a signal, but there is no sound source.
- **Resolution:** Use the appearance and disappearance of an external tone as the criterion, with a no-source control; alternatively, have the device play a test tone and record it.
- **Applicability:** General.

### ALSA Hook Requires the Sequencer

- **Symptoms:** A USB sound card or codec driver does not load automatically.
- **Cause:** Ubuntu's alsa-base hook requires `snd-seq`, which is not enabled in the kernel.
- **Resolution:** Set `SND_SEQUENCER=m` and include it in the module package.
- **Applicability:** Ubuntu; 2026.

### A PipeWire MMAP Pre-Fill Deadlock Is Hidden by a Custom Test Tool

- **Cause:** A gate added in the PREPARED state to work around a problem causes PipeWire to hang during pre-fill. The custom tool writes data only after START, so it does not expose the failure.
- **Resolution:** Remove the gate after fixing the root cause. Validate using the real PipeWire path.
- **Applicability:** General.

### Vendor Telemetry Is Used to Decide Whether There Is Output

- **Cause:** Output-power and voltage-monitoring values from a vendor amplifier cannot serve as proof of actual output.
- **Resolution:** Use electrical loopback, a recording or human listening.
- **Applicability:** General.

### Amplifier Driver Requires a Supply That Does Not Exist on the Vendor Device

- **Symptoms:** The amplifier driver does not probe; the binding requires `dvdd-supply`, but neither the vendor device tree nor the driver has this supply.
- **Resolution:** Temporarily provide a zero-voltage placeholder `regulator-fixed` so the driver can load, and record the technical debt. Replace it once the real supply is confirmed.
- **Applicability:** AW88261 and similar devices; 2026.

### The Trade-Off Around the Speaker Gain Limit

- **Fact:** One amplifier driver's default is approximately −24 dB, while vendor playback is approximately −4.5 dB. However, the vendor system has ADSP speaker protection. Raising gain to the vendor level without that protection risks overheating and damage.
- **Approach:** Present the numerical chain (register value, dB and power multiplier) and the risk for the user to decide. Do not copy the gain from another device that has speaker protection.
- **Applicability:** Multi-amplifier tablets; 2026.

## Unresolved

- Mainline has no ADSP speaker-protection module, so the maximum volume is limited.
- It is not known whether the occasional amplifier power-on error during boot is harmless.
- After the audio service (WirePlumber) crashes, ADSP APM/PRM requests stop responding; recovery requires a reboot.
