# Pass 1 — Checkpoint 07: Battery, Charging, Suspend and System Time

## 1. Position and scope

- Traversal: **Pass 1 — initial structural and linguistic reconstruction**
- Previous checkpoint: [Pass 1 / Checkpoint 06](pass-1-checkpoint-06.md)
- Completed source: experience/power-and-time.md
- Source branch: main
- Source blob SHA: d74ba3b4bc320f2036f185e7614d8cdc1a703c6b
- English translation: translation/en/experience/power-and-time.md
- Translation review: translation/reviews/experience/power-and-time.md.md
- Chinese descriptive reconstruction: reconstruction/zh/power-and-time.md
- Next source: experience/sensors-camera-media.md
- Technical validation: not performed

This checkpoint records movement within Pass 1. The declared purpose remains initial structural and linguistic reconstruction, not technical validation.

## 2. Reconstructed document role and coverage

The source is a cross-cutting power/time experience record covering battery, charging, suspend, system time, watchdog and thermal management. Its structure is recommended route, completion criteria, concrete pitfalls and unresolved issues.

Body coverage is uneven:
- Battery/fuel-gauge measurement and charging status/control receive detailed route and failure treatment.
- Suspend covers prerequisites, deep versus s2idle behaviour, wake-source tracing, clock measurement, charging handoff and fuel-gauge polling.
- RTC/system time distinguishes normally read-only PMIC RTC access, transitional saved-time restoration plus NTP, and an SDAM offset as the root approach.
- Watchdog appears in the title but has no dedicated route, completion criterion or pitfall entry in this document.
- Thermal management is limited to using vendor temperature zones and thresholds; no detailed acceptance or troubleshooting procedure is given.

The document does not define a general power-management state machine or unified lifecycle architecture.

## 3. Recovered route

1. Read fuel-gauge data through an upstream I2C driver or ADSP battmgr via pmic_glink; compare against the vendor system.
2. Start charging integration read-only. Account for SoC-generation-specific qcom_battmgr layouts/current signs; defer writes and constrain writable values.
3. Treat private fast charging as a separate userspace authentication and 9 V negotiation capability.
4. Defer suspend until native display, touch, keyboard power management and wireless recovery are complete. A first release may disable suspend if recorded as technical debt.
5. Treat PMIC RTC as normally read-only; distinguish SDAM offset storage from boot-time saved-time restoration plus NTP.
6. Use vendor temperature zones and thresholds.

## 4. Completion and evidence boundaries

- Battery readings are compared with contemporaneous vendor readings, with example tolerances of ±8% or ±300 mV.
- Charging requires a 15–30 minute wall-charger test, rising charge level, sustained positive current and normal battery temperature. A status flag alone is insufficient.
- Plug/unplug latency is measured separately at kernel sysfs and UPower layers.
- Suspend acceptance uses successful-suspend count, CLOCK_BOOTTIME versus CLOCK_MONOTONIC, unchanged boot ID, and working display/touch after resume.
- Time acceptance checks for rejected PMIC writes during a boot, seconds-level accuracy after reboot and OS switching, and synchronisation after networking becomes available.

There is no explicit completion criterion for watchdog or thermal management.

## 5. Failure-record distinctions

Charging entries distinguish:
- Low battery or inadequate computer-USB input from software instability.
- Plug detection and desktop charging indicators from actual net charging.
- Private fast-charge authentication/high-voltage negotiation from PD recognition at 5 V.
- Charging flags from current, voltage, bus voltage and charge trends.
- SoC-generation-specific battmgr layouts and current-sign conventions.
- Read-only PMIC evidence from a first write experiment, which should be isolated as a single-variable RAM-boot test.
- Shared I2C access to a fuel gauge, and Android charging that may be disabled after prolonged computer-USB use.

Time entries distinguish read-only PMIC RTC, transitional saved-time restoration, NTP synchronisation and SDAM offset storage. The chrony workaround explicitly accepts unauthenticated time.

Suspend entries distinguish:
- Missing wake-source records from absence of a wake source.
- Deep suspend from s2idle.
- CLOCK_MONOTONIC from elapsed suspend time.
- One suspend interval from multiple s2idle wake/suspend segments.
- Charging-client handoff and fuel-gauge polling lifecycle from ordinary suspend entry.

