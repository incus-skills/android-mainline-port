# Touch, Keys, Keyboard Accessories, Stylus and Fingerprint

## Recommended Route

**Touch:** First identify the hardware arrangement:

- **Standalone touch controller, firmware stored in chip flash:** The driver is relatively independent; treat it as a normal I2C/SPI peripheral.
- **Touch-and-display integration (TDDI), firmware stored in RAM:** Resetting the panel loses the touch firmware. The driver must follow the panel power sequence (`drm_panel_follower`) and download the firmware again after each panel power-on.
- **Raw data plus host-side algorithm (THP):** The kernel only downloads firmware and transfers raw frames; userspace computes touch points. This requires an open-source algorithm implementation and device-specific calibration data.

Procedure: Read the vendor driver source (disassemble only when source is unavailable) and the firmware-download sequence and command timing in the vendor logs → use a mainlined driver for the same family as the baseline → use a device with the same SoC as a reference for node structure, but use values from this device → compare each function against the vendor implementation, including frame format → integrate with the panel power sequence → calibrate orientation.

**Keys:** The power key and volume-down key usually come from PMIC PON (`pwrkey`/`resin`); volume-up is often a PMIC GPIO using `gpio-keys`. Describe them according to this device's original device tree, and use the vendor debounce values.

**Keyboard accessories:** Extract the protocol from the vendor driver and implement it as an HID transport driver or a serdev driver. Keep the protocol core separate and testable. Add board-specific power, detect and direction pins, and integrate the tablet-mode switch.

## Completion Criteria

- **Touch:** Test real-finger taps, swipes, multi-touch, all four corners and both portrait and landscape orientations. Test cold boot, screen-off/screen-on and panel power cycling separately. Input-device numbers must remain stable across repeated cold boots. An increasing interrupt count, an input device existing, or events injected by a program do not count as success.
- **Keys:** Verify real evdev press and release events; the desktop must show the volume indicator or perform the configured power-key action.
- **Keyboard:** Verify real key presses, key combinations, detach and reattach, and recovery after the screen turns off.

## Pitfalls

### Touch Stops Working Permanently after a TDDI Panel Turns Off

Tag: **[Frequent] [Time-consuming]**

- **Symptoms:** After the display turns off automatically following several idle minutes, touch does not work when the screen comes back on. SPI reports no errors; the interrupt line remains low; the interrupt count stops increasing; logs show no errors. Restarting the touch service restores operation immediately.
- **Cause:** Display and touch are in the same chip. The panel driver asserts reset when the display turns off, which also removes the touch firmware. The touch driver downloads firmware only when the service starts.
- **Confirmation:** Let the desktop turn the screen off for a few seconds, then turn it on. Check whether the interrupt count resumes and whether a new firmware-ready log appears.
- **Resolution:** Register the touch driver as a panel follower. Stop the stream and download firmware again in the panel's enabled/disabling (or prepared/unpreparing) callbacks. If the device is opened before the panel is powered on, do not fail immediately; defer until the panel is ready.
- **Prevention:** If earlier touch testing passed while the bootloader's panel state was still in place, repeat the test after the display driver takes over or screen-off support is added.
- **Applicability:** TDDI panels (for example, the Novatek NT365xx family); 2026.

### Truncated Frame Length Leaves Firmware in an Intermediate State, but the Driver Reports Success

Tag: **[Time-consuming]**

- **Symptoms:** The driver prints "Update firmware success", but there are no interrupts or data.
- **Cause:** A community implementation truncated the vendor's state-cleanup frame and checksum-enable frame. Its success check only tested a one-byte threshold.
- **Confirmation:** Use this device's vendor binary as the reference and compare every transaction byte-for-byte, including lengths. Check whether the firmware state machine actually reaches its ready state.
- **Resolution:** Restore the vendor frame lengths. Change the success check to test the actual firmware-ready status value.
- **Applicability:** Closed-protocol touch controllers such as Novatek; general method.

### The Second Header of a Segmented Firmware Image Is Not Written, but the Checksum Still Passes

