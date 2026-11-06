# Translation Review: Battery, Charging, Suspend, System Time, Watchdog and Thermal Management

## Source and scope

- Source: experience/power-and-time.md
- Source branch: main
- Source blob SHA: d74ba3b4bc320f2036f185e7614d8cdc1a703c6b
- Translation: translation/en/experience/power-and-time.md
- Traversal: Pass 1 — initial structural and linguistic reconstruction
- Review scope: recommended route, completion criteria, failure records, terminology, cross-document relationships and unresolved boundaries
- Technical validation: not performed

## Document role and structure

The source is a cross-cutting power and time experience record. Its title names battery, charging, suspend, system time, watchdog and thermal management. Its body gives a recommended route and completion criteria, then records concrete pitfalls and three unresolved issues.

The actual body coverage is uneven. Battery measurement, charging, RTC/system time and suspend receive detailed treatment. Watchdog appears in the title but has no dedicated route, acceptance criterion or pitfall entry in this document. Thermal management appears as a brief rule to use vendor temperature zones and thresholds, without a corresponding acceptance criterion or detailed failure record.

The document does not define a general power-management state machine or a unified lifecycle architecture.

## Translation decisions

- “电量计” is rendered as “fuel gauge”, distinguishing the measurement device/interface from the battery itself.
- “电量” is rendered contextually as “battery level” or “charge level”; “电流” remains current and is not collapsed into a charging-status flag.
- “充电状态与控制” is rendered as “charging status and control”, preserving the distinction between observing a charging state and issuing control writes.
- “只读” is retained as “read-only”; the source explicitly places read-only evidence gathering before writes.
- “快充” is rendered as “fast charging”; private authentication and voltage negotiation remain distinct prerequisites.
- “休眠” is rendered as “suspend” in the Linux power-management context. The source separately names deep suspend and s2idle.
- “唤醒” is rendered as “wake” or “resume” according to whether the text refers to a wake event or return from suspend.
- “PMIC RTC” and “SDAM” are retained as technical identifiers. The source distinguishes a read-only RTC from storing an offset in SDAM.
- “温区和阈值” is rendered as “temperature zones and thresholds”; the source does not provide numerical values.
- “净充电” is rendered as “net charging”, preserving the distinction between a charger being detected and battery charge actually increasing.
- Clock names, kernel interfaces, systemd properties and chrony options are retained verbatim.

## Route and acceptance structure

The recommended route is staged by risk and evidence:

1. Read fuel-gauge data and compare it with the vendor system.
2. Begin charging integration with a read-only client; defer writes and constrain writable values.
3. Treat private fast charging as a separate authentication and voltage-negotiation capability.
4. Defer suspend until native display, touch, keyboard power management and wireless recovery are complete; disabling it in the first release is permitted if recorded as technical debt.
5. Treat PMIC RTC access as normally read-only and distinguish the SDAM offset approach from boot-time restoration plus NTP.
6. Use vendor-derived thermal zones and thresholds.

The completion criteria do not cover every named domain equally. Battery readings are compared with contemporaneous vendor readings; charging requires observed charge increase, positive current and normal temperature; plug/unplug latency is measured separately at kernel and UPower layers; suspend uses counters, clock differences, boot identity and post-resume display/touch function; time checks PMIC write rejection, cross-boot/OS accuracy and network synchronisation.

There is no explicit completion criterion for watchdog or thermal management.

## Failure-record distinctions

The charging cases distinguish:
- low battery or insufficient USB input from software failure;
- physical charging from desktop charging indicators;
- private fast-charge authentication and high-voltage negotiation from PD charger recognition at 5 V;
- status flags from measured current, voltage, bus voltage and battery trend;
- SoC-generation-specific battmgr layouts and current-sign conventions;
- safe read-only PMIC inspection from a first, separately isolated write experiment.

The time cases distinguish a read-only PMIC RTC, transitional saved-time restoration, NTP synchronisation and persistent SDAM offset storage. The chrony workaround explicitly accepts unauthenticated time.

The suspend cases distinguish missing wake-source records from the actual source of wake events; deep suspend from s2idle; monotonic time from elapsed suspend time; and charging-client handoff from ordinary suspend entry. The source records that s2idle may consist of multiple short wake/suspend segments.

