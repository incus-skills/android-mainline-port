# Ubuntu Userspace

The recommended overall architecture and package split are described in `../references/ubuntu.md`. This document records specific pitfalls only. Written against Ubuntu 26.04 + GNOME 49/50.

## Completion Criteria

- Native APT can install, upgrade and remove all project packages; no package is left half-configured in dpkg.
- Fresh installation: GNOME Initial Setup has correct scaling and usable touch input; after the wizard finishes, `systemctl --failed` is empty.
- After a cold boot, no service times out while waiting for a device.

## Pitfalls

### flash-kernel Reports Unsupported Platform and Leaves dracut Half-configured

Tag: **[Frequent]**

- **Symptom:** installing a kernel or updating initramfs fails; `dpkg --configure -a` fails.
- **Cause:** the Debian-family boot deployment tool does not recognize the device whose boot images it is supposed to manage.
- **Resolution:** set `/etc/flash-kernel/machine` to `none` (ship this in the device-support package); the project supplies its own hook to generate boot images.
- **Prevention:** disable the distribution's boot deployment tool before installing any packages for the first time.
- **Applicability:** Ubuntu/Debian ARM64; 2026.

### A Kernel Hook Fails When a Prerequisite File Is Missing, Leaving dpkg Half-configured

- **Symptom:** after the user selects a keyboard layout in Initial Setup, the system rebuilds initramfs. The project hook fails because a mapping file is not yet present, leaving dracut half-configured.
- **Cause:** the hook assumes all files are already in place; any package update can trigger an initramfs rebuild.
- **Resolution:** if prerequisite files are missing, the hook prints a notice and exits 0. It must not write a slot or delete an image set. Order installation steps around the rule that rebuilding initramfs at any point must be harmless.
- **Applicability:** Ubuntu; 2026.

### dracut Builds initramfs on the Build Machine and Includes the Build Machine's Hardware

- **Cause:** with `hostonly=yes`, dracut selects modules according to the current execution environment (QEMU or chroot).
- **Resolution:** set `hostonly="no"` and `hostonly_cmdline="no"`; explicitly list the required modules and filesystems. The trade-off is that initramfs includes all modules, making it slower to generate and larger. Check available space on the boot partition.
- **Applicability:** dracut; 2026.

### A Generic Kernel Is Installed Again

- **Symptom:** after `apt upgrade`, a distribution generic kernel appears and interferes with the boot-image hook.
- **Cause:** the official image includes `linux-generic`, an HWE metapackage, headers and `kdump-tools`.
- **Resolution:** purge these packages during the build. Order: install distribution updates and project userspace packages first; purge the generic kernel packages next; install the project kernel last. This generates initramfs only once.
- **Applicability:** Ubuntu; 2026.

### A Reused Kernel Version Name Causes the New Kernel to Load Old Modules

- **Symptoms:** one version name refers to two different builds; `linux-version sort` orders versions containing `+` incorrectly; a debugging RAM image does not match the installed module directory.
- **Resolution:** increment the ABI number for every package build (`<version>-<project><N>`); never reuse a version name and do not use a `+` suffix. Maintain an ABI ledger containing the version name, kernel commit and configuration hash.
- **Applicability:** General; 2026.

### APT Automatically Removes the Project Kernel Package

- **Cause:** APT protects only kernel packages whose names begin with prefixes such as `linux`.
- **Resolution:** name the project kernel package using the `linux-image-<release>` convention, or add its exact package name to `APT::NeverAutoRemove`.
- **Applicability:** Ubuntu; 2026.

### Snap Applications Cannot Use the GPU, or Fail to Launch After Connecting a Custom GPU Library

Tag: **[Time-consuming]**

- **Symptom 1:** snap applications such as Firefox, Thunderbird and the app store use software rendering. Strictly confined snaps obtain their OpenGL userspace from the `mesa-2404` provider of the `gpu-2404` interface. Its Mesa may not recognize a new GPU, and host-built Mesa cannot enter the snap sandbox.
- **Symptom 2:** after connecting these snaps to a custom GPU provider, Thunderbird and the app store fail to launch (missing libraries such as `libXfixes`). These snaps were packaged with libraries such as X11 and libxml2 removed, expecting `mesa-2404` to supply them; the custom provider contains only Mesa.
- **Resolution:** base the custom provider on the complete contents of the original `mesa-2404`, replacing only the Mesa libraries. Test loading against the actual dependencies of consumer snaps. A first-boot service installs the provider and connects every snap with a `gpu-2404` plug. When applications migrate to core26, the interface becomes `gpu-2604`, requiring another provider.
- **Prevention:** offline validation must simulate consumer loading, not check only the provider's own dependencies.
- **Applicability:** Ubuntu snap; 2026.