- **Symptoms:** Hardware CRC matches, but the status remains incorrect and firmware information is all zero.
- **Cause:** A header segment outside the CRC coverage was not written; a conditional branch was misread during disassembly.
- **Resolution:** Find vendor source and compare function by function. First create a raw SPI interface through debugfs so that a reboot experiment taking ten minutes becomes an online experiment taking seconds.
- **Prevention:** Do not disassemble when source is available. A passing CRC does not prove that all data was written.
- **Applicability:** Touch controllers using segmented (cascade) firmware; general method.

### Touch Commands Are Sent Too Early and Time Out

- **Symptoms:** The first command times out when opening the data stream, and open fails.
- **Cause:** The vendor sends these commands only after firmware is ready and data frames have started arriving (an interval of several hundred milliseconds).
- **Resolution:** Wait for the first frame before sending commands. Restore commands omitted from the vendor sequence. Do not add fixed delays or retries.
- **Applicability:** THP-style touch; 2026.

### SPI Transfers Through GPI DMA Return EIO

- **Symptoms:** The data stream immediately fails with `-EIO`.
- **Cause:** The GPI DMA completion callback sets the status first, so the SPI core does not accumulate `actual_length`.
- **Resolution:** Correct when the callback sets the status (kernel fix).
- **Applicability:** Qualcomm GENI SPI with GPI DMA; 2026.

### Touch Probes Before the Panel Powers On, Reads All-Zero Data and Triggers a NULL-Pointer Failure

- **Symptoms:** On a dual-DSI panel, the touch driver loads before the panel is ready, reads a zero chip ID and subsequently hangs through a NULL-pointer failure.
- **Cause:** `fw_devlink` transfers the touch dependency on the panel to the main DSI device, so the driver probes as soon as SPI is ready. `input->dev.parent` is not set, causing a NULL pointer in `touchscreen_parse_properties()`.
- **Resolution:** Do not access hardware during probe; wait until the panel is ready for the first time before reading the chip ID. Set the input device's parent.
- **Applicability:** TDDI with dual DSI; 2026.

### Touch Orientation Calibration Repeatedly Fails

- **Symptoms:** Touching the top-left corner produces a response at the bottom-right, or the axes are swapped.
- **Cause:** The mapping was inferred from a remembered sequence of four-corner taps. The kernel processes the transform by mirroring first and swapping axes afterwards.
- **Resolution:** Ask the user to make one swipe in a single direction, then derive the mapping from the raw evdev coordinates. Use the generic `touchscreen-swapped-x-y` and `touchscreen-inverted-x/y` properties, or apply a fixed userspace transform.
- **Applicability:** General.

### Volume Keys Do Not Respond

- **Symptoms:** Key presses produce no events.
- **Cause:** The volume keys are not described in the device tree at all, or the pins and debounce values copied from a reference device differ from this device.
- **Resolution:** Add `gpio-keys` for volume-up and PMIC PON `resin` for volume-down, using this device's original device tree. Use the vendor debounce values.
- **Prevention:** When the user tests keys, use a key recorder that runs for an extended period rather than a test window lasting only a few dozen seconds, so presses are captured whenever they happen.
- **Applicability:** Qualcomm; 2026.

### A Keyboard Accessory Enumerates Successfully but Produces No Key Data

Tag: **[Time-consuming]**

- **Symptoms:** Several HID devices register, the version query times out and there are no interrupts.
- **Cause:** The vendor's always-on power supply is missing, leaving the bus held low (see `kernel-and-resources.md`); the voltage is not on a valid PMIC step; or the device tree omits the clock frequency and the vendor default is 400 kHz.
- **Resolution:** First distinguish the electrical layer: detach the accessory, check for zero transfers, switch the pins to GPIO, and drive only the wake pin. Confirm the line levels before investigating the protocol.
- **Prevention:** Enumeration is not progress. Provide a real bus self-test that can be triggered at any time.
- **Applicability:** I2C keyboard bridge (for example, Nanosic); general method.

### Keyboard Protocol Details

