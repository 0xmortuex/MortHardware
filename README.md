# MortHardware

Reusable bare-metal hardware drivers extracted from [MortOS](https://github.com/0xmortuex/MortOS), a hobby operating-system kernel written in the Mort language.

This repository records the hardware-support work built for MortOS and provides the source, integration contract, test recipes, and design notes needed to reuse it in another Mort kernel. It is software that controls hardware; it is not a physical board, chipset, or dongle.

## What works today

| Area | Current capability | Verified with | Status |
| --- | --- | --- | --- |
| PCI | Configuration mechanism #1, bus 0 scan, all 32 slots and 8 functions | QEMU PCI topology | Working |
| Ethernet | RTL8139 discovery, reset, MAC read, DMA TX, RX ring, frame transmit/receive | QEMU `rtl8139` | Working |
| Audio | Intel 82801AA AC'97 discovery, mixer volume, 48 kHz PCM-out DMA, test tone | QEMU `AC97` | Working |
| USB | UHCI discovery/reset, root-port reset, device address assignment, device/configuration descriptor reads | QEMU `usb-tablet` | Working foundation |
| USB classes | Interface class/subclass/protocol detection | USB HID tablet, class 3 | Working |
| PC speaker | PIT channel 2 tone generation and gate control | Legacy PC speaker interface | Working |
| Wi-Fi | PCI capability detection only | PCI class scan | Driver not implemented |
| Bluetooth | USB Wireless Controller interface detection only | USB descriptor parser | HCI transport not implemented |

“Working foundation” for USB means the host controller can enumerate a root-port device and parse its first interface. It does not yet mean hubs, hot-plug, HID reports, mass storage, or Bluetooth traffic are supported.

## Repository layout

```text
src/
  ac97.mx       Intel 82801AA AC'97 PCM driver
  endian.mx     byte-order helpers used by Ethernet
  ethernet.mx   Ethernet frame helpers
  hardware.mx   PCI capability scan and PC speaker
  rtl8139.mx    Realtek RTL8139 Ethernet driver
  uhci.mx       USB 1.1 UHCI enumeration foundation
docs/
  ARCHITECTURE.md
  MORTOS-BUILD-LOG.md
  PORTING.md
  ROADMAP.md
  TESTING.md
examples/
  boot.mx
```

## Kernel contract

The drivers intentionally rely on a very small host-kernel surface:

- `inb`, `inw`, `inl` and `outb`, `outw`, `outl` for x86 port I/O.
- PCI configuration-space access through ports `0xCF8` and `0xCFC`.
- Storage whose physical address is visible to 32-bit DMA hardware.
- A short early-boot delay mechanism.
- Initialization before interrupts for the current polling UHCI path.

MortOS currently runs with paging off, so a Mort pointer is also the physical DMA address. A kernel with paging enabled must replace every direct `&buffer as u32` DMA address with a physical-address translation and must keep the buffers pinned.

Read [the porting guide](docs/PORTING.md) before using the code outside MortOS.

## Minimal integration

Compile the required `.mx` files into the kernel's Mort translation unit, provide the platform functions above, then initialize during boot:

```mort
hw_scan_pci();
rtl_init();       // optional Ethernet
ac97_init();      // optional audio
usb_boot_init();  // before enabling interrupts in the current implementation
```

See [examples/boot.mx](examples/boot.mx) for a fuller example and [docs/ARCHITECTURE.md](docs/ARCHITECTURE.md) for ownership and data flow.

## MortOS integration

The active MortOS implementation is on the [`settings` branch](https://github.com/0xmortuex/MortOS/tree/settings). Its Settings app exposes live status for Ethernet, wireless capability, USB host/device enumeration, USB VID/PID and interface class, AC'97, storage, and other kernel capabilities.

The exact implementation history and commit IDs are recorded in [docs/MORTOS-BUILD-LOG.md](docs/MORTOS-BUILD-LOG.md).

## Safety and scope

This is experimental kernel code. It can program PCI command registers, perform DMA, reset controllers, and drive I/O ports. Run it in QEMU first. Real hardware needs chipset-specific testing, correct DMA mapping, interrupt routing, cache coherency, and robust timeout/error handling.

## Contributing

Useful next contributions include UHCI hub and hot-plug support, HID transfers, Bluetooth USB HCI transport, a documented Wi-Fi chipset target, interrupt-driven operation, PCI bus/bridge traversal, and hardware test reports. Please include the device vendor/product ID, emulator or machine model, logs, and reproduction steps.

## License

MIT © 2026 Fadi Raad (0xmortuex). See [LICENSE](LICENSE).
