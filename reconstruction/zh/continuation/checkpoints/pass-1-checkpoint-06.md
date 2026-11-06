# Pass 1 — Checkpoint 06: Wi-Fi and Bluetooth

## 1. Position and scope

- Traversal: **Pass 1 — initial structural and linguistic reconstruction**
- Previous checkpoint: [Pass 1 / Checkpoint 05](pass-1-checkpoint-05.md)
- Completed source: experience/wireless.md
- Source branch: main
- Source blob SHA: c8ae92e9d20207fd686398412da8df3e49216768
- English translation: translation/en/experience/wireless.md
- Translation review: translation/reviews/experience/wireless.md.md
- Chinese descriptive reconstruction: reconstruction/zh/wireless.md
- Next source: experience/power-and-time.md
- Technical validation: not performed

This checkpoint records movement within Pass 1. The declared purpose remains initial structural and linguistic reconstruction, not technical validation.

## 2. Reconstructed document role

The source is a Qualcomm Wi-Fi and Bluetooth bring-up experience record. Wi-Fi centres on WCN devices, PCIe and ath11k/ath12k; Bluetooth centres on the UART path and hci_qca for the same combo-chip context.

It does not define a unified wireless architecture. The two subsystems have some shared device-level context but retain separate buses, drivers, initialisation requirements and acceptance sequences.

## 3. Recovered routes

**Wi-Fi:** read-only vendor evidence (PCI identity, firmware/board combination, MAC source, regulatory region and initialisation commands); target-derived power sequencing; built-in PCIe controller/PHY with normal IOMMU isolation; provenance-preserving driver patch import; version-matched firmware set; device-local MAC before NetworkManager; vendor-equivalent transmit-power limit.

**Bluetooth:** enable UART and parent serial-engine wrapper; establish PMIC ownership of power sequencing and enable GPIO; place matching firmware where the requesting boot stage can access it; set the device-local address before bluetooth.service rather than hard-coding it in the device tree.

## 4. Completion and evidence boundaries

Wi-Fi acceptance progresses through PCIe link, complete firmware board match, interface appearance, scan, association, DHCP/DNS/ping, throughput, stability, reconnection and MAC stability. Actual 5 GHz frequencies and 80 MHz channels are required. Scan visibility does not establish connectivity.

Bluetooth acceptance progresses from automatic controller availability after cold boot and correct address through discovery, pairing, actual headphone or keyboard/mouse function, reconnection and suspend/resume. hci0 up is explicitly insufficient.

These are two distinct acceptance sequences. Their shared progression from lower-level availability to user-facing function is an observer correspondence, not a named source framework.

## 5. Explicit collection-level relationships

The wireless source itself contains no Markdown links to other experience documents. patterns.md explicitly lists wireless cases under:

- **Pattern C:** timing and lifecycle, including PCIe link retention with power-domain shutdown and the btmgmt standard-input issue.
- **Pattern F:** low-level success is not user usability, including scan visibility without connectivity and hci0 up without successful pairing.
- **Pattern G:** upstream/distribution fixes, including wireless band calculation and refusal to scan during recovery patches.
- **Pattern H:** another implementation cannot be copied wholesale, including board-data and device-specific calibration concerns.

These are explicit collection references. A case may exhibit more than one pattern dimension.

## 6. Observer-reconstructed correspondences

- **Identity/calibration provenance:** PCI identity, firmware version, board-data key and device-specific calibration jointly determine whether an experiment can be interpreted.
- **Target hardware evidence:** supplies, PMICs, GPIOs, PCIe PHY and serial-engine wrappers must be grounded in target vendor evidence; reference devices supply candidates, not copy-ready values.
- **Distinct lifecycle contexts:** driver reload, s2idle power-domain handling, service startup and controller initialisation have different recovery constraints.
- **Regulatory interactions:** regulatory.db, configured country code, AP-advertised 802.11d country and driver band calculation interact, but the source does not formalise a state model.
- **Endpoint attribution:** Bluetooth failures may require packet evidence from both peers.
- **User-facing acceptance:** scanning, HCI state and controller visibility are intermediate observations; network use and actual peripheral function remain separate.
- **Device-private identity:** MAC and Bluetooth address are recovered from local device data and must not be embedded in public images.

## 7. Unresolved and evidence boundaries

The source leaves three issues unresolved:
- Bluetooth initialisation repeatedly reports -84 frame reassembly, with no identified cause.
- The vendor's approximately eight-second boot-time radio calibration has no corresponding mainline implementation.
- After repeated reconnections, the Bluetooth ACL return path stalls although pairing, file transfer and audio work; responsibility is not attributed to either side.

The source does not establish general remedies for these issues, a device-independent wireless configuration, or the current upstream status of the community patches mentioned.

No driver, firmware, regulatory, IOMMU, power-sequencing, BlueZ, Bluetooth protocol or real-device claim has been technically validated in this pass.

## 8. Why power and time is next

The experience-library index lists power-and-time.md immediately after wireless.md. The next source is selected by the source collection's own order. The wireless material includes power-domain and suspend interactions, but that does not establish in advance how the next document will frame power, charging, time or lifecycle. Those structures must be recovered independently from its own source.

## 9. Re-entry sequence

1. Read reconstruction/zh/continuation/current-position.md.
2. Read this checkpoint, then Checkpoint 05 and earlier checkpoints as needed.
3. Read reconstruction/zh/README.md and translation/INVENTORY.md.
4. Revisit patterns.md, kernel-and-resources.md, audio.md and touch-and-input.md only where their source relationships are relevant.
5. Independently inspect experience/power-and-time.md; distinguish its own claims from expectations formed by wireless power-domain and suspend cases.

## 10. Position change

Pass 1 remains the same declared traversal. The document position has advanced from audio.md (Checkpoint 05) through wireless.md (Checkpoint 06). The next position is power-and-time.md. Pass number, document position and session checkpoint number remain separate coordinates.
