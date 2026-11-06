# Sensors, Camera and Hardware Video Decoding

## Recommended Route

**Sensors (Qualcomm SSC; sensors run on SLPI or the ADSP sensor_pd)**

FastRPC → hexagonrpcd (open source, with patches) → libssc → iio-sensor-proxy (a version with SSC support) → desktop compositor. Import sensor calibration and configuration (registry) read-only from the Android data partition. This is device-unique data and must not enter the public repository. Prefer reusing patch sets from the same SoC or the same generation of community work.

**Hardware video decoding (Qualcomm Iris/Venus)**

Use the upstream driver, device-specific signed firmware (overriding the default firmware name), build the driver as a module, and enable the device-tree node. Complete the userspace side with GStreamer plugins.

**Camera**

First establish whether upstream has a camera subsystem (CAMSS) and camera clock controller for the SoC. If not, it will usually be necessary to wait for community or upstream support; mark the camera unsupported in the first release.

## Completion Criteria

- **Rotation:** for each of the four orientations, record the actual physical placement, raw sensor data, SensorProxy output, compositor state and screenshot. All five must agree. The first boot after a fresh installation must work without manual intervention.
- **Video:** output must match software decoding frame by frame (for example, by comparing each frame's MD5); applications must be able to use hardware decoding.
- **Camera:** capture a correct image in a real-world scene. The existence of `/dev/video*` does not count.

## Pitfalls

### Any Layer of Automatic Rotation Can Fail

Tag: **[Time-consuming]**

- **Symptoms:** the image is inverted in landscape orientation; it does not rotate in portrait; the D-Bus value changes but the desktop does not rotate; the failure recurs after each flash; or sensors are absent after the first installation.
- **Causes (one example at each layer):** the installation matrix is guessed from ideal values; a Hall-switch change is lost while rebuilding a kernel branch and its polarity is reversed; polkit rejects the compositor's request at login; the compositor requests rotation only when the SensorProxy name first appears, so a fixed delay cannot solve the race; the SSC implementation in iio-sensor-proxy has a re-entrancy problem; hexagonrpcd lacks a method or large-buffer handling (resolved by importing an upstream commit); the remote processor starts automatically after about two seconds, but its configuration is not readable until 20–40 seconds later.
- **Resolution:** disable automatic startup of that remote processor in the kernel and have a userspace service start it once, after the configuration is readable; confirm all four physical orientations against the matrix; add the polkit rule; act only after the compositor service name appears.
- **Prevention:** do not restart the DSP during a single boot; ensure the test framework really reports failures; test order can hide serialisation errors, so run the tests in a shuffled order as well.
- **Applicability:** Qualcomm SSC + GNOME; 2026.

### Rotation Does Not Work When the Keyboard Is Attached or While Borrowing the Firmware Display

- **Cause:** Mutter manages orientation only when the tablet-mode switch is ON; the simpledrm Unknown connector does not support rotation. Neither is a sensor problem.
- **Applicability:** GNOME; 2026.

### Sensors on Another User's Device Cause a Remote-Processor Reset

- **Resolution:** provide a disable switch as a workaround and collect logs.
- **Applicability:** SSC; 2026.

### Hardware Video Decoding Produces No Frames

- **Cause:** when platform data from an adjacent SoC is reused, the payload format of a firmware message (for example, RESUME) may differ from the vendor's format.
- **Confirmation:** first use `strace` to inspect V4L2 calls (`QBUF`, `STREAMON`, `DQBUF`), then inspect firmware message traffic.
- **Resolution:** correct the platform data to match the vendor downstream message sequence.
- **Applicability:** Qualcomm Iris; 2026.

### All Multimedia Drivers Built into the Kernel Prevent Several Images from Booting

- **Cause:** the primary suspect is the camera clock controller hanging on its first probe.
- **Resolution:** build multimedia-related drivers as modules; load a new clock controller manually as a module the first time it is used.
- **Applicability:** Qualcomm; 2026.

### Camera Sensor Model Is Identified Incorrectly

- **Resolution:** use the chip ID read back during probe and the official header files; do not guess from the vendor library name. Pay attention to 7-bit versus 8-bit CCI address notation (`>>1`).
- **Applicability:** General.

### Sensor Tests Must Be Evaluated on a Fresh Boot

- **Cause:** the ADSP sensor PD reads its configuration only once per ADSP boot, and its state survives a hexagonrpcd restart.
- **Resolution:** reboot before each test group, record the boot ID, and do not judge results using old state. The configuration-file service must provide the original ODM directory and permit writing to the entire registry parent directory (using a temporary copy); writes larger than 256 bytes require the upstream hexagonrpcd fix.
- **Prevention:** reject stale copies when importing device-local sensor configuration; use the latest version on the device.
- **Applicability:** Qualcomm SSC; 2026.

### The Camera Can Capture Frames but Is Not Visible in Applications

- **Symptoms:** a test script captures 30 real-scene frames, but applications such as Snapshot show no camera.
- **Cause:** the camera daemon (for example, the camerad system service) is missing, so PipeWire does not enumerate the device.
- **Resolution:** acceptance must reach the entry point actually used by the user. Successful frame capture does not close the issue "camera unavailable".
- **Applicability:** libcamera + PipeWire; 2026.

### The Camera Autofocus Actuator Does Not Respond

- **Cause:** the actuator supply voltage is not represented in the mainline regulator steps, and there is no device-local register evidence.
- **Resolution:** do not guess a neighbouring voltage step. First read the original-device voltage register; enable the actuator only after confirmation. Do not enable autofocus before then.
- **Applicability:** VCM actuator; 2026.

### Camera Restart Leaves the Pipeline Permanently Returning `-EPIPE`

- **Symptoms:** after intensive testing, media-pipeline link validation fails and continues to fail across camera-service restarts.
- **Possible cause:** the routing function ignores a link-disable failure and returns early, leaving links and ACTIVE formats behind.
- **Status:** not located.
- **Applicability:** libcamera software-ISP route; 2026.

### Unloading the Video Driver Deadlocks and a Normal Reboot Hangs During Shutdown

Tag: **[Fatal]**

- **Symptoms:** `rmmod qcom_iris` hangs; then, during `systemctl reboot`, USB disconnects and the device never returns.
- **Cause:** the unload path holds a core mutex while waiting for an interrupt thread to finish, while that thread is waiting for the same mutex.
- **Resolution:** fix the lock ordering in the kernel using the upstream patch under review. On the test side, do not unload or rebind real hardware drivers unattended. Stage a new module before reboot and validate it using a new RAM boot. Before rebooting, check for tasks in D state; if any exist, use SysRq (s, u, b).
- **Applicability:** Qualcomm Iris; 2026.

### VP9 Ends Early with a Slow Consumer; AV1 Drops Its First Frame

- **VP9 cause:** timestamp metadata underflows, fabricating an "end of sequence" marker. Only a slow-consumer test (delaying each frame read) exposes it.
- **AV1 cause:** the container's `av1C` data is passed to the decoder as if it were the bitstream.
- **Method:** for every video format, test both fast and slow consumption and compare frame by frame with software decoding. Confirm that the default player actually selects V4L2 hardware decoding.
- **Applicability:** Qualcomm Iris + GStreamer; 2026.

## Unresolved

- Upstream camera-subsystem support for new SoCs.
- Autofocus and video recording.