Other cases cover shared I2C access to a fuel gauge and Android charging that is disabled after prolonged computer-USB use.

The translation preserves the uneven source-entry structure. It does not invent missing confirmation, prevention or applicability fields.

## Explicit cross-document relationships

- power-and-time.md explicitly refers to display.md for the black-screen-after-suspend unresolved issue.
- patterns.md, Pattern C, explicitly lists power-and-time.md among timing/lifecycle cases, including charging-control handoff during suspend.
- patterns.md, Pattern E, explicitly lists power-and-time.md for monotonic-clock and s2idle segmented-duration measurement errors.
- patterns.md, Pattern I, discusses controlled watchdog non-feeding tests, but its explicit file references are to bootloader, sensors/camera/media and testing/process; it does not explicitly list power-and-time.md.
- kernel-and-resources.md names RTC_DRV_PM8XXX, NVMEM_REBOOT_MODE, NVMEM_SPMI_SDAM and BATTERY_QCOM_BATTMGR. These are relevant collection-level correspondences to the RTC, SDAM and battmgr subjects, not links declared by the power document itself.

## Observer-reconstructed correspondences

- **Measurement versus status:** battery level, current, voltage and charge trend are treated as evidence; a UI icon, charger classification or authentication flag alone does not establish charging.
- **Read-only evidence before writes:** the route consistently delays PMIC and charging-control writes until their target-specific behaviour and constraints are understood.
- **Lifecycle coupling:** suspend depends on display, touch, keyboard and wireless readiness; charging clients and fuel-gauge polling also require suspend/resume handling.
- **Measurement validity:** clock selection and s2idle segmentation affect whether elapsed suspend time is interpreted correctly. This corresponds to Pattern E but is not a named general measurement model in the source.
- **Layered reporting:** kernel sysfs and UPower can have different observed delays; the source requires measuring both rather than treating the desktop display as the only layer.
- **Target-specific parameters:** SoC generation, vendor protocol, PMIC ownership, SDAM allocation and temperature thresholds constrain transfer from one device to another.

These are descriptive correspondences, not a source-defined unified power architecture.

## Unresolved and underspecified areas

The source explicitly leaves three issues unresolved:
- Linux net charging and fast charging require private authentication and negotiation; the first release usually cannot complete them.
- The cause of RTC returning to 1970 after an abnormal reset is unknown.
- Display remains black after suspend/resume; the source directs the reader to display.md.

Additional source-coverage boundaries:
- Watchdog is named in the title but has no dedicated body route or acceptance criterion.
- Thermal management is limited to the instruction to use vendor temperature zones and thresholds; no thresholds or thermal acceptance procedure are given.
- Average-power and remaining-time readings are noted as unreliable for some gauges without cause or resolution.
- The source gives suspend checks but does not define a universal suspend state machine or an overall suspend completion model.
- It does not establish whether the unresolved RTC/display issues have since been fixed.

These gaps are preserved rather than filled with external Linux power-management knowledge.

## Bounded sufficiency for Pass 1

For the initial structural and linguistic reconstruction, the recommended route, acceptance criteria, failure entries, explicit pattern references, relevant kernel-resource correspondences and three unresolved items have been recovered. The title/body coverage gaps for watchdog and thermal management are also recorded.

The remaining questions require device evidence, implementation evidence, author input or a separate technical-validation pass. They do not prevent moving to the next source document. Reopen an issue only when a new discriminating source, relevant observer-position change, translation conflict or concrete interpretive consequence becomes available.

No charging, PMIC, RTC, chrony, suspend, clock, watchdog, thermal or real-device claim has been technically validated in this pass.


---

## Pass 3 — Technical and semantic review

### Review position and evidence

