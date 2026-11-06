# Battery, Charging, Suspend, System Time, Watchdog and Thermal Management

## Recommended Route

1. **Read the fuel gauge without writing:** use an upstream I2C fuel-gauge driver (for example, bq27xxx), or ADSP battmgr through pmic_glink. First confirm that readings agree with the vendor system.
2. **Charging status and control:** generic qcom_battmgr data layouts and current-sign conventions may differ across SoC generations. Vendor-private channels (for example, Xiaomi MCA or OnePlus charging services) use GLINK. Start with a read-only client; defer writes, and apply allowlists and bounds to writable values.
3. **Fast charging:** private protocols (for example, Xiaomi MiPPS) require a userspace authentication daemon and 9 V negotiation. Prefer reusing a community implementation.
4. **Suspend:** prerequisites are complete power management for native display, touch and keyboard, plus wireless recovery from suspend. The first release may disable suspend; record this in technical debt.
5. **Time:** the PMIC RTC is usually read-only from Linux. The root fix is to store an offset in PMIC SDAM; a transitional approach restores the last saved time at boot and then uses NTP.
6. **Thermal management:** use only the vendor temperature zones and thresholds.

## Completion Criteria

- **Battery:** readings differ from the vendor system's readings at the same time by an acceptable amount (for example, ±8% or ±300 mV).
- **Charging:** connect a wall charger for 15–30 minutes; charge level rises (for example, by at least 3%), current remains positive, and battery temperature is normal. A status flag alone does not count.
- **Plug/unplug:** measure latency separately at the kernel sysfs and UPower layers.
- **Suspend:** the successful-suspend counter increases; CLOCK_BOOTTIME and CLOCK_MONOTONIC diverge; the boot ID remains unchanged; display and touch work after resume.
- **Time:** no PMIC write is rejected during one boot; after reboot and switching operating systems, time differs by no more than seconds; time synchronises after networking becomes available.

## Pitfalls

### Low Battery or Insufficient Computer USB Power Masquerades as a Software Failure

Tag: **[Frequent] [Time-consuming]**

- **Symptoms:** black screen, touch failure or random resets; identical code behaves differently between attempts; the device repeatedly drains after automatic boot while connected to power.
- **Cause:** the backlight boost converter and panel bias draw directly from the battery and may fail to start at very low charge. A computer USB port supplies only about 400–500 mA; under desktop load the device may still discharge. Linux and Fastboot usually cannot achieve net charging.
- **Resolution:** read a fresh battery level before leaving Android. Do not start experiments below a threshold (for example, 40%); use a wall charger. When identical code produces different results, check battery level and voltage first.
- **Applicability:** General.

### Charging Icon Remains After Unplugging or Does Not Refresh After Plugging In

- **Symptoms:** the desktop still shows charging or full after unplugging; plugging into computer USB does not change the icon.
- **Cause:** plug detection itself may be working normally (kernel and UPower can both report within one second). At full charge, the gauge may report Full and retain that state until charge falls. Computer USB may result in net discharge, which UPower reports accurately.
- **Confirmation:** log kernel online/status and UPower state every second during a controlled plug/unplug test.
- **Resolution:** identify which layer is responsible before changing anything; do not change the driver based on a subjective impression of delay.
- **Applicability:** General.

### Original Fast Charger Supplies Almost No Current at 5 V

- **Symptoms:** the charger is recognised as PD at 5 V, but firmware limits input current to about 100 mA.
- **Cause:** the private fast-charge protocol requires authentication and high-voltage negotiation; firmware does not permit normal current at 5 V.
- **Resolution:** use the community authentication and 9 V negotiation implementation. A 5 V experiment can establish only whether the write took effect.
- **Applicability:** Xiaomi and similar private fast-charge implementations; 2026.

### Private Charging Authentication Daemon Exits Non-zero with a Non-original Charger, Leaving the System Degraded

- **Resolution:** exit normally with status 0 when the charger is not an original supported charger.
- **Applicability:** Charging-authentication daemon; 2026.

### Treating a Charging Flag as Proof That the Battery Is Charging

- **Symptoms:** forcing an authenticated flag displays fast charging but current does not change; charging type oscillates between two values.
- **Resolution:** use current, voltage, bus voltage and battery-level trends as evidence.
- **Applicability:** General.

### battmgr Protocol Is Incompatible with a New SoC, or Current Sign Is Reversed

- **Cause:** data layout changes between generations; on some generations battery-current sign is reversed; pmic_glink may wait for a status notification that the device's firmware never publishes.
- **Resolution:** use generation-specific property tables and a sign quirk; do not wait for the absent notification; or use the vendor-private channel directly.
- **Applicability:** Qualcomm; 2026.

### First Linux Write to a PMIC Register Disrupts an Entire Test Run When Bundled into a Large Change

