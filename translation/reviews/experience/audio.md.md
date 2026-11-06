# Translation Review: Audio

## Source provenance

- Source: `experience/audio.md`
- Source branch: `main`
- Source blob SHA: `46e576b39a1d724e2b9d422373a05ea60409dcfc`
- English artifact: `translation/en/experience/audio.md`
- Reconstruction pass: Pass 1 — initial structural and linguistic reconstruction
- Review scope: document role, internal route, acceptance boundaries, failure records, explicit cross-document relationships, terminology and evidence limits
- Technical validation: not performed

## Document role

This is an AudioReach/ADSP-centred audio bring-up experience record. It covers DSP prerequisites, hardware topology, backend and amplifier reuse, topology compilation, PCM/DSP graph progression, amplifier tuning and power-up, microphone paths, UCM/PipeWire integration, volume and user-facing acceptance.

The document is organised around a layered bring-up route, completion criteria, individual failure records and unresolved issues. It is not a general Linux audio reference and its examples are grounded in Qualcomm platforms and Ubuntu/PipeWire where stated.

## Internal structure

### Recommended route

The route is explicitly sequential at the layer level:

1. Establish ADSP firmware loading (PAS), QRTR, PD mapper and GPR.
2. Recover actual hardware topology from the vendor runtime device tree and mixer configuration.
3. Reuse same-SoC working backends and amplifier drivers; compile topology with a fixed audioreach-topology version; recover backend parameters from ACDB where possible.
4. Progress through PCM open/configuration/prepare, DSP graph acceptance, amplifier activation, silent/all-zero playback, electrical loopback and human listening.
5. Validate UCM and PipeWire on a fresh system; configure upmix for multi-channel speakers.
6. Treat first audible output as safe bring-up, not the final product gain decision.

This is a progression through distinct layers and evidence boundaries, not a claim that every device uses the same physical backend or microphone arrangement.

### Completion criteria

The source separates four evidence surfaces:

- **Playback/software and DSP:** pointer movement, zero error counters, amplifier register readback, correct tuning-file versions and no amplifier/DSP errors during one boot.
- **Machine-observed signal:** electrical loopback or the device microphone recording the test tone.
- **Human confirmation:** an agreed audible test window, with each speaker confirmed separately on multi-speaker devices.
- **Microphone function:** recording must track an external source's presence and absence, with a no-source control.

All must be tested on a fresh installation without saved ALSA state. The source therefore treats the environment's retained configuration as part of the acceptance boundary.

### Pitfall records

The document records distinct failure mechanisms across:
- TDM clock polarity and clock properties.
- Bootloader rewriting root compatible and acceptance tests ignoring asynchronous errors.
- Lost remote-processor crash-reason reporting.
- Frontend/backend PCM format and channel-count incompatibility.
- DAPM amplifier timing before the TDM bit clock exists, and PRM clock-ID selection.
- Invalid topology port numbering.
- Device-tree amplifier tuning-file naming and loss of an unexported fix from the patch series.
- Safe bring-up gain leaking into release configuration, driver defaults, missing speaker protection and UCM not exposing hardware gain.
- PipeWire stereo upmix configuration.
- UCM route placement, unavailable controls and saved ALSA state causing Dummy Output.
- A failed external-display output invalidating a whole PipeWire profile, including built-in microphone access.
- Mainline DSP fixed DMA-channel assumptions and analogue/digital microphone misclassification.
- Microphone acceptance without an actual sound source.
- Ubuntu ALSA sequencer dependency.
- PipeWire MMAP pre-fill deadlock hidden by a custom test tool.
- Vendor amplifier telemetry being mistaken for output evidence.
- A missing amplifier supply represented temporarily by a zero-voltage placeholder regulator.
- The safety trade-off between vendor gain and absence of ADSP speaker protection.

The source's entries do not share a fully uniform schema. The translation preserves cases where symptoms, confirmation or prevention are not separately stated.

## Explicit cross-document relationships

The audio source itself contains no Markdown links to other experience documents. The following connections are established by neighbouring source material and the repository's pattern index, rather than direct links inside audio.md.

### patterns.md — residual state (Pattern D)

Pattern D explicitly cites audio.md for the fresh-installation "Dummy Output" failure. Its general treatment is to enumerate retained state, reproduce on a fresh system/new boot/empty directory, and validate without residual state.

This is directly consistent with audio.md's requirement to test UCM/PipeWire without `/var/lib/alsa/asound.state`. The pattern is an explicit index-level relationship; the detailed audio mechanism remains in audio.md.

### patterns.md — user-visible function (Pattern F)

Pattern F's general diagnostic route requires tracing the complete hardware-to-user path and closing a user-reported problem only after the final user entry point works. The audio document's progression from DSP and amplifier state through loopback/recording to human listening is a domain-specific instance of this broader evidence boundary.

