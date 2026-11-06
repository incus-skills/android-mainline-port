# Wi-Fi and Bluetooth

## Recommended Route

**Wi-Fi (Qualcomm WCN series, PCIe with ath11k/ath12k)**

1. **Read-only vendor evidence:** record PCI subsystem IDs, firmware file/version combinations, board-data source, MAC source, regulatory region, and commands issued by the vendor driver at startup (especially the transmit-power limit).
2. **Power sequencing:** inspect the power-sequencing node (qcom,wcnXXXX-pmu). Confirm each supply individually through the reference relationships in the vendor device tree. The PCIe PHY supply is usually the hardest to establish.
3. **PCIe:** use the built-in PCIe controller and PHY. Preserve normal IOMMU isolation; do not use iommu.passthrough=1.
4. **Driver patches:** prefer upstream and community patches (for example, new-chip definitions, QDSS and power control). Import them commit by commit and record their provenance.
5. **Firmware and identity:** use the complete firmware set corresponding to one version. Read the MAC from the device's own data partition and set it before NetworkManager starts.
6. **Transmit power:** apply the transmit-power limit in the same way as the vendor implementation.

**Bluetooth (the same combo chip, UART with hci_qca)**

1. Enable the UART node and its parent serial-engine wrapper. Establish which PMIC owns the power-sequencing and enable GPIO.
2. Match firmware to the vendor version and include it in the firmware package. If it is needed early, include it in the initramfs.
3. Read the address from the device's own data partition and write it before bluetooth.service starts. Do not hard-code it in the device tree.

## Completion Criteria

- **Wi-Fi, layer by layer:** PCIe link → firmware log shows a complete board match → network interface appears → scan → association → DHCP, DNS and ping → throughput → no disconnections over a period of time → reconnection → stable MAC. For 5 GHz, verify actual frequencies in the 4.9–5.9 GHz range and test wide 80 MHz channels. Seeing an access point during scanning does not mean the device can connect to it or use the network.
- **Bluetooth:** after a cold boot, the controller becomes available automatically, has the correct address, discovers devices, pairs, produces headphone audio or accepts keyboard/mouse input, reconnects, and remains usable after suspend/resume. hci0 up does not constitute a pass.

## Pitfalls

### 5 GHz / 80 MHz Association Disconnects About Six Seconds Later

Tag: **[Time-consuming]**

- **Symptoms:** Authentication and key exchange succeed every time. About six seconds later, beacons are lost, the transmit queue cannot drain, and the local connection is dropped; DHCP never obtains an address. GNOME reports this misleadingly as an incorrect password. 2.4 GHz works; limiting the channel to 40 MHz works.
- **Cause:** Above approximately 20 dBm transmit power, the link freezes. The vendor driver applies a SAR power limit at every startup; the mainline driver does not.
- **Confirmation:** Within the same boot, compare a fixed 20 dBm setting with automatic power. Note that limiting the channel to 40 MHz also disables HE, so test channel width and HE as separate variables.
- **Resolution:** Apply the same SAR limit during driver initialisation as the vendor implementation. Do not treat a fixed 20 dBm value as the production solution, and do not copy another device's board data: it contains that device's calibration.
- **Prevention:** When the symptom is "can scan but cannot connect", compare the vendor driver's initialisation command sequence before changing firmware or board files.
- **Applicability:** ath12k + WCN78xx; 2026.

### 5 GHz Scan Returns No Results

Two distinct cases are recorded.

**Case 1 — Regulatory-region frequency calculation**

- **Symptoms:** 2.4 GHz networks are visible, but no 5 GHz networks appear.
- **Cause:** When the regulatory region has no 6 GHz rules, the driver's band-calculation code reduces a single-radio device's available bands to 2.4 GHz only.
- **Resolution:** Fix the frequency-range calculation; a community patch exists.

**Case 2 — Incompatible board data and firmware**

- **Symptoms:** After changing only the board file, the firmware crashes near the end of download and the network interface disappears.
- **Cause:** Board data is incompatible with the main firmware version. The driver first searches board-2.bin using the full key; once it finds a match, it does not read board.bin. Therefore, an experiment that changes only board.bin has no effect.
- **Resolution:** Select the board using the factory selection table and replace the complete firmware-version combination. Use bdencoder to replace data for the exact key only.

- **Prevention:** For every firmware experiment, confirm in the log that the complete key matched; otherwise the result cannot be attributed.
- **Applicability:** ath11k/ath12k; 2026.

### Wi-Fi Firmware Fails to Load with Normal IOMMU Isolation

- **Symptoms:** The PCIe device encounters DMA faults and the firmware does not start.
- **Cause:** After iommu-map was changed to two cells, the PCIe controller driver silently parsed config_sid incorrectly and wrote the wrong stream ID. Passthrough mode concealed the problem.
- **Resolution:** Fix the driver's parsing; an upstream fix exists. Do not use passthrough mode to hide the defect.
- **Applicability:** Qualcomm PCIe; around mainline 7.2; 2026.

### No Device-Specific Evidence for the PCIe PHY Supply; Investigation Remains Blocked

Tag: **[Time-consuming]**

- **Symptoms:** Link training fails, and the vendor device tree does not directly name this supply.
- **Resolution:** Infer candidate supplies from reference relationships in the vendor device tree and reference devices. After collecting read-only evidence (including the vendor PCIe state), make one single-variable decision on the real device. Note that this supply may be shared with UFS or the USB PHY; do not change its voltage casually.
- **Applicability:** Qualcomm PCIe.

