---
publish: true
created: 2026-04-01T15:42:24.371+01:00
modified: 2026-06-17T15:30:21.443+01:00
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

### Sources

- https://en.wikipedia.org/wiki/Initial\_ramdisk