### Removing the Firefox Snap from an Image Cannot Be Done by Deleting Its Seed Entry

- **Cause:** the official Desktop image has snap applications preseeded. `state.json` records pending tasks; deleting only the seed entry leaves a task whose file is missing. Running `snap remove` in the build VM causes snapd to time out and roll back the whole operation.
- **Resolution:** install the deb version of Firefox from Mozilla's official APT repository (repository, key and pin). After first boot, a one-shot service waits for seeding to finish, then runs `snap remove --purge firefox`; it must first confirm that the deb version exists. Update the dock favourite to point to the deb version.
- **Applicability:** Ubuntu 22.04 and later; 2026.

### Snap Mount Fails or snap-confine Lacks Capabilities

- **Cause:** the kernel lacks SquashFS XZ/LZO/xattr support; extracting squashfs as an ordinary user loses extended attributes and device nodes; `security.capability` is lost.
- **Resolution:** enable the required kernel configuration; extract as root with a tool that preserves xattrs; inspect capability bits on the actual files.
- **Applicability:** Ubuntu; 2026.

### Results from Testing Snap Behaviour in QEMU Do Not Generalize to Real Hardware

- **Cause:** under TCG, snapd operations often time out and roll back transactions.
- **Resolution:** reserve snap acceptance testing for real hardware. Set time limits for AppArmor/snapd-related investigations.
- **Applicability:** General.

### GNOME Initial Setup Uses 100% Scaling and the Text Is Too Small

- **Cause:** the wizard runs under a temporary account without a persistent configuration directory, so it cannot read display settings stored in a user's home directory. When borrowing the firmware display, physical dimensions are unavailable and Mutter selects 100%.
- **Resolution:** use the system-wide configuration `/etc/xdg/monitors.xml`, which is read by GDM, Initial Setup and user sessions. The fundamental fix is for the display driver to provide physical dimensions.
- **Applicability:** GNOME; 2026.

### Initial Setup Writes an Invalid Keyboard Layout and console-setup Fails

- **Symptom:** after the wizard, `console-setup.service` fails and the system is degraded.
- **Cause:** the wizard writes `XKBLAYOUT="default,us"`.
- **Resolution:** use a drop-in to clean the invalid value before `console-setup` runs.
- **Applicability:** Ubuntu 26.04; 2026.

### Initial Setup Overwrites the Hostname

- **Cause:** the wizard always writes the username as the computer name.
- **Resolution:** set a pretty hostname (use it as the Bluetooth name); document that users can change the computer name in the wizard.
- **Applicability:** GNOME; 2026.

### The System Language Remains English

- **Cause:** the wizard sets only the user's language.
- **Resolution:** when needed, configure `/etc/default/locale` and preinstall language packs.
- **Applicability:** Ubuntu; 2026.

### Power-button Behaviour Does Not Match Tablet Expectations

- **Symptoms:** a short press opens a shutdown dialog and the system shuts down automatically after 60 seconds; or the default action is suspend even though suspend is not implemented.
- **Resolution:** while suspend is incomplete, set GSettings `power-button-action='nothing'` or retain the dialog (user choice), and set logind `HandlePowerKey=ignore`. Once suspend works, change the action to “turn off the display and lock”.
- **Applicability:** GNOME; 2026.

### A Service Cannot Access the User D-Bus

- **Cause:** `ProtectHome=yes` also hides `/run/user`.
- **Resolution:** use `ProtectHome=read-only` and separately allow access to `/run/user`.
- **Applicability:** systemd.

### A Socket Binds to a Network Interface That Does Not Yet Exist at Boot, Delaying Boot by 90 Seconds

Tag: **[Fatal]**

- **Symptom:** boot takes 90 seconds longer, SSH does not listen, and the display manager is also delayed.
- **Cause:** the debugging SSH socket uses `BindToDevice=usb0`. The interface is created by the debugging service, while that service waits for the boot target containing the socket. They wait on each other until timeout. Testing a role switch while the system is already running never triggers this condition.
- **Resolution:** bind to a fixed address and add `FreeBind=yes`; use `IPAddressAllow`/`IPAddressDeny` to accept only the USB subnet.
- **Prevention:** after changing units, sockets, udev, initramfs or generators, verify with a cold boot. First reproduce the issue using the previous version as a control.
- **Applicability:** systemd; 2026.

### The zram Generator Adds Another 90-Second Boot Delay on a Kernel Without zram

- **Resolution:** enable zram in the kernel before installing the generator configuration, or check for support during the build.
- **Applicability:** Ubuntu; 2026.

### A Service Is Not Re-enabled on the Upgrade Path

- **Cause:** package migration removes the enabled symlink. `deb-systemd-helper enable` treats this as “the user disabled it” and does not recreate it.
- **Resolution:** reproduce the real upgrade offline using the package tools from the same version installed on the device. Before changing packages related to the debugging channel, confirm a rollback route.
- **Applicability:** Debian packaging; 2026.