### Reloading the Wi-Fi Driver During One Boot Crashes Firmware or the Kernel

- **Symptoms:** After rmmod/insmod, the firmware fails on its second power-up; a subsequent recovery failure can lead to a kernel panic when scanning is attempted.
- **Resolution:** Do not reload the driver during one boot. Refuse scans while recovery is in progress (a community patch exists). Reboot when an experiment requires changing the module.
- **Applicability:** ath12k.

### Wi-Fi MAC Is All Zeros or Changes Randomly on Every Boot

- **Cause:** No MAC address is programmed into the network device; the vendor system reads it from the data partition.
- **Resolution:** Extract it read-only from the device's own data partition and set it through the device tree or udev before NetworkManager starts. Fall back to a random address if it is unavailable. The MAC is device-unique data and must not be included in public images.
- **Applicability:** General.

### Bluetooth Controller Does Not Appear or Firmware Request Fails

- **Symptoms:** No HCI device appears; firmware request returns -2.
- **Cause:** The parent serial-engine wrapper is not enabled, or the firmware is not in a location accessible at the time it is requested (for example, it exists only in the root filesystem while the driver requests it during the initramfs stage).
- **Resolution:** Enable the wrapper; put the firmware in the initramfs or defer loading.
- **Applicability:** Qualcomm; 2026.

### Bluetooth Has Persistent TX Timeouts

Tag: **[Time-consuming]**

- **Cause:** The enable GPIO points to a PMIC other than the one actually used, despite the PMICs having the same model.
- **Resolution:** Check the PMIC and GPIO number against the vendor device tree.
- **Applicability:** Platforms with multiple PMICs.

### Bluetooth Address Configuration Works Manually but Fails as a systemd Service

Tag: **[Time-consuming]**

- **Symptoms:** The address-configuration tool cannot see a configurable controller for 30 seconds when run as a service, but succeeds manually within 10 seconds.
- **Cause:** btmgmt sends the command-line command to standard input for processing. A service has /dev/null as standard input; epoll_ctl returns EPERM and the process remains stuck indefinitely. An earlier investigation incorrectly attributed this to startup ordering and rebuilt the orchestration around that mistaken diagnosis.
- **Confirmation:** First compare the execution environments (standard input/output, namespaces, capabilities, cgroup and all systemctl show properties), then use strace.
- **Resolution:** Give it an empty pipe as standard input, for example true | timeout 2s btmgmt ...
- **Applicability:** BlueZ 5.8x; 2026.

### Bluetooth Controller Is in the Unconfigured State

- **Cause:** A Qualcomm controller without a public address appears as unconfigured.
- **Resolution:** Read the address from the device's own data partition and write it with btmgmt public-addr. The temporary index is not necessarily 0.
- **Applicability:** hci_qca.

### Bluetooth LE Keyboard and Mouse Connect but Produce No Input

- **Cause:** The kernel lacks UHID.
- **Applicability:** General.

### Wi-Fi Country Code and Regulatory Domain Have Wide-Ranging Effects

Tag: **[Frequent] [Time-consuming]**

- **Symptoms:** Setting the country code removes 5 GHz networks; the country code does not take effect at boot; changing it during association causes a WLAN SMMU fault, firmware crash, or even a driver Oops while holding RTNL that deadlocks networking on the whole device.
- **Cause (combined factors):** An early failure to load regulatory.db queues subsequent country hints; after a country is set, firmware follows the country advertised by the connected router through 802.11d; in some countries (for example, those without 6 GHz rules), the single-radio band calculation removes 5 GHz; changing the country code at runtime triggers driver and firmware defects.
- **Resolution:** Keep a fixed default consistent with the vendor system; do not change the country code at runtime. Use the community fix for single-radio frequency-range calculation. If a feature is complex and its benefit is small, remove it.
- **Applicability:** ath12k + WCN78xx; 2026.

### Wi-Fi Fails to Recover After Suspend/Resume

- **Cause:** PCIe retains its link during s2idle, but the driver turns off the associated power domain.
- **Confirmation:** Use kprobe to trace the power-domain and PCIe suspend callbacks.
- **Resolution:** Reuse the community awake path and add the corresponding power-domain flag for the SoC.
- **Applicability:** Qualcomm PCIe + ath12k; 2026.

### The Bluetooth Test Peer Has a Kernel Defect of Its Own

- **Symptoms:** After reconnection, l2ping fails and the packet capture shows an invalid credits value.
- **Cause:** The peer computer used for testing has an old ERTM credits defect in its kernel.
- **Resolution:** Capture btsnoop on both ends. Use a peer with a fixed kernel or another device; do not attribute the failure from the tablet-side log alone.
- **Applicability:** BlueZ testing method; 2026.

### Controller Cannot Be Used When the Device Tree Has No Bluetooth Address

- **Resolution:** Use an MGMT helper program to write the address from the device's own data during first boot (a community implementation exists). If an address is already configured, do not change it.
- **Applicability:** hci_qca.

## Unresolved

- Bluetooth initialisation reports -84 frame reassembly on every attempt; the cause has not been identified.
- The vendor system performs an approximately eight-second radio calibration at boot; mainline has no corresponding implementation.
- Bluetooth pairing, file transfer and audio work, but after repeated reconnections the ACL return path stalls. Responsibility has not been attributed to either side.
