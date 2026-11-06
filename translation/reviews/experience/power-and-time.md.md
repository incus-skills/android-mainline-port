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
