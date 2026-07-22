# Testing

## Verified environment

The initial drivers were verified on 22 July 2026 with QEMU 11.0.50 on an x86 freestanding MortOS build.

| Test | QEMU device option | Expected observation |
| --- | --- | --- |
| RTL8139 | `-device rtl8139` | Driver initializes, reads MAC, obtains DHCP connectivity in MortOS |
| AC'97 | `-device AC97` | Controller reports ready; PCM test submission succeeds |
| UHCI + HID | `-usb -device usb-tablet` | One USB device, descriptor ready, VID 1575, PID 1, interface class 3 |

The VID/PID values above are decimal values shown by MortOS Settings. In hexadecimal they are `0627:0001`, QEMU's emulated tablet identity.

## Recommended test progression

1. Boot in QEMU without optional devices and confirm every initializer fails cleanly.
2. Add exactly one target controller and verify discovery.
3. Verify register reset and controller-ready state.
4. Exercise DMA while capturing emulator logs or packets.
5. Run repeated cold boots and guest resets.
6. Add malformed, absent, slow, and unsupported-device cases.
7. Only then test a specifically identified physical machine.

## MortOS regression checks

The MortOS tree includes a smoke suite:

```powershell
python build.py build
python test.py smoke
python build.py iso
```

The USB verification used an ISO boot to obtain the framebuffer desktop, opened Settings → Hardware, and confirmed the live descriptor fields. The general smoke test currently covers the boot banner, help, echo, and uptime; dedicated automated hardware assertions are still a roadmap item.

## Reporting a result

Include:

- CPU/platform or QEMU version.
- Exact device option or PCI/USB vendor and product IDs.
- Whether paging/IOMMU is enabled.
- Controller BAR values and relevant status bits.
- Reproduction commands.
- Whether the failure occurs at discovery, reset, transfer submission, or completion.