- **Symptoms:** after RTC writes are enabled, SPMI arbitration rejects writes repeatedly (more than ten times per second), preventing other functions from being tested in that run.
- **Resolution:** make the first write to a PMIC or security-sensitive register a separate, single-variable experiment from a RAM-booted system.
- **Applicability:** Qualcomm; general method.

### System Time Is Wrong and Does Not Synchronise Even When Networking Is Available

Tag: **[Frequent]**

- **Symptoms:** every boot returns to an old date; time remains wrong after networking starts; HTTPS and package repositories report certificate errors.
- **Combined causes:** the PMIC RTC is read-only; chrony normally requires NTS authentication, but an NTS certificate is judged not yet valid when the system clock is wrong; many routers intercept NTP requests and return unauthenticated time.
- **Resolution:** restore the last saved time at boot (for example, with fake-hwclock; its oneshot service must remain active or it may run repeatedly). Configure chrony with nocerttimecheck and authselectmode ignore, accepting the cost of unauthenticated time. The root fix is an SDAM offset; first confirm read-only in the vendor system that the relevant SDAM unit is unused.
- **Prevention:** use host time as the reference for logs spanning operating systems.
- **Applicability:** Qualcomm + Ubuntu chrony; 2026.

### Deep Suspend Immediately Wakes and No Wake Source Is Visible

- **Symptoms:** pm_wakeup_irq is empty.
- **Cause:** IPCC/glink interrupts use IRQF_NO_SUSPEND and skip wake configuration, so they do not appear in wake records. Battery-management notifications and sensor streams can wake the system; in deep suspend, any such message causes a full resume.
- **Resolution:** add temporary tracing at the IPCC and glink layers to identify the source; stop sensor streams before suspend; avoid entering suspend during the post-boot notification period; progress through pm_test levels before testing real suspend.
- **Prevention:** schedule automatic resume with OnCalendar and WakeSystem=yes; OnBootSec does not count time spent suspended.
- **Applicability:** Qualcomm; 2026.

### Average Power and Remaining-Time Readings Are Unreliable

- **Applicability:** Some fuel gauges; 2026.

### The Fuel Gauge's I2C Bus Is Shared by Multiple Execution Environments

- **Cause:** another processor accesses the same bus, while mainline has no lock coordinating the accesses.
- **Resolution:** confirm the vendor system's access pattern before reading; keep access read-only and reduce polling frequency.
- **Applicability:** Qualcomm; 2026.

### Using a Monotonic Clock to Measure Suspend Duration Falsely Reports an Immediate Wake

Tag: **[Frequent]**

- **Symptoms:** the one or two seconds between PM: suspend entry and suspend exit in dmesg are interpreted as an immediate wake; or a tool reads only the first segment and reports an early wake.
- **Cause:** CLOCK_MONOTONIC (including dmesg/journal monotonic timestamps and wakeup last_change_ms) does not advance during suspend. In s2idle, non-wakeup interrupts can briefly resume the CPU before it suspends again, splitting one suspend interval into multiple Timekeeping suspended segments.
- **Resolution:** measure elapsed suspend time using wall-clock time or BOOTTIME − MONOTONIC; for s2idle, sum the segments and record their count. logind may not record a power-key press that occurs during suspend.
- **Applicability:** General; 2026.

### deep or s2idle?

- **Observed behaviour:** on Qualcomm platforms, ADSP IPCC/glink messages fully wake deep suspend; s2idle instead exhibits a cycle of briefly waking and suspending again.
- **Current default in the source:** use s2idle (mem_sleep_default=s2idle, SuspendState=freeze); compare deep and s2idle power consumption separately.
- **Applicability:** Qualcomm.

### Charging Control Must Be Handed Off During System Suspend

- **Cause:** a private charging client stops polling during suspend, leaving the charging chip with an unsafe or stale setting.
- **Resolution:** use the standard prepare/complete callbacks. Hand control back to a conservative state before suspend and refuse suspend if handoff fails. A charging-chip shutdown error must not be reported as success.
- **Applicability:** Private charging channels (for example, MCA with a direct-charging chip); 2026.

### Fuel-Gauge Polling Triggers Warnings During Suspend

- **Resolution:** stop polling in the fuel-gauge driver during suspend and resume polling afterward.
- **Applicability:** bq27xxx and similar devices.

### Charging from a Computer USB Port Under Android Stops After Several Hours

- **Symptoms:** while waiting for charge before testing, the battery stops gaining charge after several hours and the PMIC reports charging disabled.
- **Resolution:** use a wall charger, or use the vendor-provided charging switch to re-enable charging. Periodically read fresh values while waiting; do not assume charging continues.
- **Applicability:** Some vendor Android systems; 2026.

## Unresolved

- Net charging and fast charging under Linux require private authentication and negotiation; the first release usually cannot close this gap.
- The cause of the RTC returning to 1970 after an abnormal reset is unknown.
- The display remains black after waking from suspend (see display.md).
