# Pass 1 — Checkpoint 04: Touch and Input

## 1. Position and scope

- Traversal: **Pass 1 — initial structural and linguistic reconstruction**
- Previous checkpoint: [Pass 1 / Checkpoint 03](pass-1-checkpoint-03.md)
- Completed source: experience/touch-and-input.md
- Source branch: main
- Source blob SHA: 92d2f179a5bdb65ccbf2c1560734f84e0ef13ffd
- English translation: translation/en/experience/touch-and-input.md
- Translation review: translation/reviews/experience/touch-and-input.md.md
- Chinese descriptive reconstruction: reconstruction/zh/touch-and-input.md
- Next source: experience/audio.md
- Technical validation: not performed

This checkpoint records movement within Pass 1. It does not begin a new pass or change the declared first-pass purpose.

## 2. Reconstructed document role

The source is a domain-specific experience record for touch, keys, keyboard accessories and fingerprint. Stylus appears in the title/scope, but the body supplies no separate stylus route, acceptance criteria or pitfall. This absence is recorded rather than inferred away.

The document contains recommended routes, completion criteria, concrete pitfall records and unresolved issues. It does not define one uniform input architecture.

## 3. Material distinctions recovered

The touch route first distinguishes:
- Standalone controllers with firmware stored in chip flash.
- TDDI controllers whose firmware is in RAM and is lost when the panel resets.
- THP arrangements where the kernel transfers raw frames and userspace computes touch points.

Other distinct routes cover PMIC/device-tree keys and keyboard accessories whose protocol core is separated from board-specific power/detect/direction controls and tablet-mode integration.

Acceptance is user-facing:
- Touch gestures, multi-touch, corners and orientations, tested separately across cold boot, screen cycles and panel power cycles.
- Stable input-device numbering across cold boots.
- Real evdev key press/release events and expected desktop behaviour.
- Real keyboard keys/combinations, detach/reattach and screen-off recovery.

Interrupt counts, device-node presence and injected events are explicitly insufficient as touch acceptance.

## 4. Explicit cross-document relationships

### display.md

The display document explicitly requires high-refresh-rate testing together with touch and links to this document. The touch document records a more specific timing failure: synchronous waiting for the first touch frame in the panel-enable callback can block display kickoff and fail at some refresh rates.

This is a cross-subsystem lifecycle/timing relationship. It does not imply that all touch failures are display failures.

### kernel-and-resources.md

The keyboard-accessory entry explicitly refers to this file for the missing always-on power supply condition that can leave the bus low. It also lists voltage-step and clock-frequency conditions. Electrical readiness is therefore a separate diagnostic layer before protocol debugging.

### patterns.md

The general problem-pattern document explicitly lists touch/input examples under:
- B — resources without a genuine owner.
- C — timing and lifecycle.
- H — another device's implementation cannot be copied wholesale.

These are source-established pattern references, not new categories created in this reconstruction.

## 5. Observer-reconstructed correspondences

The reconstruction records, without treating them as source-defined architecture:

- Panel lifecycle as a shared dependency between display and TDDI touch firmware.
- The distinction between firmware loss on panel reset and callback blocking before display kickoff.
- Electrical readiness as a prerequisite to interpreting keyboard protocol behaviour.
- User-boundary evidence as stronger than driver logs or device registration.
- Target-device evidence as necessary for board values, calibration and accessory lifecycle even when same-family/community code supplies useful structure.
- Chassis metadata as a policy input with side effects beyond exposing the GNOME power-key setting.

## 6. Unresolved and evidence boundaries

The source explicitly leaves three issues unresolved:
- THP releases a stationary press after approximately one frame and sometimes causes whole-screen flicker.
- The first physical shortcut-key press does not respond.
- A tablet booted while attached to the magnetic keyboard does not detect it until detached and reattached.

The source does not establish a stylus-specific route, a universal input/panel/keyboard state model, a complete input-device test matrix or general accessory-protocol portability criteria.

This pass has not validated kernel APIs, driver behaviour, PMIC values, GNOME/BlueZ policy, firmware procedures or device-specific claims. These are reserved for a separate technical evidence pass.

## 7. Why audio is next

The experience-library index lists audio.md immediately after touch-and-input.md. The next document is therefore selected by the source collection's own order. The current reconstruction does not presume that audio shares the touch document's lifecycle or resource relationships; those must be recovered from the audio source itself.

## 8. Re-entry sequence

1. Read reconstruction/zh/continuation/current-position.md.
2. Read this checkpoint, then Checkpoint 03 and earlier checkpoints as needed.
3. Read reconstruction/zh/README.md and translation/INVENTORY.md.
4. Revisit patterns.md, display.md and kernel-and-resources.md where their explicit links are relevant.
5. Independently inspect experience/audio.md, distinguishing its own claims from expectations formed by the input/display material.

## 9. Position change

Pass 1 remains the same declared traversal. The document position has advanced from gpu.md (Checkpoint 03) through touch-and-input.md (Checkpoint 04). The next position is audio.md. Pass number, document position and session checkpoint number remain separate coordinates.
