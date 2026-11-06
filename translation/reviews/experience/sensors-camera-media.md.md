# Translation Review: Sensors, Camera and Hardware Video Decoding

## Source and scope

- Source: `experience/sensors-camera-media.md`
- Source branch: `main`
- Source blob SHA: `98f85dbecb4ec3248a31ac337729784788f0423b`
- Translation: `translation/en/experience/sensors-camera-media.md`
- Traversal: Pass 1 — initial structural and linguistic reconstruction
- Review scope: recommended routes, completion criteria, failure records, terminology, cross-document relationships and unresolved boundaries
- Technical validation: not performed

## Document role and structure

The source is a domain experience record spanning three related but distinct areas: Qualcomm sensor integration and automatic rotation, Qualcomm Iris/Venus hardware video decoding, and camera support. It follows the experience-library structure: recommended route, completion criteria, pitfalls and unresolved issues.

The three routes have different support assumptions:
- Sensor integration describes a userspace chain from FastRPC and hexagonrpcd through libssc and iio-sensor-proxy to the desktop compositor. Device-local calibration and registry data are imported read-only and excluded from the public repository.
- Hardware video decoding assumes an upstream driver, device-specific signed firmware, a loadable kernel module, a device-tree node and userspace GStreamer support.
- Camera support begins by checking for upstream CAMSS and camera-clock-controller support. Where these are absent, the source recommends marking the camera unsupported in the first release rather than implying that a device node alone establishes support.

The source does not define a unified multimedia architecture or a shared lifecycle/state model across sensors, camera and video decoding.

## Translation decisions

- “传感器” is translated as “sensors”; “自动旋转” as “automatic rotation”, keeping the sensor subsystem distinct from the desktop behaviour it enables.
- “高通 SSC” is retained as “Qualcomm SSC”.
- “远端处理器” is translated as “remote processor”; SLPI, ADSP and sensor_pd remain technical identifiers.
- “registry” is retained for the sensor calibration/configuration store. The source treats it as device-specific data imported from Android's data partition, not public project material.
- “借用固件画面” is translated as “borrowing the firmware display”, consistent with the display document's borrowed-display route.
- “视频硬解” is translated as “hardware video decoding”; Iris/Venus, V4L2 and GStreamer remain named.
- “相机子系统” is translated as “camera subsystem”; CAMSS is retained.
- “相机时钟控制器” is translated as “camera clock controller”.
- “实景” is translated as “real-world scene”, preserving the requirement for an actual scene rather than a synthetic node or test-only condition.
- “取帧” is translated as “capture frames”; successful frame capture is not conflated with application-level camera availability.
- “慢速消费” is translated as “slow consumer” / “slow consumption”, retaining the deliberate per-frame read delay used to expose the VP9 issue.
- “未定位” is rendered as “not located”, preserving the source's unresolved status rather than implying a confirmed root cause.
- Technical names, message identifiers, error codes, command names and the `>>1` notation are retained verbatim.

## Route and acceptance structure

### Sensors and rotation

The source gives an explicit chain:
FastRPC → hexagonrpcd → libssc → iio-sensor-proxy → desktop compositor.

It also identifies device-local calibration/configuration as an input to that chain, with a read-only import boundary and a prohibition on committing this data to the public repository. Reuse is preferred from the same SoC or same-generation community patch sets.

Rotation acceptance is multi-surface: physical placement, raw sensor data, SensorProxy output, compositor state and screenshot must agree in each of four orientations. The first boot after a fresh installation must work without manual intervention.

The pitfalls show that a failure at the desktop may originate at several distinct layers: installation matrix, Hall-switch polarity, polkit, compositor discovery timing, iio-sensor-proxy re-entrancy, hexagonrpcd method/buffer handling, or remote-processor startup before its registry is readable. The source also distinguishes non-sensor causes: Mutter's tablet-mode condition and simpledrm's Unknown connector.

Sensor tests are boot-sensitive because the ADSP sensor PD reads configuration once per ADSP boot and retains state across a hexagonrpcd restart. The source therefore requires a fresh boot for each test group and a recorded boot ID.

### Hardware video decoding

The route combines upstream driver support, device-specific signed firmware, modular driver integration, device-tree enablement and userspace GStreamer plugins.

Acceptance requires frame-by-frame agreement with software decoding and actual application access to hardware decoding. The failure records distinguish V4L2 queue/stream operations from firmware-message sequencing, and distinguish fast from slow frame consumption. The source gives separate VP9 and AV1 failure mechanisms.

