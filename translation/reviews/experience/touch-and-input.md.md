# Translation Review: Touch, Keys, Keyboard Accessories, Stylus and Fingerprint

## Source provenance

- Source: `experience/touch-and-input.md`
- Source branch: `main`
- Source blob SHA: `92d2f179a5bdb65ccbf2c1560734f84e0ef13ffd`
- English artifact: `translation/en/experience/touch-and-input.md`
- Reconstruction pass: Pass 1 — initial structural and linguistic reconstruction
- Review scope: document role, internal structure, operational meaning, explicit cross-document relationships, observer correspondences, terminology and evidence boundaries
- Technical validation: not performed

## Document role

This is a domain-specific experience record covering touch controllers and touch-and-display integration, physical keys, keyboard accessories, fingerprint support, and stylus in its title/scope. It provides recommended routes, completion criteria, confirmed pitfall records and unresolved issues.

The body does not provide a separate stylus implementation route or stylus-specific pitfall/acceptance section. That absence is preserved; no stylus behaviour is inferred from the touch or keyboard material.

## Internal structure

### Recommended routes

The opening section distinguishes three touch architectures:

1. Standalone controller with firmware in chip flash.
2. TDDI, where panel reset removes firmware held in RAM and the touch driver must follow panel power lifecycle.
3. THP, where the kernel transfers raw frames and userspace computes touch points using an open algorithm and device-specific calibration.

It then gives a source-led implementation procedure: inspect vendor source and logs, use a same-family mainlined driver as a baseline, take structure from same-SoC references but values from the target device, compare frame formats function by function, integrate with panel power and calibrate orientation.

Keys and keyboard accessories have separate routes. The former is tied to the target device's original device tree and PMIC topology. The latter separates protocol logic from board-specific electrical controls and desktop tablet-mode integration.

### Completion criteria

The criteria are explicitly user-facing and lifecycle-sensitive:

- Touch requires physical interaction across gestures, multi-touch, corners and orientations, plus separate cold-boot, screen-cycle and panel-power-cycle tests. Stable input-device numbering is also required across repeated cold boots.
- Keys require real evdev press/release events and the expected desktop response.
- Keyboard accessories require real keys, combinations, detach/reattach and recovery after screen-off.

The source explicitly rejects interrupt counts, device-node existence and programmatically injected events as sufficient touch evidence.

### Pitfall records

The entries cover distinct mechanisms rather than a single generic input failure:

- TDDI firmware loss on panel reset.
- Truncated vendor command frames and insufficient firmware-ready checks.
- Missing firmware segment header despite a passing CRC.
- Commands issued before THP data frames begin.
- GPI DMA completion ordering and SPI `actual_length`.
- Probe ordering and input-device parent setup on dual-DSI TDDI.
- Coordinate transform order and calibration.
- Missing or incorrect PMIC/GPIO key description.
- Keyboard enumeration without electrical bus readiness.
- Protocol framing, checksum, UART echo and host/keyboard initiation.
- Expected GNOME tablet-mode orientation policy.
- Fingerprint status mapping, secure template permissions, PAM/session-level acceptance and secure-world bus ownership.
- Blocking first-frame wait in the panel-enable callback.
- Keyboard power-state lifecycle mismatch.
- GNOME chassis metadata changing power-key UI and other desktop/Bluetooth behaviour.

The entries are not a uniform schema. The translation preserves omissions rather than adding fields that the source did not provide.

## Explicit cross-document relationships

### `display.md`

The display document explicitly says that high-refresh-rate validation must be performed together with touch testing and links to this file. The touch document adds concrete touch acceptance across refresh-rate changes and records a panel-enable callback failure in which waiting synchronously for the first touch frame blocks display kickoff.

This establishes a direct relationship between display lifecycle/timing and touch operation. It does not mean every touch issue is a display issue.

### `kernel-and-resources.md`

The keyboard-accessory entry explicitly refers to this file for the missing always-on power supply/resource condition that can leave the bus low. It also lists voltage-step and clock-frequency causes. This connects accessory protocol symptoms to electrical/resource readiness before protocol debugging.

### `patterns.md`

The experience index routes troubleshooting through the general problem-pattern document. That document explicitly cites touch-and-input examples under:
- Pattern B: resources without a genuine owner (keyboard bus power).
- Pattern C: timing and lifecycle (TDDI firmware loss, early commands, panel callback blocking and keyboard power state).
- Pattern H: another device's implementation cannot be copied wholesale (keyboard keepalive strategy).