### Build-machine APT Lists Remain in the Image, Causing the User's First apt install to Fail

- **Resolution:** clean `/var/lib/apt/lists` at the end of the build; document that the user must first run `sudo apt update`.
- **Applicability:** General.

### The User Thinks Home Is Only 60 GB

- **Cause:** GNOME's About page displays only the root partition.
- **Resolution:** explain this in the documentation.
- **Applicability:** GNOME; 2026.

### An Old Script in /usr/local/sbin Shadows the New Packaged Version

- **Resolution:** do not manually place scripts on the device; deliver everything through packages.
- **Applicability:** General.

### A Manually Assembled Root Filesystem Is Missing Files

- **Symptoms:** the touch-firmware directory is missing; a missing dconf profile disables scaling, the on-screen keyboard and idle policy; mdev changes `/dev/null` permissions to 0660, breaking D-Bus, logind and GDM.
- **Resolution:** make the device layer a build artifact (a package) with a manifest and installation self-check. Validate configuration through the program that actually reads it.
- **Applicability:** General.

### The GDM Session Reads a Different dconf Configuration Than Expected

- **Cause:** the GDM session's `XDG_CONFIG_HOME` points to `/var/lib/gdm3/seat0/config`.
- **Resolution:** when reading GSettings, reproduce the target process's environment.
- **Applicability:** GNOME; 2026.

### The On-screen Keyboard Has a Null-pointer Failure on the Lock Screen

- **Resolution:** use a GNOME Shell extension with a guard; test once without a physical keyboard.
- **Applicability:** GNOME 49; 2026.

### NTFS3 Filenames Are Garbled

- **Cause:** the default character set is ISO-8859-1; reading and writing with the same driver does not reveal the round-trip problem.
- **Resolution:** specify `iocharset=utf8` in the mount options.
- **Applicability:** General.

### Keys Do Not Respond for Several Seconds After Locking the Screen

- **Symptom:** after pressing the power button to lock and suspend, the system occasionally takes about 30 seconds to actually suspend; keys are ignored in the meantime.
- **Cause:** the sleep delay inhibitor lock held by gnome-shell is occasionally not released. logind waits until `InhibitDelayMaxSec` forces suspend; unattended-upgrades has configured this limit to 30 seconds. The specific lock that remains held has not been identified.
- **Confirmation:** passively collect `ListInhibitors` and the Shell main-thread wait point; measure sleep duration using wall-clock time.
- **Resolution:** identify the stale lock and fix it in upstream or distribution code. Do not mask it by reducing `InhibitDelayMaxSec`.
- **Applicability:** GNOME 50, Ubuntu 26.04; 2026.

### The App Store Displays Chinese as Squares

- **Cause:** the snap-store version shipped in the image has no system-font fallback in its ARM64 Flutter engine; upstream has fixed this.
- **Resolution:** run `snap refresh snap-store` to obtain the fixed version.
- **Prevention:** when a distribution-shipped application has a problem, first check whether upstream has fixed it in a newer version before considering a local modification.
- **Applicability:** Ubuntu 26.04 ARM64; 2026.

### Package Dependencies Tie Basic Device Functions to a Specific Desktop Version

- **Symptom:** the device-support package depends on a display-power package, which hard-depends on a particular version of gnome-settings-daemon. As a result, basic functions such as dual-system switching and wireless addresses also depend on GNOME.
- **Resolution:** basic device packages must not depend on desktop components; put desktop integration in a separate package.
- **Applicability:** Packaging design; 2026.

### A Type=oneshot Service Runs Repeatedly During Boot and Hits the Start Limit

- **Symptom:** cold-boot acceptance reports a failed service; that service ran 4–6 times during one boot.
- **Cause:** the service is attached to `sysinit.target` but lacks `RemainAfterExit=yes`. It runs again whenever another service pulls in a Wants dependency on sysinit, eventually triggering `StartLimitBurst`. Example: Ubuntu's `fake-hwclock-load.service`, which also resets the clock to its saved value on every run.
- **Resolution:** add `RemainAfterExit=yes` in a drop-in.
- **Prevention:** when acceptance reports a failed service, first count its “Starting” occurrences across the boot before deciding whether the latest change caused it.
- **Applicability:** systemd; 2026.

### Do Not Fork Core Components to Change GNOME Behaviour

- **Approach:** do not fork core packages such as Mutter, gnome-shell, gnome-settings-daemon, GTK or GStreamer. If necessary, apply a minimal patch to the distribution source package (versioned as `+<project>1`), pin its source and record an exit condition (remove the patch after upstream merges it).
- **Applicability:** Ubuntu; 2026.