- Pass: Pass 3 — technical and semantic review of the operational English translation set.
- Source: `experience/power-and-time.md`, `main`, blob `d74ba3b4bc320f2036f185e7614d8cdc1a703c6b`.
- English target baseline: `translation/en/experience/power-and-time.md`, `translation/english-reconstruction`, blob `5be75ae67a9876c72208a1dce9b7a58c8a130773`.
- Existing review baseline: `translation/reviews/experience/power-and-time.md.md`, blob `a5c0440ceb361a69403d9057de430f4dbee3642f`.
- Pass 3 criteria: `reconstruction/zh/continuation/checkpoints/pass-3-checkpoint-01.md`, blob `0870d3ea261a6354649379b96891cda4ca2a0ba6`.
- Direct display dependency: source `experience/display.md`, blob `6cd5016457d4cbab87fae56a4dd5e3c1204bbbdd`; English `translation/en/experience/display.md`, blob `ed9ec9b19090dd5907e132569545082b94c43d27`.
- Governing translation instructions: `translation/README.md`, blob `f9bf34ae28e838ae881f9e207797f26c1b327baa`.
- Glossary: `translation/GLOSSARY.md`, blob `7cd456b51223fd2cd3cf408cb14bae7f7d167254`.
- Current position at execution: Pass 3 / Checkpoint 17; next document `experience/power-and-time.md`. The position record had advanced beyond the historical SHA stated in the task packet, but its current document position agrees with this task. Checkpoint 17 records wireless completion and the same next document.
- External technical validation and real-device testing: not performed.

### 1. Imperative force

The target preserves the source's recommendations, restrictions and required sequencing: read-only fuel-gauge inspection before writes; read-only charging client before control; defer suspend until the named prerequisites are complete; use vendor temperature zones and thresholds; and refuse suspend when charging-control handoff fails. The first release may disable suspend provided it is recorded as technical debt. The target preserves that permission and condition.

### 2. Conditional scope

Applicability qualifications are retained, including Qualcomm, particular vendor charging implementations, some fuel gauges, some vendor Android systems, and the stated 2026 context. Examples remain examples rather than universal thresholds. The title's watchdog coverage gap and the limited thermal-management treatment remain explicit; no missing procedure or acceptance criterion has been invented.

### 3. Decision logic

The separate routes and evidence requirements remain distinct. In particular, a charging flag or charger classification does not establish net charging; current, voltage, bus voltage and battery-level trend remain the evidence. PMIC RTC read-only behaviour, SDAM offset storage and boot-time saved-time restoration plus NTP are not collapsed into one mechanism. Deep suspend and s2idle retain different wake behaviours and measurement implications. Charging-client handoff and fuel-gauge polling remain separate suspend concerns.

### 4. Technical tokens

Commands, paths, kernel interfaces, configuration properties, clock identifiers, units, driver names and error strings were compared. The target retains the relevant tokens, including `bq27xxx`, `qcom_battmgr`, `pmic_glink`, `CLOCK_BOOTTIME`, `CLOCK_MONOTONIC`, `IRQF_NO_SUSPEND`, `pm_wakeup_irq`, `OnCalendar`, `WakeSystem=yes`, `OnBootSec`, `mem_sleep_default=s2idle`, `SuspendState=freeze`, `prepare/complete`, and the chrony options.

One material numerical-language discrepancy was corrected. The source says SPMI arbitration rejects writes `每秒十几次` (more than ten times per second / in the teens per second); the baseline translation said “dozens per second”, which overstates the frequency. The target now says “more than ten times per second”.

### 5. Structure

The recommended route, completion criteria, pitfall sequence, applicability statements and unresolved section remain in the source's order. The target does not add missing diagnostic or prevention fields to entries that do not contain them in the source. The watchdog and thermal asymmetry is preserved.

### 6. Cross-document references

The source's unresolved black-screen-after-wake item refers to `display.md`. The English target retains “see display.md”; within the English experience directory this identifies `translation/en/experience/display.md`, the intended counterpart. The source and English display dependency were both fetched at the task-specified revisions. No additional document was needed to assess this direct reference.

### 7. Uncertainty

The target retains the three unresolved source issues: Linux net charging and fast charging requiring private authentication and negotiation; the unexplained RTC return to 1970 after abnormal reset; and black display after suspend/resume. It also retains the underspecified average-power/remaining-time issue and the lack of detailed watchdog and thermal procedures. No external technical knowledge was used to fill these gaps.

### Disposition

One source-justified correction was made to the English target: “dozens per second” was corrected to “more than ten times per second”. No other material source-to-target discrepancy requiring a change was identified. The existing Pass 1 review above is preserved unchanged.

No PMIC, charging, RTC, chrony, suspend, clock, watchdog, thermal or real-device claim has been externally validated.