The Pattern F text is not a direct link from audio.md; this is a cross-document correspondence, not a source-declared audio taxonomy.

### kernel-and-resources.md

The kernel/resource document names `SND_SEQUENCER` among kernel options whose absence can silently remove desktop functionality, and separately notes that Ubuntu's ALSA hook requires `snd-seq`. Audio.md records the corresponding user-space symptom: USB sound-card or codec drivers do not load automatically unless `SND_SEQUENCER=m` is enabled and packaged.

This is a shared technical dependency visible across the two documents. Neither record establishes that every audio bring-up failure is a kernel-module issue.

### experience/README.md

The experience index defines the library's navigation and maintenance rules: consult patterns first, then the relevant domain file; only confirmed root causes belong in the pitfalls section, while unresolved issues remain at the end. Audio.md follows that collection pattern. This is a collection-level relationship rather than a direct in-document link.

## Observer-reconstructed correspondences

These are readings across the source, not a source-defined architecture:

- **Layered evidence:** successful PCM setup, DSP graph acceptance, amplifier activation, electrical output and human audibility are different observations. A positive result at one layer does not establish the next.
- **Topology is device-derived:** same-SoC reuse provides a baseline, while the target's vendor runtime device tree, mixer configuration and ACDB data ground the actual topology and backend parameters.
- **Asynchronous error channels matter:** a synchronous `prepare` return can be successful while amplifier/APM/ADSP errors appear later in logs.
- **Measurement requires a stimulus and control:** microphone acceptance needs an external sound source and a no-source comparison; amplifier telemetry is not output evidence.
- **Fresh-state reproducibility:** saved ALSA state can conceal invalid UCM routing or controls. A clean install is not merely packaging hygiene; it changes whether the system exposes its actual first-run behaviour.
- **Safety and product policy:** conservative first-sound gain, product gain and vendor speaker-protection behaviour are separate decisions. Increasing gain without equivalent protection is a user-facing hardware risk, not merely a tuning choice.
- **Lifecycle and service state:** WirePlumber failure is associated with APM/PRM requests becoming unresponsive, but the source leaves the recovery mechanism unresolved beyond reboot.

## Important distinctions preserved

- ADSP/PAS, QRTR, PD mapper and GPR readiness are prerequisites, not proof of audio output.
- Backend type, amplifier model/count and microphone path are hardware-topology questions.
- Frontend PCM capabilities and DSP/backend capabilities can differ.
- Amplifier activation may need to wait until the TDM bit clock exists.
- A correct graph and a successful `prepare` do not prove audible output.
- Tuning-file identity/version is separate from amplifier driver registration.
- Safe bring-up gain is not necessarily suitable product gain.
- PipeWire profile availability can couple output and microphone exposure.
- A saved ALSA state can make a development machine pass while a fresh installation fails.
- A microphone test without a source is not a valid negative result.
- Vendor power/voltage telemetry is not a substitute for loopback, recording or listening.
- The zero-voltage placeholder regulator is explicitly a temporary workaround with technical debt, not a confirmed physical supply.
- The absence of ADSP speaker protection constrains safe maximum gain.

## Unresolved and underspecified areas

The source explicitly leaves three issues unresolved:
- Mainline lacks an ADSP speaker-protection module, limiting maximum volume.
- It is unknown whether an occasional amplifier power-on error during boot is harmless.
- Following a WirePlumber crash, ADSP APM/PRM requests stop responding and only reboot restores operation.

The source does not establish:
- A universal topology applicable to all Qualcomm audio devices.
- A complete mapping of every possible microphone, amplifier or backend variant.
- A general recovery procedure for the WirePlumber/APM/PRM failure.
- A quantified safe gain limit independent of the specific amplifier and protection implementation.

These absences are not filled from external audio documentation.

## Translation decisions

- “音频” is translated as “Audio”; “功放” as “amplifier” rather than assuming every device is a speaker-only power amplifier.
- “后端” and “前端” are rendered as “backend” and “frontend”, retaining the ALSA/AudioReach graph context.
- “播放图” is rendered as “playback graph”.
- “调参文件” is rendered as “tuning file”.
- “上混” is rendered as “upmixing”.
- “全零播放” is rendered as “all-zero playback”; this is distinct from audible playback.
- “电气回环” is rendered as “electrical loopback”.
- “人耳” is rendered as “human listening” to retain the source's separate human-confirmation stage.
- Technical identifiers and names (`PAS`, `QRTR`, `GPR`, `LPASS`, `ACDB`, `q6apm`, `wm_adsp`, `APM`, `PRM`, `UCM`, `WirePlumber`) are retained.
- “占位 regulator” is rendered as “placeholder regulator”; the translation preserves that it is a zero-voltage temporary device-tree workaround and technical debt.

## Review boundary

