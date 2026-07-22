# Roadmap

## USB host stack

- Replace fixed boot delays with monotonic deadlines and decoded TD errors.
- Support multiple queue heads and reusable control-transfer construction.
- Enumerate both UHCI root ports without returning after the first device.
- Add configuration selection (`SET_CONFIGURATION`).
- Add hub traversal, hot-plug state, device removal, and address allocation.
- Build interrupt and isochronous transfer APIs.
- Add HID boot-keyboard and pointer class drivers.
- Add USB mass-storage transport after a block-device API exists.

## Bluetooth

- Match Wireless Controller interface class `e0/01/01`.
- Implement USB HCI event, command, ACL, and SCO endpoint transport.
- Add HCI reset, local feature discovery, scan, and connection management.
- Add L2CAP and an initial pairing/security model.
- Select profiles explicitly rather than claiming generic Bluetooth support.

## Wi-Fi

- Choose one documented chipset or emulator target before writing a driver.
- Implement device reset, firmware-loading policy if required, RX/TX rings, and interrupts.
- Add 802.11 management, authentication, association, and WPA2/WPA3 through a clearly scoped security design.
- Keep “controller detected” separate from “driver ready” and “network connected.”

## PCI and DMA

- Traverse PCI bridges and buses beyond bus 0.
- Parse BAR sizes and capabilities safely.
- Add MSI/MSI-X behind a host interrupt API.
- Introduce a DMA allocator, physical-address translation, pinning, and memory barriers.
- Define support for 64-bit and IOMMU-enabled kernels.

## Existing devices

- RTL8139: interrupt-driven RX/TX, descriptor completion, link state, statistics, and recovery.
- AC'97: completion interrupts, streaming buffers, underrun recovery, capture, and codec capability probing.
- PC speaker: timer-backed non-blocking tones.

## Quality

- Emulator-driven hardware integration tests with machine-readable assertions.
- Register-level traces for failure diagnosis.
- Fuzz descriptor parsers with host-side tests.
- Hardware support matrix generated from reproducible reports.
- Versioned host-kernel adapter API.
