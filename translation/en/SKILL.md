---
name: android-mainline-port
description: A guide and experience library for porting mainline Linux and desktop distributions to Android phones and tablets (Qualcomm + Ubuntu GNOME practice). Covers assessment, building, bootloader, device trees, display, touch, audio, wireless, charging, suspend, dual-boot, packaging, testing and release. Use when the user wants to run native Linux on an Android device, port a mainline kernel, boot a custom kernel with fastboot, troubleshoot peripherals that do not work under mainline, or assess whether a device can be ported.
---

# Porting Mainline Linux to Android Devices

## How this skill is organised

- **Outer framework** (`SKILL.md` and `references/`): what to do first and what to do next, working discipline, technical selection, and how to determine whether something has passed. Relatively stable.
- **Experience library** (`experience/`): recommended approaches, completion criteria, and specific pitfalls organised by domain (symptoms, causes, how to confirm, how to resolve, and how to prevent). Continuously extended as projects progress.

The experience comes from porting Qualcomm tablets to Ubuntu 26.04 + GNOME. The hardware and kernel sections are broadly applicable. When changing distributions, adjust the Ubuntu-specific sections according to the final section of `references/ubuntu.md`. Version numbers and interface names are time-sensitive; check their current upstream status before use. Where upstream has already solved a problem, use the simpler upstream solution directly.

## How to use this skill

- **New project:** Follow the process below from Stage 0. For each stage, first read the corresponding section of `references/process.md`, then read the relevant files in `experience/`.
- **Taking over an existing project:** First read the project's own `AGENTS.md`, status table and technical-debt table, then identify the current stage in the process.
- **Making a technical decision:** First read the default approach and rules in the relevant section of `references/decisions.md`.
- **Troubleshooting:** First use `experience/patterns.md` to classify the problem and follow the general troubleshooting procedure. Then use error codes, function names and configuration items from the logs to search (`grep`) for specific cases in `experience/`.
- **Assessment only:** Complete Stage 0 and produce an assessment report and decision list.

## Process

| Stage | Activities | Completion criteria | Main references |
| --- | --- | --- | --- |
| 0 — Project scoping | Set up the workspace; establish a research pool (upstream, same-SoC community, vendor source code, similar projects); investigate reuse; inventory hardware; select approaches; estimate effort; confirm decisions with the user | User has confirmed the decision list; do not touch the device before confirmation | `references/process.md`, `references/decisions.md`, `references/discipline.md` |
| 1 — Build environment | Toolchain, ccache, persistent OUT directory, root-filesystem build method; build an unmodified community reference kernel once | Reference kernel can be reproduced; incremental builds take only a few minutes | `references/build-environment.md` |
| 2 — Evidence acquisition and backup | Root access; back up and verify the confirmed scope; extract the runtime device tree, firmware, PMIC voltages, display parameters and vendor command sequences; prepare recovery | Hashes of both backup copies match; recovery images are available | `references/process.md`, `experience/usb-and-storage.md` |
| 3 — First boot | Understand the bootloader offline; create a minimal device tree; add timestamp probes; use a read-only initramfs + USB networking + timeout fallback | Host can connect to a shell; automatic fallback to the original system works on timeout | `experience/bootloader-and-boot.md`, `experience/kernel-and-resources.md` |
| 4 — First display and RAM desktop | Select a display route and start native takeover in parallel; configure touch, GPU, RAM root filesystem and GNOME; prepare desktop automation and acceptance scripts | A person can see the display and operate it by touch; hardware indicators pass | `experience/display.md`, `experience/touch-and-input.md`, `experience/gpu.md` |
| 5 — Installation to storage | Maintain images; partition; use tar and on-device formatting; manage boot-image sets and active-slot rotation; provide switching tools; keep the debug channel disabled by default but available to enable | Cold-boot budget passes; switching between Linux and Android passes on a real device | `experience/usb-and-storage.md`, `references/ubuntu.md` |
| 6 — Peripherals | Work through one peripheral at a time, in priority order: inspect reuse → compare with vendor implementation → independent test image → real-device test → merge → cold-boot regression | Each item has evidence of its actual result | Relevant domain files in `experience/` |
| 7 — Release | Feature freeze; layered packaging; verify a clean installation from scratch using the public documentation; first-hour audit; review external documentation | Others can build, install and use the result by following the documentation | `references/ubuntu.md`, `experience/build-and-release.md` |
| 8 — Maintenance | Remove local implementations when upstream incorporates the functionality; pay down technical debt according to impact; perform cold-boot verification after each update | — | `references/discipline.md` |