- Do not mix up the frame-header length, the two checksum algorithms or the business-frame prefix.
- A single-wire, half-duplex UART receives its own echo. Verify that the echo matches the transmitted frame before treating it as delivered.
- The host must not transmit first; wait for the keyboard's synchronisation packet.
- Newer serial-core versions add an intermediate `<uart>:<ctrl>.<port>` device, which older diagnostic tools cannot find.
- Do not register a touchpad that the accessory does not have. If it does have a touchpad, mark it as an external device to avoid conflict with tablet mode.
- **Applicability:** Magnetic keyboard; 2026.

### The Screen Does Not Rotate Automatically When the Keyboard Is Attached

- **Cause:** GNOME (Mutter) manages screen orientation only when the tablet-mode switch is ON. When the keyboard is attached, the switch is OFF. This is expected behaviour, not a defect.
- **Applicability:** GNOME.

### Fingerprint

- **Status mapping:** Map only the exact retry prompt to `retry`; waiting states are not device errors.
- Template files require mode `0600`; set `UMask=0077` for the service.
- Determine success or failure from PAM, `Session.Unlock` and `LockedHint`, not driver logs.
- Establishing credentials for the first time during a RAM boot also writes to secure storage; it is not a zero-write operation.
- Do not hand a bus shared with the secure world over to Linux.
- **Applicability:** QSEECOM + libfprint-TOD; 2026.

### Waiting Synchronously for the First Touch Frame in the Panel-Power-On Callback Slows Screen Wake, Sometimes Breaks Touch, and Can Fail after Refresh-Rate Changes

Tag: **[Frequent] [Time-consuming]**

- **Symptoms:** After screen-off/screen-on, the first frame sometimes times out after 500 ms and touch stops working (no SPI error). After changing to 120/60 Hz in Settings, touch fails (`THP first valid frame not received: -110`), while 90/144 Hz works. Screen wake is slower overall.
- **Cause:** The touch driver waits synchronously for the first frame in the panel-enable callback. That callback runs before the display controller starts output (kickoff), so the display commit is blocked. At some refresh rates, the first frame can never arrive within the timeout.
- **Confirmation:** Record the times of the panel callback, display kickoff and first touch interrupt together.
- **Resolution:** Have the callback queue asynchronous work only; move the first-frame wait into that work item while preserving the time budget. Do not extend the timeout, remove commands or hide the problem with automatic service restarts. After the fix, the screen-wake path became about 0.3 seconds faster, and touch recovered at all four refresh rates.
- **Applicability:** TDDI/THP touch with DRM panel follower; 2026.

### The Power Lifecycle of a Magnetic Keyboard Must Match the Vendor Implementation

- **Symptoms:** The keyboard stops responding after screen-off or suspend. Only wake or accelerometer frames appear on the bus; there are no HID frames. Removing power and powering it on again restores operation.
- **Cause:** The accessory's power-state register is not written as in the vendor's screen-off/screen-on sequence (for example, write 0 on screen-off, then read before writing 1 on screen-on). A community periodic keepalive and automatic reset were used to mask the problem.
- **Resolution:** Read the vendor driver's screen-off/screen-on lifecycle and implement the same sequence. Remove periodic keepalive and workarounds such as "discard the first interrupt".
- **Prevention:** Protocols can differ completely between accessories of the same category on different devices (for example, one uses an active UART heartbeat while another uses an event-driven I2C bridge). Do not copy another device's keepalive strategy.
- **Applicability:** Magnetic keyboard; 2026.

### GNOME Settings Does Not Show a Power-Key Option

- **Cause:** GNOME hides power-key settings for machines whose Chassis is `tablet` or `handset` (and also hides them when suspend is disabled).
- **Resolution:** Set Chassis to `convertible` (write it only if missing, and remove it on uninstall). Note that `convertible` changes other behaviour, including sleep delay and idle-sleep hints. BlueZ has no mapping from `convertible` to a Bluetooth device class, so the remote peer sees "uncategorised".
- **Applicability:** GNOME 49/50, BlueZ 5.85; 2026.

## Unresolved

- THP touch: a stationary press is released after approximately one frame; the entire screen occasionally flickers during touch operation.
- The first physical shortcut-key press does not respond.
- When the tablet boots while attached to the magnetic keyboard, the keyboard is not detected; removing and reattaching it makes it appear.
