# Pass 1 — Checkpoint 08: Sensors, Camera and Hardware Video Decoding

## 1. Position and scope

- Traversal: **Pass 1 — initial structural and linguistic reconstruction**
- Previous checkpoint: [Pass 1 / Checkpoint 07](pass-1-checkpoint-07.md)
- Completed source: `experience/sensors-camera-media.md`
- Source branch: `main`
- Source blob SHA: `98f85dbecb4ec3248a31ac337729784788f0423b`
- English translation: `translation/en/experience/sensors-camera-media.md`
- Translation review: `translation/reviews/experience/sensors-camera-media.md.md`
- Chinese descriptive reconstruction: `reconstruction/zh/sensors-camera-media.md`
- Next source: `experience/usb-and-storage.md`
- Technical validation: not performed

This checkpoint records movement within Pass 1. The declared purpose remains initial structural and linguistic reconstruction, not technical validation.

## 2. Reconstructed document role and coverage

The source is an experience record covering three related but distinct areas:
- Qualcomm SSC sensors and automatic rotation;
- Qualcomm Iris/Venus hardware video decoding;
- camera support.

Its structure is recommended route, completion criteria, pitfalls and unresolved issues. The source does not define a unified multimedia architecture or shared lifecycle/state model across these areas.

The routes have distinct support assumptions:
- Sensors use FastRPC → hexagonrpcd → libssc → iio-sensor-proxy → desktop compositor. Device-local calibration and registry data are imported read-only from Android's data partition and excluded from the public repository.
- Hardware video decoding uses an upstream driver, device-specific signed firmware, a modular kernel driver, a device-tree node and userspace GStreamer plugins.
- Camera support begins by checking upstream CAMSS and camera-clock-controller support. Where absent, the first release should normally mark the camera unsupported.

## 3. Completion and evidence boundaries

- Rotation requires agreement across physical placement, raw sensor data, SensorProxy output, compositor state and screenshot for all four orientations; first boot after a fresh installation must require no manual operation.
- Video requires frame-by-frame agreement with software decoding and actual application access to hardware decoding.
- Camera requires a correct image captured in a real-world scene. A `/dev/video*` node is insufficient.

The acceptance paths are not identical. Rotation reaches the desktop compositor; camera acceptance explicitly reaches the user's application path. The source does not define a shared acceptance template.

## 4. Failure-record distinctions

### Sensors and rotation

Failure mechanisms span:
- installation-matrix assumptions;
- Hall-switch polarity and changes lost during kernel-branch reconstruction;
- polkit at login;
- compositor discovery timing;
- iio-sensor-proxy re-entrancy;
- hexagonrpcd method and large-buffer handling;
- remote-processor startup before registry availability.

Mutter's tablet-mode requirement and simpledrm's Unknown connector are explicitly identified as non-sensor causes of rotation failure.

ADSP sensor_pd reads configuration once per ADSP boot; its state survives hexagonrpcd restarts. Each test group therefore requires a fresh boot and recorded boot ID. Shuffling test order is also recommended to expose serialisation errors.

### Hardware video decoding

The no-frame case distinguishes V4L2 queue/stream calls from firmware-message format and sequencing. The source also records:
- multimedia drivers built in can prevent images from booting, with the camera clock controller as the primary suspect;
- video-driver unload can deadlock through a mutex/interrupt-thread lock cycle and make normal reboot unavailable;
- VP9 slow-consumer timestamp handling can fabricate an end marker;
- AV1 container `av1C` data can be passed to the decoder as bitstream.

The fatal unload entry requires no unattended unload/rebind of real hardware drivers and RAM-boot validation of new modules.

### Camera

The camera entries distinguish:
- sensor chip identification from vendor-library naming;
- frame capture from application-level camera availability through a daemon and PipeWire;
- verified actuator voltage from guessed regulator steps;
- persistent `-EPIPE` symptoms from an unconfirmed possible cause.

## 5. Explicit cross-document relationships

- `patterns.md`, Pattern D, explicitly lists this document for fresh-boot sensor configuration checks and stale device-local data copies.
- `patterns.md`, Pattern F, explicitly lists this document for captured frames that do not make a camera available to applications.
- `patterns.md`, Pattern I, explicitly lists this document for video-driver unload deadlock and shutdown/recovery risk.
- `experience/README.md` lists the document's subject areas as collection navigation.
- The source document itself contains no Markdown links to other domain documents.

## 6. Observer-reconstructed correspondences

- **Layered user-facing path:** sensors reach the compositor through SensorProxy; cameras reach applications through a daemon and PipeWire. Lower-layer success does not prove user-facing availability.
- **Fresh-state validity:** ADSP sensor_pd configuration is read once per ADSP boot; restarting hexagonrpcd does not create a fresh test state.
- **Target-specific reuse boundary:** community patches and adjacent-SoC platform data are not proof of equivalence; firmware message formats, registry data, calibration and actuator voltage remain device-specific.
- **Timing/lifecycle:** remote-processor startup can precede registry availability; compositor discovery and login-time policy affect rotation; daemon restart does not necessarily reset ADSP state.
- **Layered evidence:** physical orientation, raw data, proxy output, compositor state and screenshot are distinct; video evidence separates V4L2 calls, firmware traffic, decoded frames and application use.
- **Recovery safety:** driver unload can deadlock the kernel; RAM-boot validation and prohibition on unattended unload/rebind preserve recovery options.

These are observer correspondences, not a source-defined unified architecture.

## 7. Unresolved and underspecified areas

Explicit unresolved items:
- upstream camera-subsystem support for new SoCs;
- autofocus;
- video recording.

The persistent camera `-EPIPE` condition is separately marked not located and has only a possible cause. It is not a confirmed root cause.

Other boundaries:
- no detailed camera bring-up sequence is given for SoCs with CAMSS support;
- the sensor and camera routes use different user-facing acceptance endpoints;
- GStreamer plugins are required but not enumerated;
- remote-processor reset mitigation on another user's device is limited to a disable switch and log collection;
- the source does not establish whether the Iris unload patch under review has merged.

These questions require new discriminating evidence but do not prevent moving to the next document.

## 8. Bounded sufficiency for Pass 1

For the initial structural and linguistic reconstruction purpose, the three routes, their support assumptions, completion criteria, failure records, explicit pattern references and unresolved items have been recovered. The possible cause of persistent camera `-EPIPE` remains explicitly unconfirmed.

Further investigation requires new device, implementation, upstream or author evidence. No sensor, rotation, camera, V4L2, firmware, GStreamer, regulator, driver-unload or recovery claim has been technically validated.

## 9. Position change

Pass 1 remains the same declared traversal. The document position advances from `experience/power-and-time.md` (Checkpoint 07) to `experience/sensors-camera-media.md` (Checkpoint 08). The next source in the experience-library index is `experience/usb-and-storage.md`.

Pass number, document position and session checkpoint number remain separate coordinates.

## 10. Re-entry sequence

1. Read `reconstruction/zh/continuation/current-position.md`.
2. Read this checkpoint, then Checkpoint 07 and earlier checkpoints as needed.
3. Read `reconstruction/zh/README.md` and `translation/INVENTORY.md`.
4. Revisit `patterns.md`, especially Patterns D, F and I, only where their explicit references are relevant.
5. Independently inspect `experience/usb-and-storage.md`; distinguish its own claims from expectations formed by earlier documents.
