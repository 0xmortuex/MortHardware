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
| [`8268e3e`](https://github.com/0xmortuex/MortOS/commit/8268e3e) | Added HID boot-mouse reports, signed pointer movement, button state, a framebuffer cursor, launcher tile clicks, and Settings section clicks. |
| [`5866840`](https://github.com/0xmortuex/MortOS/commit/5866840) | Added Bluetooth USB class/endpoint discovery, HCI Reset control transport, Command Complete validation, and honest Detected/Reset sent/Ready Settings states. |
| [`d853628`](https://github.com/0xmortuex/MortOS/commit/d853628) | Added per-device USB addresses, an eight-entry device table, both UHCI root ports, one-level hub power/reset/status enumeration, and simultaneous keyboard/mouse bindings. |
| [`c3a72a7`](https://github.com/0xmortuex/MortOS/commit/c3a72a7) | Added a Settings USB Devices inventory showing address, VID/PID, class/protocol, and configuration state for each enumerated device. |
| [`9a0e226`](https://github.com/0xmortuex/MortOS/commit/9a0e226) | Added automatic root/hub USB hot-plug recovery, manual Settings rescan, topology event diagnostics, and a 13-assertion QEMU regression suite. |
| [`560309e`](https://github.com/0xmortuex/MortOS/commit/560309e) | Completed actionable Settings pages and search routing, added live hardware/network/storage/privacy/power/diagnostic controls, and persisted per-user preferences in MortFS. |
| [`8785757`](https://github.com/0xmortuex/MortOS/commit/8785757) | Replaced the local Vex mock-up with a native Mort browser using DHCP, DNS, ARP, TCP and HTTP, including tabs, history, bookmarks, links, redirects, private mode and downloads. |
| [`1db04ca`](https://github.com/0xmortuex/MortOS/commit/1db04ca) | Added private local address suggestions, HTTP/1.1 chunked-transfer decoding, public Settings/browser architecture guides, and a 17-assertion end-to-end browser regression. |
| [`84581ca`](https://github.com/0xmortuex/MortOS/commit/84581ca) | Removed QEMU-specific routing assumptions: DHCP now records subnet mask, gateway and DNS options, while Vex performs subnet-aware ARP routing through the leased gateway. |
| [`d8cae3c`](https://github.com/0xmortuex/MortOS/commit/d8cae3c) | Added Content-Type-aware HTML/plain-text rendering, safe rejection of unsupported binary media, and expanded the native browser regression to 21 assertions. |
| [`07a7820`](https://github.com/0xmortuex/MortOS/commit/07a7820) | Connected the existing Mort USB/PS2 pointer path to Vex tabs, new-tab control, navigation buttons, address bar, history, bookmarks and extracted-link rows. |
| [`10f4d73`](https://github.com/0xmortuex/MortOS/commit/10f4d73) | Added an end-to-end QEMU check that enumerates a USB mouse, moves the live MortOS pointer, clicks Vex's new-tab control, and verifies the tab state change. |
| [`b041161`](https://github.com/0xmortuex/MortOS/commit/b041161) | Added MortFS-backed recovery of the last non-private HTTP page, exposed only through an explicit Home action so startup never makes an unexpected network request. |
| [`fefc822`](https://github.com/0xmortuex/MortOS/commit/fefc822) | Began the native HTTPS foundation in Mort with freestanding SHA-256, HMAC-SHA256 and HKDF-SHA256, each publish-gated by standard boot-time test vectors. |
| [`4b12841`](https://github.com/0xmortuex/MortOS/commit/4b12841) | Added the ChaCha20 block function in Mort and gated it with the RFC 8439 test vector as the next TLS 1.3 cipher-suite prerequisite. |
| [`2cfcaee`](https://github.com/0xmortuex/MortOS/commit/2cfcaee) | Added Poly1305 and the combined ChaCha20-Poly1305 AEAD construction in Mort, with RFC 8439 tag, ciphertext, and combined-authentication vectors. |
| [`a3a7b4b`](https://github.com/0xmortuex/MortOS/commit/a3a7b4b) | Added constant-time-ladder X25519 in Mort, validated against RFC 7748's iteration vector plus Alice/Bob public keys and shared secret. |
| [`c750da0`](https://github.com/0xmortuex/MortOS/commit/c750da0) | Added TLS 1.3 HKDF-Expand-Label and Derive-Secret encoding in Mort, validated against the published RFC 8448 handshake trace. |

## Demonstrated results

- RTL8139 Ethernet obtained a DHCP address (`10.0.2.15`) on QEMU's default user network.
- AC'97 initialized and accepted a 48 kHz PCM test-tone buffer.
- UHCI enumerated QEMU's USB tablet as one device, read VID/PID `0627:0001`, and identified USB interface class `3` (HID).
- QEMU's USB keyboard configured as class/protocol `3/1`, navigated the graphical desktop, and entered a working terminal command through UHCI reports.
- QEMU's USB mouse moved a live framebuffer cursor and opened Settings by clicking its launcher tile.
- A combined QEMU topology enumerated keyboard, class-9 hub, and downstream mouse as three devices; keyboard navigation and mouse selection worked during the same boot.
- The Settings USB Devices page displayed all three addressed devices and their descriptor identities during that combined-device boot.
- Automated QEMU tests removed and reattached both a root-port keyboard and a hub-connected mouse; the device table and HID bindings recovered in all 13 assertions.
- MortOS remained responsive in the graphical launcher and Settings app after boot-time USB enumeration.
- Vex loaded a host-served page through the guest RTL8139 path, followed a relative link and redirect, decoded a chunked response, and preserved browser state across reboot.
- The combined shell, USB, Settings, and browser verification completed 41/41 assertions on the documented tree.
- The portable-route browser regression completed 19/19 assertions, including validation of the router and DNS values delivered by DHCP.
- Content handling completed 21/21 browser assertions, including literal plain text, chunked HTML, and binary-response rejection.
- The USB-mouse browser pass completed 22/22 assertions with an actual pointer-driven toolbar click.
- Private-safe session recovery completed 24/24 browser assertions, including stored URL validation and explicit restore.
- The final combined gate completed 48/48 assertions: 4 shell, 13 USB hot-plug, 7 Settings, and 24 native-browser checks.
- The TLS primitive checkpoint completed 7/7 smoke assertions, including SHA-256, RFC 4231 HMAC and RFC 5869 HKDF vectors inside the booted kernel.
- The ChaCha20 checkpoint completed 8/8 smoke assertions with the RFC 8439 block vector inside the booted kernel.
- The authenticated-encryption checkpoint completed 10/10 smoke assertions after validating Poly1305 and the full ChaCha20-Poly1305 AEAD vector.
- The key-exchange checkpoint completed 11/11 smoke assertions with RFC 7748 iteration and full Diffie-Hellman agreement vectors.
- The TLS 1.3 key-schedule checkpoint completed 12/12 smoke assertions with an RFC 8448 derived-secret trace.

## What this does not claim

- No new physical hardware was manufactured.
- PCI Wi-Fi detection is not a Wi-Fi MAC/PHY driver.
- USB Bluetooth-class recognition is not yet a Bluetooth HCI, L2CAP, pairing, or profile stack.
- USB keyboard support is not yet a general hub, hot-plug, arbitrary-HID, storage, or isochronous subsystem.
- QEMU validation is not proof of compatibility with arbitrary physical chip revisions.

These distinctions are deliberate so readers can see exactly what MortOS can do today and what remains engineering work.