The fatal unload case concerns kernel locking and recovery risk, not decoding output correctness. It prohibits unattended unload/rebind of real hardware drivers and requires RAM-boot validation of a new module.

### Camera

The source first checks upstream CAMSS and camera clock-controller support; absent support may mean the first release should explicitly mark the camera unsupported.

Acceptance requires a correct image from a real-world scene. The failure records distinguish:
- sensor identification from vendor-library naming;
- a captured frame from a camera exposed through the daemon/PipeWire/application path;
- a confirmed autofocus supply voltage from a guessed regulator step;
- an unresolved persistent `-EPIPE` pipeline state from a confirmed cause.

The unresolved section separately names new-SoC camera-subsystem support, autofocus and video recording.

## Explicit cross-document relationships

- `patterns.md`, Pattern D, explicitly lists `sensors-camera-media.md` for fresh-boot sensor configuration checks and stale device-local data copies.
- `patterns.md`, Pattern F, explicitly lists `sensors-camera-media.md` for the case where frames can be captured but the camera is unavailable to applications.
- `patterns.md`, Pattern I, explicitly lists `sensors-camera-media.md` for the video-driver unload deadlock and shutdown/recovery risk.
- The source's own body does not contain Markdown links to other domain documents.
- `experience/README.md` lists this document as covering sensors and automatic rotation, camera and hardware video decoding. That is collection-level navigation, not a dependency declared by the document itself.

## Observer-reconstructed correspondences

- **Layered user-facing path:** sensor readings pass through SensorProxy and the compositor; camera frames pass through a daemon and PipeWire before reaching applications. In both cases, lower-layer success does not establish the user-facing function.
- **Fresh-state validity:** sensor configuration is read once per ADSP boot, so restarting hexagonrpcd is not equivalent to a fresh test state. This aligns with Pattern D, but the sensor document supplies the domain-specific mechanism.
- **Target-specific reuse boundary:** community patches and adjacent-SoC platform data are starting points, not proof of equivalence. Firmware message formats, registry data, sensor calibration and actuator voltage remain device-specific.
- **Timing and lifecycle:** remote-processor startup can precede registry availability; compositor discovery and login-time policy checks affect rotation; service restart does not necessarily reset ADSP sensor state.
- **Evidence at multiple layers:** physical placement, raw readings, proxy output, compositor state and screenshot are separate observations. Video validation likewise separates V4L2 operations, firmware traffic, decoded frames and application use.
- **Recovery safety:** video-driver unload can deadlock the kernel and make normal reboot unavailable. The source's RAM-boot and no-unattended-unload instructions preserve a recovery path while testing.

These are descriptive correspondences across the document. The source does not name them as a common architecture or formal model.

## Unresolved and underspecified areas

The source explicitly leaves:
- upstream camera-subsystem support for new SoCs;
- autofocus;
- video recording.

It also marks the camera pipeline's persistent `-EPIPE` condition as not located and provides only a possible cause. This must not be promoted to a confirmed root cause.

Other coverage boundaries:
- The camera route gives a support-presence check but no detailed bring-up sequence where CAMSS is available.
- The sensor route names the compositor as the final layer, while the camera route explicitly requires an application entry point; the source does not define one shared acceptance template.
- The video route says userspace GStreamer plugins are needed, but does not enumerate packages, plugin versions or a detailed userspace setup sequence.
- The source does not specify a general recovery policy for remote-processor resets on another user's device beyond a disable switch and log collection.
- The source does not establish whether the upstream patch under review for the Iris unload deadlock has since merged.

These boundaries are retained rather than filled with external multimedia or kernel knowledge.

## Bounded sufficiency for Pass 1

For initial structural and linguistic reconstruction, the three routes, their distinct support assumptions, the completion criteria, the recorded failure mechanisms, explicit pattern references and unresolved issues have been recovered. The possible cause of the persistent camera `-EPIPE` condition remains explicitly unconfirmed.

Further work on new-SoC camera support, autofocus, recording, the `-EPIPE` condition or the Iris unload patch requires new device, implementation, upstream or author evidence. Those questions do not prevent proceeding to the next experience document.

No sensor, remote-processor, rotation, camera, V4L2, firmware, GStreamer, regulator, driver-unload or recovery claim has been technically validated in this pass.

## Position change

Pass 1 remains the same declared traversal. The document position advances from `experience/power-and-time.md` (Checkpoint 07) to `experience/sensors-camera-media.md` (Checkpoint 08). The next source is determined by the ordering in `experience/README.md`: `experience/usb-and-storage.md`.

Pass number, document position and session checkpoint number remain separate coordinates.
