---
publish: true
created: 2026-04-01T15:41:17.764+01:00
modified: 2026-06-17T15:31:11.057+01:00
---

Links: [[Linux]], [[UEFI]], [[Bootloaders]], [[Linux Kernel]]
Date: 2026-04-01
Visibility (remove one):

- [[Public]]

---

# Unified Kernel Image (UKI)

The idea is that the image is deterministic and cryptographically signed.
It's using a UKI, a "Unified Kernel Image".
According to the[ Arch wiki](https://wiki.archlinux.org/title/Unified_kernel_image), it's an executable that can be booted directly from UEFI firmware. Distributed as an `.efi` file.
Just to make sure we're on the same page:

- [**UEFI** firmware](https://wiki.archlinux.org/title/Unified_Extensible_Firmware_Interface)  is an interface that abstracts over firmware, to allow OSs to boot in many architectures.
  So a UKI is an executable file that you can pass directly to UEFI firmware, and it will boot up the OS.
- ✅ So, as opposed to what? What are other ways to kernel image?
  - I think just having a bunch of loose files?
  - Yeah, we install the `.efi` file that is a bootloader, that then goes and looks for other files in the system like the kernel and initrd.

### Sources

https://wiki.archlinux.org/title/Unified\_kernel\_image