## 6. Explicit cross-document relationships

- The source explicitly links display.md for the unresolved black-screen-after-suspend issue.
- patterns.md Pattern C explicitly lists power-and-time.md for timing/lifecycle issues, including charging-control handoff during suspend.
- patterns.md Pattern E explicitly lists power-and-time.md for monotonic-clock and s2idle segmented-duration measurement errors.
- patterns.md Pattern I discusses controlled watchdog non-feeding tests, but its explicit file references are bootloader-and-boot.md, sensors-camera-media.md and testing-and-process.md; it does not list power-and-time.md.
- kernel-and-resources.md names RTC_DRV_PM8XXX, NVMEM_REBOOT_MODE, NVMEM_SPMI_SDAM and BATTERY_QCOM_BATTMGR. These are relevant collection-level correspondences, not links declared by the power/time source.

## 7. Observer-reconstructed correspondences

- **Measurement versus status:** charge level, current, voltage and trend are evidence; a UI icon, charger classification or authentication flag alone does not establish charging.
- **Read-only evidence before writes:** PMIC and charging-control writes are delayed until target-specific behaviour and constraints are understood.
- **Lifecycle coupling:** suspend depends on display, touch, keyboard and wireless readiness; charging clients and fuel-gauge polling also require suspend/resume handling.
- **Measurement validity:** clock selection and s2idle segmentation affect suspend-duration interpretation. This corresponds to Pattern E but is not a source-defined general measurement model.
- **Layered reporting:** kernel sysfs and UPower may report different delays and must be measured separately.
- **Target-specific parameters:** SoC generation, vendor protocol, PMIC ownership, SDAM allocation and temperature thresholds constrain transfer between devices.

These are descriptive correspondences, not a source-defined unified power architecture.

## 8. Unresolved and underspecified areas

The source explicitly leaves three issues unresolved:
- Linux net charging and fast charging require private authentication and negotiation; the first release usually cannot close this.
- The cause of RTC returning to 1970 after an abnormal reset is unknown.
- Display remains black after suspend/resume; the source points to display.md.

Additional coverage boundaries:
- Watchdog is named in the title but has no dedicated body route or acceptance criterion.
- Thermal management is limited to the instruction to use vendor temperature zones and thresholds.
- Average-power and remaining-time readings are unreliable for some fuel gauges, with no cause or resolution recorded.
- Suspend checks are present, but no universal suspend state machine or complete acceptance model is defined.
- This source does not establish whether the RTC or display issues have since been fixed.

These gaps are preserved rather than filled from external Linux power-management knowledge.

## 9. Bounded sufficiency for Pass 1

For the initial structural and linguistic reconstruction purpose, the recommended route, acceptance criteria, failure entries, explicit pattern references, relevant kernel/resource correspondences and three unresolved items have been recovered. The title/body coverage gaps for watchdog and thermal management are also recorded.

Remaining questions require device evidence, implementation evidence, author input or a separate technical-validation pass. They do not prevent moving to the next source. Reopen a question only when a new discriminating source, relevant observer-position change, translation conflict or concrete interpretive consequence becomes available.

No charging, PMIC, RTC, chrony, suspend, clock, watchdog, thermal or real-device claim has been technically validated in this pass.

## 10. Position change

Pass 1 remains the same declared traversal. The document position has advanced from wireless.md (Checkpoint 06) through power-and-time.md (Checkpoint 07). The next source in the experience-library index is sensors-camera-media.md. Pass number, document position and session checkpoint number remain separate coordinates.

## 11. Re-entry sequence

1. Read reconstruction/zh/continuation/current-position.md.
2. Read this checkpoint, then Checkpoint 06 and earlier checkpoints as needed.
3. Read reconstruction/zh/README.md and translation/INVENTORY.md.
4. Revisit patterns.md, kernel-and-resources.md, display.md and wireless.md only where their source relationships are relevant.
5. Independently inspect experience/sensors-camera-media.md; distinguish its own claims from expectations formed by earlier documents.
