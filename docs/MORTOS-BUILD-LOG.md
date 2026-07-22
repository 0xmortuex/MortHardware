# What was built in MortOS

This is the public implementation record for the hardware work integrated into MortOS on 22 July 2026.

| MortOS commit | Work delivered |
| --- | --- |
| [`6f57f8c`](https://github.com/0xmortuex/MortOS/commit/6f57f8c) | Vendored the Mort networking stack and exposed `net`/`httpd` shell commands. The stack includes Ethernet, ARP, IPv4, ICMP, UDP, DHCP, DNS, TCP, and HTTP support around RTL8139. |
| [`9499d8e`](https://github.com/0xmortuex/MortOS/commit/9499d8e) | Added generic PCI capability discovery, Settings hardware status, Ethernet lifecycle controls, and legacy PC-speaker support. |
| [`2dfd9c4`](https://github.com/0xmortuex/MortOS/commit/2dfd9c4) | Added Intel 82801AA AC'97 discovery, volume control, PCM DMA, and Settings controls. |
| [`fab3759`](https://github.com/0xmortuex/MortOS/commit/fab3759) | Corrected PCI scanning to include all eight functions per slot, enabling discovery of multifunction USB controllers. |
| [`111c4ac`](https://github.com/0xmortuex/MortOS/commit/111c4ac) | Added minimal freestanding `memset`, `memcpy`, and `memmove` primitives required by optimized kernel builds. |
| [`48a2c0d`](https://github.com/0xmortuex/MortOS/commit/48a2c0d) | Added boot-time UHCI ownership, root-port enumeration, USB address assignment, device/configuration descriptors, VID/PID, and interface class reporting in Settings. |
| [`8db9d6b`](https://github.com/0xmortuex/MortOS/commit/8db9d6b) | Added USB `SET_CONFIGURATION`, endpoint discovery, HID boot-protocol selection, interrupt-IN polling, usage-to-scancode translation, and working USB keyboard input. |

## Demonstrated results

- RTL8139 Ethernet obtained a DHCP address (`10.0.2.15`) on QEMU's default user network.
- AC'97 initialized and accepted a 48 kHz PCM test-tone buffer.
- UHCI enumerated QEMU's USB tablet as one device, read VID/PID `0627:0001`, and identified USB interface class `3` (HID).
- QEMU's USB keyboard configured as class/protocol `3/1`, navigated the graphical desktop, and entered a working terminal command through UHCI reports.
- MortOS remained responsive in the graphical launcher and Settings app after boot-time USB enumeration.

## What this does not claim

- No new physical hardware was manufactured.
- PCI Wi-Fi detection is not a Wi-Fi MAC/PHY driver.
- USB Bluetooth-class recognition is not yet a Bluetooth HCI, L2CAP, pairing, or profile stack.
- USB keyboard support is not yet a general hub, hot-plug, arbitrary-HID, storage, or isochronous subsystem.
- QEMU validation is not proof of compatibility with arbitrary physical chip revisions.

These distinctions are deliberate so readers can see exactly what MortOS can do today and what remains engineering work.
