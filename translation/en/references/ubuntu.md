# Ubuntu Integration and Decoupling

The experience in this skill comes from Ubuntu 26.04 LTS + GNOME (Wayland, GDM, dracut and snap). The following is the recommended approach: the structure to which actual projects ultimately converged. Adjust the degree of separation and the names to fit the actual project.

## 1. Goal: The Distribution Must Be Replaceable

```text
┌─────────────────────────────────────────────┐
│ Desktop policy layer: GNOME defaults,       │  ← rewrite when changing
│ scaling, power button, first-boot services, │     desktop or distribution
│ browser replacement                         │
├─────────────────────────────────────────────┤
│ Integration layer: boot-image hooks,        │  ← change hook locations
│ dracut configuration, disabling            │     when changing distribution
│ flash-kernel, switching tools               │
├─────────────────────────────────────────────┤
│ Device layer: udev, device services, UCM    │  ← mostly unchanged
│ and topology, sensor stack, charging        │
│ authentication, importing local data        │
├─────────────────────────────────────────────┤
│ Hardware layer: kernel Image, device tree,  │  ← unchanged
│ modules, firmware                           │
├─────────────────────────────────────────────┤
│ Distribution: official root filesystem,     │
│ unmodified except for added packages        │
└─────────────────────────────────────────────┘
```

Rules:

- Hardware-related components must not depend on specific package-manager behaviour, a display manager, a fixed account or a desktop.
- Do not modify distribution files. If replacement is necessary, use `dpkg-divert`; uninstalling the package must restore the original.
- Device-unique data (calibration, MAC address, Bluetooth address and sensor configuration) must not go into any package. Import it during installation from the device's data partition; report an explicit error if it is missing.
- Do not create one “master manager” package or permanently running service to control everything.

## 2. Package-Splitting Recommendations

Split packages according to “who maintains it, how often it is updated, and which layer it belongs to”. Replace `<dev>` with the device identifier.

| Package | Contents | Layer | Notes |
|---|---|---|---|
| `linux-image-<version>-<dev><ABI>` | `/boot/vmlinuz-<release>`, `/lib/modules/<release>/` (complete module directory), device tree under `/usr/lib/linux-image-<release>/` | Hardware | Follow upstream `scripts/package/builddeb` layout. The name must begin with `linux-image` so APT protects it from automatic removal |
| `<dev>-kernel` | Depends only on the current kernel package | Hardware | Upgrades and rollback change only this package |
| `<dev>-firmware` | Firmware files under `/lib/firmware/updates/`, with a per-file manifest (source, path and hash) | Hardware | Do not compete with `linux-firmware` for paths; resolve conflicts with `dpkg-divert` |
| `<dev>-device-support` | udev rules, device services (userspace touch, charging, slot-success marking), boot-image hooks, dracut modules and configuration, `/etc/flash-kernel/machine`, switching tools, debugging channel (disabled by default), polkit rules | Device + integration | Small package; changes most frequently |
| `<dev>-audio` (may be merged into device-support) | UCM and ALSA topology files | Device | |
| `<dev>-sensors` | Remote-processor RPC daemon, sensor libraries, replacement iio-sensor-proxy (`dpkg-divert`) | Device | Remove when supported by the distribution version |
| `<dev>-mesa` | Mesa required by a new GPU, installed in a separate prefix and enabled only when the GPU driver has loaded | Device | Remove when supported by the distribution's Mesa |
| `<dev>-snap-graphics` | `gpu-2404` provider snap and auto-connect service | Desktop | Needed only for snap-based distributions |
| `<dev>-desktop` | GNOME defaults (gschema override), scaling in `/etc/xdg/monitors.xml`, power-button behaviour, chrony and fake-hwclock configuration, zram, PipeWire upmixing, console-setup correction, browser replacement | Desktop policy | Rewrite when changing desktop |
| `<dev>-debug` | Diagnostic tools and debug shell | Optional | Not installed by default |
| `<dev>-device` | Metapackage depending on the packages above | — | The user only needs to install this |

At minimum, keep **kernel, firmware and desktop policy** separate: their update frequencies and ownership are entirely different.

## 3. Kernel Package

- Version name: `<upstream-version>-<dev><ABI>`, for example `7.2.6-foo3`. **Increment the ABI for every packaged build**, including builds that change only the device tree or toolchain. Never reuse or roll back an ABI. When `MODVERSIONS` is disabled, module ABI breakage cannot be detected; reusing a version name can cause an old module to be loaded by a new kernel.
- Maintain an ABI ledger: version name, kernel commit and `.config` hash. The packaging script packages only combinations recorded in the ledger.
- Do not use a `+` suffix (`linux-version sort` orders it incorrectly); disable `LOCALVERSION_AUTO`.
- Restrict the matching pattern when the packaging script searches for version tags, to prevent project release tags from being matched first.

## 4. Boot-Image Hooks

Android-device boot images must be generated by the project itself.

- Place hooks in `/etc/kernel/postinst.d/zz-<dev>-boot`, `/etc/kernel/postrm.d/` and `/etc/initramfs/post-update.d/`. When the kernel or initramfs changes, generate a new boot-image set in the device's native format, save it to the switching storage and (optionally) write it to the active slot and read it back for verification.
- Retain the previous version as a rollback set.
- If prerequisites (signing keys, partition mappings, etc.) are missing, hooks must print a message and **exit 0** without writing a slot or deleting an image set. Any package update may trigger initramfs regeneration (for example, when the user changes the keyboard layout in the first-run setup wizard); a hook failure can leave dpkg half-configured.
- Hooks must recognise when they are running inside a build chroot and must not write to the device in that case.
- Set `none` in `/etc/flash-kernel/machine` to disable the distribution's built-in boot-deployment tool.

