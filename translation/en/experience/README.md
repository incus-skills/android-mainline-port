# Experience Library

This directory stores concrete experience by domain: recommended routes, completion criteria, and pitfalls encountered. The outer process and methodology (`SKILL.md`, `references/`) change infrequently; this library grows through additions from each new project.

All experience comes from the actual porting process for a Qualcomm tablet to **Ubuntu 26.04 + GNOME (Wayland)**, using mainline kernels 6.17 through 7.2. The hardware and kernel sections are largely distribution-independent. When changing distributions, the userspace sections must be checked and adapted; see the final section of `../references/ubuntu.md`.

## Files

Start with `patterns.md`: it abstracts the pitfalls in the files below into nine problem categories, with ways to recognise each category and general handling steps.

| File | Scope |
|---|---|
| `bootloader-and-boot.md` | Bootloader (Qualcomm ABL), boot image formats, first boot, debugging channel, early logs, recovery |
| `kernel-and-resources.md` | Built-in code and modules, resource reclamation and sync_state, PMIC voltages, device-tree values, serial engines |
| `display.md` | Borrowing the firmware display (simpledrm), native display takeover, DSC, screen off/on, backlight, night light |
| `gpu.md` | Adreno kernel support, firmware, self-built Mesa, Mesa defects on new GPUs |
| `touch-and-input.md` | Touch (including TDDI), buttons, keyboard accessories, stylus, fingerprint |
| `audio.md` | ADSP/AudioReach, amplifiers, microphones, UCM, PipeWire, volume |
| `wireless.md` | Wi-Fi (ath11k/ath12k), Bluetooth (hci_qca) |
| `power-and-time.md` | Battery, charging, suspend, RTC and system time, watchdog, thermal management |
| `sensors-camera-media.md` | Sensors and automatic rotation, camera, hardware video decoding |
| `usb-and-storage.md` | USB/Type-C/external display, UFS, partitioning, dual boot, flashing, installer |
| `ubuntu-userspace.md` | Ubuntu packaging, dracut, snap, GNOME first-run setup and desktop defaults |
| `build-and-release.md` | Build pipeline, caching, reproducibility, release |
| `testing-and-process.md` | Acceptance criteria, host scripts, automated testing, parallel collaboration, sub-agents, working with the user |

## How to use it

- When a new fault occurs: first use `patterns.md` to identify its category, then look for a specific entry in the corresponding file.
- Before starting a stage or subsystem: read the relevant file's "Recommended route" first, then review entries marked 【Frequent】, 【Time-consuming】, or 【Critical】.
- When investigating a fault: use distinctive terms from the logs (error codes, function names, configuration options) to `grep` this directory. The "Symptoms" field in entries is written, where possible, with text that can be searched directly.

## Entry format

```
### One sentence stating the symptom and cause
Markers: 【Frequent】 【Time-consuming】 【Critical】 (as applicable)
- Symptoms: Original log text, error codes, or what the user sees
- Cause:
- How to confirm: Fastest way to locate the problem
- How to fix:
- Prevention:
- Applicability: General / Qualcomm / a particular SoC generation / distribution version; year
```

This format follows the same reasoning as the rules in `../references/decisions.md`: "Symptoms" state the conditions, "How to fix" gives the action, "Cause" explains why, and "Applicability" makes clear when the rule does not hold. The entries describe reusable patterns, not the chronology of a particular incident.

- 【Frequent】: encountered in multiple projects.
- 【Time-consuming】: investigation took several days or consumed substantial real-device testing.
- 【Critical】: can prevent the device from booting, cause data loss, or sever the debugging connection.

## Maintenance rules

- For a new problem, first search for an entry of the same kind. If one exists, add to it; otherwise create a new entry in the most relevant file.
- Only add problems with a confirmed root cause to the "pitfalls" entries. Put unresolved problems in the "Unresolved" section at the end of the file, and state what has already been ruled out.
- Do not record device models, serial numbers, addresses, personal information, internal project codenames, or experiment IDs. Chip models, driver names, configuration options, and error codes may be recorded; they are the keywords needed to match similar problems.
- Do not delete problems that have been fixed upstream. Add a sentence at the end of the entry: "Fixed upstream: version or commit".
- When a recommended route changes, edit the "Recommended route" section directly and note in the superseded entry that it has been replaced.
