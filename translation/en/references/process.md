# Detailed Process

For each stage, specify: **goal, activities (in order), outputs, completion criteria, and relevant experience**. The sequence itself reflects accumulated experience: most major losses of time come from skipping steps or doing them out of order.

## Overview

```text
Stage 0  Project scoping: workspace, research pool, reuse investigation, assessment, user confirmation
Stage 1  Build environment: toolchain, caches, reproducible community baseline
Stage 2  Stock-device evidence and backup: root access, backups, obtain the "ground truth"
Stage 3  First boot: understand the bootloader, run a shell and debugging channel in memory
Stage 4  First display and RAM desktop: display, touch, RAM root, GNOME
Stage 5  Install to storage: partitioning, root filesystem deployment, dual boot, debugging channel, cold boot
Stage 6  Adapt peripherals individually: prioritised, one at a time
Stage 7  Package, validate a clean installation, release
Stage 8  Maintenance: keep up with upstream, pay down technical debt
```

**Parallelism principles:**
- Stage 1 can run in parallel with the latter part of Stage 0.
- Native display takeover can begin alongside other work from Stage 4.
- Offline preparation in Stage 6 can run in parallel, but real-device testing must be limited to one peripheral at a time.

---

## Stage 0: Project Scoping

**Goal:** Establish whether the project is feasible, how it should be approached, the approximate time required, and which decisions require user input. Do not touch the device.

1. **Create the workspace:** Following `discipline.md`, create a private workspace, a kernel repository and an integration repository. Write an initial `AGENTS.md`. Agree with the user on commit author, accounts, repository names and licences.
2. **Verify device identity:** Record the model, exact SoC model (including suffix), whether the bootloader can be unlocked, stock OS version, storage layout and A/B configuration.
3. **Build a research pool:** Retrieve and pin versions, recording their URLs and commits:
   - Upstream kernel support for the SoC, including DTSI, display, GPU, wireless, audio and remote processors, as well as patch series under review on lore/patchew.
   - Community kernels and userspace projects for the same SoC (for example, postmarketOS, ROCKNIX and individually maintained branches).
   - Vendor open-source kernel: inventory its drivers on the first day (display, touch, keyboard, amplifier, charging, etc.) and create a "stock-source comparison table". Do not reverse-engineer what source code already explains.
   - Stock firmware packages, with hashes verified.
   - Existing Linux projects for comparable devices: first read their installation instructions, installers, retrospectives and technical-debt records.
4. **Create a reuse-investigation table:** One row per feature, recording upstream status, community status (working on a real device or code only), vendor code, gaps and recommended source.
5. **Create a hardware inventory:** Panel, touch, GPU, wireless chip, amplifier and codec, fuel gauge, backlight, PMIC, sensors, keyboard bridge, camera and fingerprint sensor.
6. **Select technical approaches:** Choose the main approaches according to `decisions.md`, recording the reasons.
7. **Estimate the workload:** Use the method in `discipline.md` to provide ranges and identify the largest sources of uncertainty.
8. **Confirm decisions with the user:** Present all decisions requiring user input together (see `SKILL.md`) and use the available question mechanism to obtain confirmation. Record the confirmed decisions in `AGENTS.md`.

**Outputs:** Workspace, research-pool inventory, reuse-investigation table, hardware inventory, technical-selection record, assessment report and `AGENTS.md`.

**Completion criteria:** The user has confirmed the decision list.

**Relevant experience:** `../experience/testing-and-process.md` (failure to examine comparable projects first; compare the foundations before reusing community work).

---

## Stage 1: Build Environment

**Goal:** Be able to build the kernel and root filesystem quickly and reproducibly.

1. Install the toolchain, configure ccache, and plan the OUT directory and disk capacity according to `build-environment.md`.
2. **Build the kernel for a community reference device without modifications** to prove that the toolchain and baseline work. Subsequent failures in new code can then be attributed more confidently.
3. Write the kernel build script: pin inputs, use a persistent OUT directory, use `flock`, and record build identity.
4. Decide how to build the root filesystem (native ARM64 CI is preferred) and get one build working.
5. Add timing to every step that takes more than five minutes.

**Completion criteria:** The reference kernel can be reproduced; one incremental build takes only a few minutes; the root filesystem can be built.

**Relevant experience:** `../experience/build-and-release.md`.

---

## Stage 2: Stock-Device Evidence and Backup

**Goal:** Obtain recovery capability and all available "ground truth". All subsequent persistent writes depend on this.