This review assesses translation fidelity and reconstructability of the source's route, evidence boundaries, failure mechanisms and unresolved issues. It does not validate AudioReach APIs, DSP topology, amplifier register values, firmware/tuning compatibility, PipeWire/UCM behaviour, kernel configuration or safe gain limits. Those require a separate technical evidence pass.


## Pass 3 — technical and semantic review

- Source revision reviewed: `experience/audio.md` on `main`, blob SHA `46e576b39a1d724e2b9d422373a05ea60409dcfc`.
- English artifact reviewed: `translation/en/experience/audio.md` (blob SHA `7764597a750a7cc4c22873feddcb5005fef2537d`).
- Review scope: all six recommended-route steps, five completion criteria, all eighteen pitfall entries, the three unresolved issues, technical tokens, imperative force, conditional scope, structure and cross-document relationships.

### Review findings

**No source-driven translation correction was required.** The English text preserves the source's instructions, conditions, distinctions and evidence limits without requiring stylistic rewriting.

### Confirmed preservation

- The six-step route retains the ADSP/PAS, QRTR, PD mapper and GPR prerequisites; target-device topology recovery; same-SoC reuse; fixed-version topology compilation; the PCM-to-human-listening progression; fresh-system UCM/PipeWire acceptance; and the separation between safe first-sound gain and product gain.
- The five completion criteria remain distinct: playback/DSP/amplifier state, machine-observed signal, human listening, microphone stimulus/control, and fresh-installation state. A successful earlier layer is not substituted for later evidence.
- All eighteen pitfalls retain their separate mechanisms and uneven source fields. These include TDM clock polarity, rewritten root `compatible`, asynchronous errors, remote-processor crash reporting, frontend/backend format intersection, DAPM/trigger timing and PRM clock identity, topology port numbering, device-tree tuning-file naming, gain and speaker-protection safety, PipeWire upmix, UCM route/control placement, profile-wide failure from an external-display output, fixed DMA-channel assumptions, microphone stimulus, ALSA sequencer dependency, MMAP pre-fill deadlock, vendor telemetry limits, placeholder supply debt, and the gain-limit trade-off.
- Source-specific instructions and force remain represented: check the root `compatible` first when PD mapper reports the stated platform error; test the only configuration difference first with one variable changed; repair crash-reason reporting before disassembly; treat any amplifier/APM/ADSP error during the boot as acceptance failure; treat firmware-version or algorithm mismatch warnings as real problems; do not increase gain in repeated 6 dB steps; and do not include unverified external-display outputs in the default verb.
- Numerical and technical details remain intact, including S32/S16/S24, the four-channel frontend limit, `Invalid channels (8)`, `mute_unmute_on_trigger`, `ControlExists`, `/var/lib/alsa/asound.state`, `arecord`, EIO, `SND_SEQUENCER=m`, PREPARED/START, `dvdd-supply`, `regulator-fixed`, approximately −24 dB and −4.5 dB, and the named AudioReach, ADSP, APM, PRM, LPASS, ACDB, q6apm, wm_adsp, UCM, PipeWire and WirePlumber identifiers.
- The three unresolved issues remain: absence of a mainline ADSP speaker-protection module and its volume consequence; uncertainty about an occasional amplifier power-on error during boot; and APM/PRM requests becoming unresponsive after a WirePlumber crash, with reboot as the stated recovery.
- The source contains no Markdown links to other experience documents. The translation does not invent any. The review retains the collection-level relationship to Pattern D, the correspondence with Pattern F, and the shared `SND_SEQUENCER` dependency with `kernel-and-resources.md` as relationships observed across the source collection, not direct links authored in audio.md.

### Structural and observer boundaries

The layered route is not converted into a universal hardware topology or state machine. Same-SoC reuse remains a starting point, while target-device vendor runtime device-tree and mixer evidence ground the actual topology. The separate playback, electrical, human-listening and microphone evidence surfaces remain distinct.

The relationship between PCM/DSP/amplifier signals and audible output, the role of fresh ALSA state, and the safety implications of gain without equivalent speaker protection are preserved as source-grounded distinctions or explicitly labelled observer correspondences. They are not expanded into a universal audio architecture.

The source does not establish a topology for every Qualcomm audio device, a complete mapping of microphone/amplifier/backend variants, a general recovery procedure for the WirePlumber/APM/PRM failure, or a device-independent safe gain limit. These absences remain explicit.

### Pass 3 disposition

- Source-based technical and semantic translation review: **completed**.
- Translation changes: **none required**.
- External technical correctness, current AudioReach/API behaviour, DSP topology, firmware and tuning compatibility, PipeWire/UCM behaviour, amplifier register values, safe gain limits and real-device behaviour: **not validated**.
- Whole-skill validation: **outstanding**.
- Next document: `experience/wireless.md`.

This review establishes source fidelity only; it does not validate the audio subsystem or the device-specific technical claims.
