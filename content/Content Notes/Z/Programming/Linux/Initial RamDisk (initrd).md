---
publish: true
created: 2026-04-01T15:42:24.371+01:00
modified: 2026-07-16T15:16:01.388+01:00
---

Links: [[Linux]], [[Bootloaders]], [[Linux Kernel]]
Date: 2026-04-01
Visibility (remove one):

- [[Public]]

---

# Initial RamDisk (initrd)

It stands for `Initial RamDisk`. Wikipedia: https://en.wikipedia.org/wiki/Initial\_ramdisk
It's a scheme for loading a temporary file system into RAM.
This is useful, for instance, if the kernel needs access to dynamic libs as part of its boot sequence (for instance, to load dynamic kernel modules for drivers and stuff).

Why do we have something like this?
Well, apart from loadable kernel modules, we also have the problem of "What if our disk is in RAID, or in NFS, or otherwise not on this machine?", and "What if we're hybernating?".

Implementation-wise, initrd is a filesystem image like the ones we use for OCI.
It then gets mounted into `/dev/ram` (so like containers, but with fewer protections.)

> Once the initial root file system is up, the kernel executes /linuxrc as its first process;\[4] when it exits, the kernel assumes that the real root file system has been mounted and executes /sbin/init to begin the normal user-space boot process

- ✅ Why is the ramdisk better than a physical disk?
  - Because the kernel needs drivers to read from disk, but it needs to mount the disk to read the drivers.
  - This initramfs contains kernel modules for uniform drivers, like NVMe, USB, RAID, etc.
    - We can modify it by compiling our own kernel, of course.
      - We can just compile the modules into the kernel
    - Or by shipping our own initramfs without recompiling the kernel.
      - If the kernel is compiled to look for it, it can look for an [[Initial RamDisk (initrd)]] , which must contain a binary called `/linuxrc` which loads the driver modules.
    - Yep. We can ship our own initramfs.
    - https://wiki.debian.org/.internal/challenge.html?original=%2finitramfs#Rebuilding\_the\_initramfs
      - In debian, they build this initramfs with `initramfs-tools`.

### Sources

- https://en.wikipedia.org/wiki/Initial\_ramdisk
- https://wiki.debian.org/.internal/challenge.html?original=%2finitramfs#Rebuilding\_the\_initramfs