1. **Connect read-only:** Verify the serial number, model, unlock state, current slot and battery level. Fastboot may not appear if the cable is faulty.
2. **Obtain root:** Either root the stock OS directly (Magisk or KernelSU modifying `init_boot`), or temporarily `fastboot boot` a recovery image with root access. Do not try to assemble minimal probes on the stock kernel.
3. **Back up:** Back up partitions within the scope confirmed by the user (normally excluding `userdata` and `super`). Separately save the primary and backup GPTs for each LUN. Calculate hashes on both host and device; the backup is complete only when they match. Record an inventory (device names can change; interpret them by referring to the inventory from then on).
4. **Collect evidence** while the stock OS is running:
   - Runtime device tree at `/sys/firmware/fdt`.
   - Boot partition image headers, vbmeta information, `fastboot getvar all`, and read-only diagnostic commands supported by the bootloader.
   - Firmware file inventory and hashes.
   - Actual PMIC voltage-register values and the always-on power supply list.
   - Display: clocks, topology, DSC parameters and panel commands from the stock driver's debugfs (do not scan with `/dev/mem`).
   - Command sequences in stock logs: touch firmware download, amplifier initialisation, wireless driver initialisation (including power limits), and charging negotiation.
   - Locations of device-unique data: calibration, MAC address, Bluetooth address and sensor configuration. Record locations only; put the contents in `private/`.
5. **Rehearse recovery:** Confirm the recovery image, the button combination for entering Fastboot, and the recovery procedure.

**Outputs:** Verified backups and their inventory, evidence files, and recovery instructions.

**Completion criteria:** Backup hashes match; the recovery image is available.

**Relevant experience:** `../experience/usb-and-storage.md` (backups and device names), `../experience/bootloader-and-boot.md` (stock-system probes).

---

## Stage 3: First Boot

**Goal:** Get the mainline kernel running in memory, establish a shell accessible from the host, and automatically return to the original OS on timeout.

1. **Understand the bootloader offline:** Image format, device-tree overlays (libufdt and a positive control), command-line source and length, root `compatible` rewriting, and hypervisor nodes.
2. **Create a minimal board DTS:** PMIC, storage, USB (device mode), buttons, ramoops and simple-framebuffer (if using the borrowed-display route). All values must come from the evidence collected in Stage 2.
3. **Inject the symbol table into a disabled container during packaging.**
4. **Create a minimal initramfs:** Read-only block devices with verification, USB NCM, DHCP, telnet/SSH, timed exit and a charging-mode branch. Do not touch peripheral registers before the debugging channel is established.
5. **Run timestamp probes on the device:** Prove handover, runtime duration and automatic recovery.
6. **Boot with a shell and debugging channel.**
7. **Complete the first-boot checklist:** Root `compatible`, `systemd-detect-virt`, final `.config`, `devices_deferred` and `sync_state` status.

**Completion criteria:** Commands work in both directions; `dmesg` can be retrieved; timeout returns to the original OS; the boot format is documented.

**Relevant experience:** `../experience/bootloader-and-boot.md`, `../experience/kernel-and-resources.md`.

---

## Stage 4: First Display and RAM Desktop

**Goal:** Run a complete desktop in memory, with a visible display and working touch input.

1. **Choose the display route** (see `../experience/display.md`): use the native route if possible; otherwise borrow the existing display and declare the resources used by the firmware in the device tree, with the provider built in.
2. **Begin native takeover in parallel:** Even when initially borrowing the display, start preparing native display support here (golden snapshots, panel driver and modular loading).
3. **Touch:** Integrate TDDI into the panel power sequence from the outset.
4. **RAM root:** Official root SquashFS + OverlayFS, transferred over HTTP via USB networking.
5. **Desktop:** GDM and GNOME session; temporary automatic login is permitted only at this stage.
6. **Create desktop automation tools:** Screenshots and input injection (see `verification.md`), so subsequent desktop tests no longer depend on the user.
7. **Add acceptance-budget scripts:** Measure time, bandwidth and display hardware metrics across multiple cold boots.

**Completion criteria:** A person confirms the display visually and touch operation physically; hardware metrics pass; timeout returns to the original OS.

**Relevant experience:** `../experience/display.md`, `../experience/touch-and-input.md`, `../experience/kernel-and-resources.md`.

---

## Stage 5: Install to Storage

**Goal:** Achieve a persistent installation, usable cold boots and working switching between the two operating systems. Start this once the desktop works in memory; do not wait until all peripherals are complete.

