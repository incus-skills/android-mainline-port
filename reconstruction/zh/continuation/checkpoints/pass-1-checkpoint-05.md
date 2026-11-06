# Pass 1 — Checkpoint 05: Audio

## 1. Position and scope

- Traversal: **Pass 1 — initial structural and linguistic reconstruction**
- Previous checkpoint: [Pass 1 / Checkpoint 04](pass-1-checkpoint-04.md)
- Completed source: experience/audio.md
- Source branch: main
- Source blob SHA: 46e576b39a1d724e2b9d422373a05ea60409dcfc
- English translation: translation/en/experience/audio.md
- Translation review: translation/reviews/experience/audio.md.md
- Chinese descriptive reconstruction: reconstruction/zh/audio.md
- Next source: experience/wireless.md
- Technical validation: not performed

This checkpoint records movement within Pass 1. The declared purpose remains initial structural and linguistic reconstruction, not technical validation.

## 2. Reconstructed document role

The source is an AudioReach/ADSP-centred audio bring-up experience record. It covers DSP prerequisites, hardware topology, backend and amplifier reuse, topology compilation, PCM/DSP graph progression, amplifier activation and tuning, microphone paths, UCM/PipeWire, volume and user-facing acceptance.

It is not a general Linux audio reference. Its examples are grounded in Qualcomm platforms and Ubuntu/PipeWire where stated.

## 3. Recovered route

The recommended route progresses through:
1. ADSP firmware loading (PAS), QRTR, PD mapper and GPR readiness.
2. Target-device topology recovery from vendor runtime device tree and mixer configuration.
3. Reuse of working same-SoC backend/amplifier implementations; fixed-version topology compilation; ACDB-derived backend parameters.
4. PCM open/configuration/prepare, DSP playback-graph acceptance, amplifier activation, all-zero playback, electrical loopback and human listening.
5. UCM/PipeWire validation on a fresh system, including upmix for multi-channel speakers.
6. Conservative first-sound gain, with product gain decided separately.

## 4. Evidence boundaries

The source distinguishes:
- Software/DSP/amplifier state: pointer movement, error counters, register readback, tuning-file version and absence of boot-time amplifier/DSP errors.
- Machine-observed signal: electrical loopback or recording through the device microphone.
- Human listening: audible test tone during an agreed window, with each speaker confirmed separately.
- Microphone behaviour: external source presence/absence reflected in recording, with a no-source control.

All checks must pass on a fresh installation without saved ALSA state. PCM open, graph acceptance, prepare success or amplifier telemetry alone do not establish audible output.

## 5. Explicit cross-document and collection relationships

The audio source itself contains no Markdown links to other experience documents. Relevant relationships were recovered from collection-level material:

- **patterns.md / Pattern D:** explicitly lists audio.md for the fresh-installation "Dummy Output" problem. The pattern's residual-state procedure corresponds to the audio requirement to test without saved ALSA state.
- **patterns.md / Pattern F:** supplies a general rule to trace hardware-to-user entry and close user-reported problems only after the final user path works. Audio's progression to loopback, recording and human listening is a correspondence, not an explicit audio-specific Pattern F listing.
- **kernel-and-resources.md:** lists `SND_SEQUENCER` as a kernel configuration whose absence can silently remove functionality, and notes the Ubuntu ALSA hook's `snd-seq` dependency. Audio.md records the corresponding automatic-loading failure for USB sound cards/codecs.
- **experience/README.md:** establishes the collection navigation and maintenance rules followed by audio.md: patterns first, then domain experience; confirmed root causes in pitfalls, unresolved cases at the end.

## 6. Observer-reconstructed correspondences

- **Layered evidence:** PCM, DSP graph, amplifier, electrical output and human audibility are distinct observations.
- **Target-derived topology:** same-SoC reuse is a baseline, while target vendor runtime device tree, mixer configuration and ACDB ground actual topology and parameters.
- **Asynchronous error channels:** a successful synchronous prepare return can coexist with later amplifier/APM/ADSP errors.
- **Stimulus and control:** microphone acceptance needs an external sound source and a no-source comparison.
- **Fresh-state reproducibility:** saved ALSA state can conceal UCM/PipeWire defects.
- **Safety and product policy:** safe bring-up gain, release gain and speaker-protection capability are separate decisions.
- **Service lifecycle:** WirePlumber crash is associated with unresponsive APM/PRM requests, but the source does not provide a recovery route beyond reboot.

These are descriptive correspondences, not a source-defined architecture.

## 7. Unresolved and evidence boundaries

The source leaves three issues unresolved:
- Mainline lacks an ADSP speaker-protection module, limiting maximum volume.
- It is unknown whether an occasional amplifier power-on error during boot is harmless.
- Following a WirePlumber crash, ADSP APM/PRM requests stop responding; recovery requires reboot.

The source does not define a universal Qualcomm audio topology, a complete map of all microphone/amplifier/backend variants, a general WirePlumber/APM/PRM recovery procedure or a device-independent safe gain limit.

No AudioReach API, DSP topology, amplifier register value, firmware/tuning compatibility, PipeWire/UCM behaviour, kernel configuration or safe gain claim has been technically validated in this pass.

## 8. Why wireless is next

The experience-library index lists wireless.md immediately after audio.md. The next source is selected by the source collection's own order. No assumption is made that wireless shares the audio document's topology or evidence model; those relationships must be established from the wireless source itself.

## 9. Re-entry sequence

1. Read reconstruction/zh/continuation/current-position.md.
2. Read this checkpoint, then Checkpoint 04 and earlier checkpoints as needed.
3. Read reconstruction/zh/README.md and translation/INVENTORY.md.
4. Revisit patterns.md, kernel-and-resources.md and experience/README.md where their collection-level relationships are relevant.
5. Independently inspect experience/wireless.md, distinguishing its own claims from expectations formed by audio and earlier subsystem documents.

## 10. Position change

Pass 1 remains the same declared traversal. The document position has advanced from touch-and-input.md (Checkpoint 04) through audio.md (Checkpoint 05). The next position is wireless.md. Pass number, document position and session checkpoint number remain separate coordinates.