### The most costly sequencing mistakes

The following ordering mistakes have the highest cost, typically losing several days each time:

1. **Research similar projects before designing.** Projects for the same SoC or similar devices often already have mature approaches and post-mortems covering installation, switching, debug channels and packaging structure.
2. **Establish the debug channel and automatic recovery before conducting any experiments.** Do not touch peripheral registers on the default boot path before the channel has been established.
3. **Acquire evidence before writing the device tree.** Every value must come from evidence obtained from the original implementation on this device; do not copy values from reference devices.
4. **Use borrowed firmware display output only for the first display.** Native display takeover starts in parallel from Stage 4. Brightness, screen-off, suspend and external display all depend on it.
5. **Make the desktop persistent as soon as it runs in RAM.** Do not wait until all peripherals are finished. Persistence is required for cold-boot verification and also speeds up iteration.
6. **Test new functionality independently before merging.** Do not put multiple changes that have not been verified on a real device into one candidate.
7. **Install packages on the device during routine iteration.** Build the complete root filesystem only for releases.
8. **Before release, perform a complete clean installation by following the public documentation.**

## Working discipline (mandatory)

See `references/discipline.md` for details.

1. **Documentation must be written at fixed points in the workflow.** Before starting a feature, document its approach and completion criteria. Before testing on the device, document expected outcomes and decision criteria; on the same day after testing, record results and evidence locations. When conclusions change, update the status table and work log. Before ending a session, record the single next step, blockers and device state. The next person or agent taking over must be able to continue by reading the documentation alone.
2. **Temporary approaches must be registered as technical debt.** Temporary approaches may be used to get the first display working or to make something function quickly, but each one must be recorded in a **single unified technical-debt table**: what was done, where it is located, why it is needed, its impact, and the conditions for removing it. Pay down this debt collectively once the full chain works.
3. **Keep the directory structure fixed; each piece of code must exist in only one place.** Kernel repositories and integration repositories are public. The private workspace holds records and internal scripts. Unique local data and passwords must not enter any repository.
4. **Every conclusion must include version information.** Record source-code commits, image hashes, test scenarios and evidence locations. After changing the kernel, system or driver, re-verify old conclusions. Do not delete failure records.
5. **Distinguish "it runs" from "it is usable".** A driver loading, a service running, a device node existing, Fastboot returning `OKAY`, or an emulator test passing only proves that one particular layer is working. See `references/verification.md` for the standards.
6. **Every step must be reversible.** Risk increases progressively from read-only operations → booting from memory → writing partitions → changing bootloader state. Before moving to a higher-risk step, confirm that a recovery path is available. Change A/B slot state only through the bootloader's own commands.
7. **Change only one variable at a time; stop when the same failure occurs for the second time.** Trace the earliest failing layer, starting from the physical layer and moving upward. Record the hypotheses already ruled out and the next experiment capable of falsifying a hypothesis.
8. **Do not use global switches to conceal problems.** Disabling resource reclamation, masking modules, using fixed delays, and retrying unconditionally all push problems into a later and more difficult stage of investigation.
9. **Changes to the boot path require cold-boot verification.** Changes to units, sockets, udev, initramfs or boot parameters must be verified through cold boot and comparison. Switching while the system is running does not count.
10. **Only one operator may control a device at any given time.** Include the serial number with every command. Check battery level before leaving Android. Do not issue destructive commands to a device whose state is uncertain. Always retain a debug channel that can be enabled in official release images.
11. **Automate testing as much as possible and minimise user involvement.** Scripts should handle device operations, data collection, assessment and recovery. Screenshot capture and input injection should be automated through the debug channel. The user should only perform actions that require a person, such as looking, listening, pressing, positioning, plugging in or unplugging. Consolidate these into one checklist, notify the user in advance, and complete them in one session. Use read-only measurement instead of asking questions whenever possible.
12. **Proceed by version.** Define the scope and acceptance method for each version first; then develop, test, release and clean up before continuing. Do not accumulate unreleased candidates.
13. **Prioritise efficiency.** Time any step taking more than five minutes, identify its bottleneck, and cache based on its inputs. Use a persistent OUT directory and sufficiently large ccache. See `references/build-environment.md`.

## Technical selection principles