1. **Confirm the partitioning scheme** (already selected by the user in Stage 0).
2. **Create a maintenance image:** Runs in memory; includes storage drivers, a root shell and partitioning tools; does not mount partitions; has no session timeout that automatically returns to Fastboot.
3. **Write the partition table:** Use the maintenance image, back up the GPT first, read it back after writing, and run `sgdisk --verify`.
4. **Deploy the root filesystem:** Format and extract the tar archive on the device; pre-populate device-unique data (calibration and addresses) so that touch and Bluetooth work on first boot.
5. **Create the boot-image set:** Generate it according to the format tested on the actual device, write it to the active slot and read it back for verification. Preserve both the Android image set and the previous Linux image set.
6. **Create in-system switching tools** and an Android-side switching entry point (KernelSU module), plus a service that marks the booted slot as successful.
7. **Debugging channel:** Disabled by default and explicitly enabled when needed. Enable it and confirm a recovery path before the first flash to a test device.
8. **Cold-boot acceptance testing** and one complete Linux → Android → Linux switching cycle.
9. **Kernel update path:** Install a new kernel package on the device; a hook rebuilds the boot-image set and retains a rollback set. Use this path for routine iteration rather than reflashing the root filesystem.

**Completion criteria:** Cold-boot budget passes; bidirectional switching passes on a real device; the debugging channel works after a cold boot when enabled and is genuinely unavailable when disabled.

**Relevant experience:** `../experience/usb-and-storage.md`, `../experience/ubuntu-userspace.md`, `ubuntu.md`.

---

## Stage 6: Adapt Peripherals Individually

**Goal:** Bring peripherals to a usable, everyday standard in priority order.

Suggested order (adjust to user needs):

Wi-Fi → audio (speakers and microphone) → battery and charging → buttons and screen-off → GPU acceleration → Bluetooth → native display (brightness and screen-off) → automatic rotation → keyboard accessories → suspend → hardware video decoding → USB host and external display → camera, etc.

**Per-peripheral cycle:**

1. Read the recommended approach and tagged entries in `../experience/<domain>.md`.
2. Investigate reuse: upstream, community and vendor source code; record the sources.
3. Compare against the stock-device evidence and identify the earliest layer at which something could fail, together with the criteria for detecting it.
4. Build offline and create an **independent** RAM test image, changing only this one peripheral. When testing a candidate kernel on an installed system, do not install the package directly (its hook writes to the slot). Instead, stage the modules using the method in `../experience/bootloader-and-boot.md`, generate an initramfs on the device, and boot it in RAM.
5. Test on a real device: use machine-checkable criteria plus physical confirmation where necessary. Locate failures by layer. If a similar failure occurs for the second time, stop and write an analysis.
6. Once it passes, merge it into the mainline version, increment the ABI for packaging, install it on the device and run cold-boot regression tests.
7. Write a device-side self-test and add it to the regression suite run after every build.
8. Register temporary approaches used to get things working quickly as technical debt.

**Completion criteria:** Every peripheral has evidence of its actual result and passes cold-boot regression testing.

---

## Stage 7: Package, Validate a Clean Installation, and Release

1. **Feature freeze:** Do not add new features before release.
2. **Package:** Split packages by layer according to `ubuntu.md`. Native APT installation, upgrade and removal must all pass.
3. **Build release artifacts:** Build from a clean commit with pinned inputs; encode only used blocks or use tar; keep attachments minimal.
4. **Validate a clean installation:** Follow the public documentation strictly and perform a fresh installation on a real device, including the GNOME first-run setup wizard (the user must complete it using touch). Then perform cold-boot acceptance testing and bidirectional switching. Incorrect or missing documentation steps are problems this test must uncover.
5. **Audit the ordinary user's first hour:** Language, keyboard, timezone, scaling, networking, browser, volume, brightness, power button, hostname and APT availability.
6. **Review external documentation:** The primary agent must personally review the README, installation and upgrade documentation, and release notes. Remove internal codenames, experiment IDs and excessive hashes. The support table must list only features that have passed on a real device. Mark anything not included in the first release as "unsupported". Put an "Ask AI to help you install" prompt on the front page. State the risks clearly (including possible bricking and data loss).
7. **Release:** Push, tag, upload attachments, and verify attachment names, sizes and checksums. Ask the user before pushing and making anything public.

**Completion criteria:** Someone else can build, install and use the result by following the documentation.

**Relevant experience:** `../experience/build-and-release.md`, `../experience/ubuntu-userspace.md`.

---

## Version Rhythm

Stages 6 and 7 repeat for each version. For every version, first define its scope in the roadmap document and specify the acceptance method for each item. Once development is complete, carry out testing in a concentrated pass (put anything requiring user participation last), finish and release, then clean up branches, worktrees and old artifacts before starting the next version.

Do not keep accumulating new features while a version remains unreleased.

## Stage 8: Maintenance

- When upstream incorporates a feature, switch to the upstream version and remove the local implementation.
- Review the technical-debt table regularly and pay down debt in order of impact.
- Perform cold-boot verification after every kernel or boot-related update.
- Record newly encountered pitfalls in this skill's `experience/` directory (if you are maintaining this skill).