These are explicit index/pattern references. The pattern labels remain the source's general categories, not a newly inferred touch taxonomy.

## Observer-reconstructed correspondences

The following are readings across the source material, not a new source-defined architecture:

- **Panel lifecycle as shared dependency:** TDDI touch firmware and display state are coupled through panel reset and power callbacks.
- **Callback ordering and non-blocking work:** the first-frame case links panel callback timing, display kickoff and touch interrupt arrival. It is distinct from firmware loss on reset, even though both can follow screen transitions.
- **Electrical readiness before protocol:** successful HID enumeration does not prove that the accessory bus is electrically usable; power, voltage and clock must be distinguished from protocol behaviour.
- **Evidence at the user boundary:** touch events, desktop key behaviour, keyboard operation and fingerprint unlock/session state are stronger acceptance evidence than driver messages or device registration.
- **Device-specific provenance:** same-family or community implementations can supply useful structure, but board values, electrical details, calibration and accessory lifecycle must come from the target device's evidence.
- **Policy metadata has side effects:** changing chassis classification can expose a GNOME setting but also changes sleep behaviour and Bluetooth peer classification.

## Important distinctions preserved

- Standalone, TDDI and THP touch architectures have different firmware and data responsibilities.
- Firmware stored in RAM is lost on panel reset; this is separate from commands sent too early or a callback blocked before display kickoff.
- A successful firmware update message, CRC match, interrupt count or input-device node does not independently establish functional touch.
- Panel probe timing, input-device parent setup and touch firmware readiness are distinct conditions.
- A real bus/electrical problem must be separated from an accessory protocol problem.
- Enumeration is not proof of usable keyboard input.
- A magnetic keyboard's power lifecycle is not interchangeable with another device's keepalive strategy.
- GNOME's keyboard-attachment orientation behaviour is described as expected policy, not a defect.
- Fingerprint success is judged at PAM/session unlock state, not by driver logs.
- RAM boot credential creation still writes secure storage; it is not a zero-write operation.
- Stylus is named in the document's scope but not separately specified in its body.

## Unresolved and underspecified areas

The source leaves three issues unresolved:
- THP releases a stationary press after approximately one frame and sometimes causes whole-screen flicker.
- The first physical shortcut-key press does not respond.
- A tablet booted while attached to the magnetic keyboard does not detect it until it is detached and reattached.

The source does not define:
- A stylus-specific route, protocol or acceptance criteria.
- A universal state model for touch/panel/keyboard power transitions.
- A complete test matrix for all input devices.
- General criteria for when an accessory protocol can be considered portable across device models.

The translation does not fill these gaps with assumptions from adjacent documents.

## Translation decisions

- “触控显示一体 (TDDI)” is rendered as “touch-and-display integration (TDDI)”.
- “主机算法 (THP)” is rendered as “host-side algorithm (THP)”; the source's kernel/userspace division is retained.
- “面板 follower” is rendered as “panel follower”, retaining the Linux DRM relationship.
- “磁吸键盘” is rendered as “magnetic keyboard”.
- “吸附” in keyboard lifecycle contexts is rendered as “attach” or “detach and reattach”, not as a generic USB connection.
- “常开电源” is rendered as “always-on power supply”.
- “电源键行为符合设置” is rendered as the configured power-key action.
- “安全世界” is rendered as “secure world”.
- “通过” is translated contextually as “pass” or “success”; it is not treated as proof beyond the source's stated evidence boundary.

## Review boundary

This review assesses translation fidelity and reconstructability of the source's structure and operational distinctions. It does not validate the kernel APIs, driver behaviour, PMIC values, GNOME/BlueZ policy, firmware procedures or device-specific claims. Those require a separate technical evidence pass.


## Pass 3 — semantic review

Source revision `92d2f179a5bdb65ccbf2c1560734f84e0ef13ffd` reviewed against the English artifact. No source-driven translation correction was required. The three touch architectures, route ordering, target-device provenance, user-facing acceptance criteria, separate electrical/protocol/firmware/lifecycle failure mechanisms, technical tokens, explicit cross-document references and unresolved issues are preserved. The source names stylus in its title/index scope but provides no stylus-specific route or acceptance criteria; this absence remains explicit. No universal input state model or complete test matrix is inferred. External technical correctness and real-device behaviour remain unvalidated. Next document: `experience/audio.md`.