## 5. initramfs (dracut)

```ini
# /etc/dracut.conf.d/90-<dev>.conf
hostonly="no"            # In QEMU/chroot builds, hostonly selects modules for the build host
hostonly_cmdline="no"
add_dracutmodules+=" <dev> "   # Device logic needed early (for example, hypervisor-node masking)
filesystems+=" ext4 "
```

- Drivers required for boot and recovery must be built into the kernel or included in initramfs. Firmware needed early (for example, touch and display) must be included in initramfs.
- `hostonly="no"` packages all modules, making the image large and generation slow. Check the available space in the boot partition (for example, vendor_boot). Solve slow generation by generating on the device; see `build-environment.md`.
- Note that Ubuntu's dracut uses 3cpio; the compression-level parameter may not take effect.

An alternative is to write a custom BusyBox initramfs (more control for early debugging and recovery, but it must be maintained yourself); see `decisions.md`.

## 6. Root-Filesystem Composition

1. SquashFS layer from the official ARM64 Desktop ISO (minimal + standard), with a fixed URL and hash.
2. Install distribution updates and project packages (such as `<dev>-device`).
3. Purge the generic kernel, HWE metapackage, headers and `kdump-tools`; then install the project kernel so initramfs is generated only once.
4. Install the Firefox deb from the Mozilla APT repository (add the repository, key and pin). A first-boot service removes the snap version after snap seeding has completed.
5. Use PARTLABEL in fstab; do not pre-create an account (use GNOME Initial Setup); set the system's default language according to the target user.
6. Clean `/var/lib/apt/lists` and build-host traces; run `fstrim` before delivery (tar or sparse image encoding only used blocks).

## 7. Align Kernel Configuration with the Distribution

Before the first release, compare the kernel configuration with Ubuntu's generic kernel and add at least: `UHID`, `PSI`, zram, the complete `nf_tables` set, WireGuard, Yama, Landlock, NTFS3, CIFS, ISO9660, dm-crypt, `SND_SEQUENCER`, `FW_LOADER_COMPRESS`, SquashFS XZ/LZO/xattr, FUSE and AppArmor (required by snap). Submit each item separately to make rollback easy.

## 8. snap and Graphics

- GPU userspace for strictly sandboxed snaps comes from a provider implementing the `gpu-2404` interface, not from system Mesa. When a new GPU is not supported by `mesa-2404`: use the complete original `mesa-2404` contents as the base, replace only the Mesa libraries, and build a provider snap. A first-boot service installs it and connects all snaps with `gpu-2404` plugs. Offline validation must simulate the consumer loading it. Snaps on core26 use `gpu-2604`.
- Prefer the browser deb (which uses system Mesa directly).

## 9. GNOME Defaults

See the detailed entries in `../experience/ubuntu-userspace.md`. Key points:

- Put scaling in `/etc/xdg/monitors.xml` (also read by Initial Setup and GDM).
- Power button: while suspend is incomplete, set it to do nothing or retain the dialog, and configure logind to ignore it; once suspend is complete, change it to screen-off and lock.
- When suspend is unavailable, disable suspend (`sleep.conf.d`) and set the idle action to none.
- Set Chassis to tablet and mask the hypervisor node.
- Time: fake-hwclock + relaxed chrony configuration (transitional).
- PipeWire multi-speaker upmixing.
- Clean up invalid keyboard layouts written by Initial Setup.

## 10. Debugging Channel (Product)

- Use socket-activated SSH through systemd, bound to a fixed USB-subnet address with `FreeBind=yes`; use `IPAddressAllow`/`IPAddressDeny` to accept only the USB subnet. Do not use `BindToDevice`.
- Disabled by default. Enable temporarily during installation; explicitly enable it through the command line or installation parameters. Its enabled state persists across reboots.
- Verify both enabled and disabled states with cold boots.

## 11. Release and Upgrade

- Release attachments: boot-image package, tools package (switching, recovery and installation), software packages, root-filesystem volumes, checksums and manifest.
- User upgrade: download the new packages and run `apt install ./*.deb`; hooks automatically rebuild the boot-image set and retain the rollback set. An APT repository can be provided later.
- Installation documentation must explain importing local data, first-run setup, toggling the debugging channel, returning to Android and the risks.

## 12. What Must Change for Other Distributions

| Component | Ubuntu/Debian | Fedora | Arch | postmarketOS |
|---|---|---|---|---|
| Kernel and firmware | deb | rpm (contents unchanged) | pkg (contents unchanged) | apk (contents unchanged) |
| Boot-image hooks | `/etc/kernel/postinst.d` | `kernel-install` plugin | pacman hook | mkinitfs / boot-deploy |
| initramfs | dracut | dracut (native) | mkinitcpio or dracut | mkinitfs |
| Replacing distribution files | `dpkg-divert` | Avoid where possible; use drop-ins | Avoid where possible | Package override |
| GPU for sandboxed applications | snap `gpu-2404` | Flatpak GL extension | Flatpak | Primarily native packages |
| Desktop policy | GNOME gschema override | Same GNOME approach | According to desktop | Phosh/GNOME/Plasma Mobile |

The hardware and device layers need little or no change; rewrite the integration and desktop-policy layers according to the table above.