Evaluate every important choice using the same set of questions, and record the rationale and conditions for reassessment. See `references/decisions.md` for the default approaches and conditional rules for approximately 40 key technical choices: "what to do when a particular situation occurs".

- Stay as close to upstream as possible: upstream > patch under review > same-SoC community > vendor code > write it yourself.
- Prefer reversibility: do not change bootloader state, do not touch critical vendor partitions, and ensure failures can recover automatically.
- Prefer evidence from real devices over claims of support. Other people's conclusions are only leads until verified on this device.
- Reuse complete sets where possible. Before reuse, compare the underlying foundations on both sides (device-tree approach, resource management and installation method).
- Minimise the amount of code that must be maintained locally. Do not introduce general-purpose frameworks or permanently running "manager" services.
- Keep the hardware layer independent of the distribution and desktop environment. Changing distributions should only require changing the integration layer.
- A temporary solution may be used initially, but work on the long-term solution must begin at the same time, and the temporary solution must be registered as technical debt.

## Decisions requiring the user's input

After assessment, present all items together. For each item, provide options, trade-offs and a recommendation. Confirm them with the user using the available question mechanism. Once confirmed, record them in `AGENTS.md`; do not ask again:

1. Whether data may be erased, and which partitions should be backed up.
2. Single-system or dual-system installation, and whether Android should be retained.
3. Partition sizes (provide 2–4 concrete options; a 60 GiB Linux root partition is usually sufficient, and placing `home` last makes expansion easier).
4. Distribution and desktop environment.
5. Whether the debug channel should be enabled or disabled by default, and whether an account should be preconfigured.
6. Accounts and repository names for release, licence, and whether original vendor firmware may be distributed with the repository.
7. Estimated workload, major risks and any missing prerequisites.
8. Which operations may be performed when the user is not present.

## File index

| File | Contents |
| --- | --- |
| `references/process.md` | Detailed actions, outputs and completion criteria for each stage |
| `references/discipline.md` | What documentation must record, technical debt, directory structure, attribution, estimation and sub-agents |
| `references/build-environment.md` | Host, toolchain, ccache, OUT, root-filesystem build, caching and avoidable overhead |
| `references/decisions.md` | Approximately 40 key technical decisions: defaults, conditions and approaches, rationale, and cases where they do not apply |
| `references/verification.md` | Status terminology, evidence, cold boot and comparison, test automation and performance budgets |
| `references/ubuntu.md` | Ubuntu layering and decoupling, package separation recommendations, kernel packages, boot hooks, dracut, snap, GNOME defaults and distribution changes |
| `experience/patterns.md` | Nine problem patterns: how to recognise them, general handling and common manifestations |
| `experience/README.md` | Experience-library file list, entry format and maintenance rules |
| `experience/*.md` | Recommended approaches, completion criteria and pitfalls across 13 domains |

## Maintaining this skill

After completing a project or solving a new problem, write the experience back to the relevant file in `experience/` (see `experience/README.md` for the format). Change the outer framework only when the process or working discipline changes. Do not include device models, serial numbers, personal information or internal project codenames.

### Experience development and upstream contribution

Treat experience development as a recurring lifecycle:

**Use → encounter → record → accumulate → validate → select → translate/adapt → propose upstream → review/incorporate → use again.**

Use of the skill may produce observations and candidate experiences. Capture and retain those candidates in this English project with enough context, provenance, evidence and applicability to make them reconstructable. The English project is the accumulating home for experiences generated through its own use; local capture does not depend on an upstream contribution being selected, submitted or accepted.

Consolidate each candidate against the existing experience library before adding anything: extend an existing entry where it describes the same problem, create an entry where a distinct confirmed problem warrants one, or retain unresolved material in the relevant file's `Unresolved` section. Follow the library's entry and maintenance rules. An observation is not automatically a confirmed cause or reusable recommendation; validate it against the evidence available, preserve the conditions and limits, and do not invent a universal validation threshold.

Only select sufficiently supported, reusable improvements for possible upstream contribution. Translate and adapt selected material to the upstream project's language and context while retaining its evidence, provenance, conditions and applicability. A pull request is a proposal for upstream review, not proof of acceptance or incorporation. Keep the English project's experience and records regardless of whether upstream accepts, rejects, delays or does not respond. When upstream incorporates a change, record the relevant upstream version or commit in the experience entry as appropriate; subsequent use may confirm, refine or challenge the experience. Ordinary library additions do not by themselves change the outer framework.
