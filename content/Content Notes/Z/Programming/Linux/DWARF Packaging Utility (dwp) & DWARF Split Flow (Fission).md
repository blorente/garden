---
publish: true
created: 2026-06-29
modified: 2026-06-29T13:56:26.190+01:00
---

# DWARF Split Flow

Large applications have a shit ton of debug information. This can slow down the linker (and cause other problems). The big idea is that, when we're compiling, we split the debug information into a separate file: `.dwo` or `DWARF object` file.

That way, the linker only has to deal with stripped object files.

Optionally, we can also publish a GDB Index (`.gdb_index`) section in the stripped `.o` files. This allows GDB to locate debug info for symbols without having to go through all the possible `.dwo` files.

You can create these files by passing `-gsplit-dwarf` to either `gcc` or `clang`.

# DWARF Packaging Utility (dwp)

This is a package to take some `.dwo` files, and package them into something that's easy to feed to GDB and LLDB.

It's part of binutils.
