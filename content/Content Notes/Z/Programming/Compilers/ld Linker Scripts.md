---
publish: true
created: 2026-07-29
modified: 2026-07-29T14:39:44.706+01:00
published: 2026-07-29T14:39:44.706+01:00
links:
  - "[[Compilers]]"
  - "[[GNU]]"
  - "[[C++]]"
  - "[[C++ Toolchains]]"
  - "[[Bazel]]"
  - "[[Debian]]"
sources:
  - https://wiki.osdev.org/Linker_Scripts
  - https://sourceware.org/binutils/docs/ld/Scripts.html#Scripts
  - https://sourceware.org/binutils/docs/ld/File-Commands.html
---

# ld Linker Scripts

They are exactly what they sound like -- instructions to the linker to make the CLI "more maintainable".

- `ENTRY(<symbol>)`: Defines the entry point of the application. Should correspond to the first executable instruction's symbol name.
  - Corresponds to the `-e` command line.
- `OUTPUT_FORMAT(<format>)`: The output format of the executable. In libbsd-dev (for amd), the we have `OUTPUT_FORMAT(elf64-x86-64)`.
- `SECTIONS{<sections>}`: It allows you to control where (which memory address) each of the sections of your executable will go. Sections are things like `.bss` (uninitialized data), `.data` (initialized data), and `.text`, you know the drill.

Dealing with files: ([docs](https://sourceware.org/binutils/docs/ld/File-Commands.html))

- `INPUT(<file(s)>)`: List of files to include in the linking. These would usually be just CLI arguments, loose. Gotchas:
  - If the file starts with `/`, it will be looked up _starting from the sysroot prefix_.
  - If you name a file `-l<file>`, like `-lfoo`, it will be expanded to `libfoo.a`, just like the CLI arg `-l`.
- `GROUP(<file(s)>)`: Same as `INPUT`, except they _need_ to be archives (i.e. `.so` or `.a` files most of the time).
  - Corresponds to `-(` in the CLI ([docs](https://sourceware.org/binutils/docs/ld/Options.html#index-_002d_0028)).S
- `AS_NEEDED(<file(s)>)`:
  - May only appear as part of GROUP and INPUT blocks.
  - Works exactly as not having it, except that ELF .so files will only be included if needed.
    - This means that "a `DT_NEEDED` entry will only be created in the final binary iff we reference a symbol in this archive."
  - Corresponds to `--as-needed`, which it sets unconditionally regardless of previous `--no-as-needed`.
- `LIB(<file(s)>)`: Like INPUT, except the files inside are treated _as though they were part of an archive._
- `SEARCH_DIR(<dir>)` Add this dir to the search path for ld.
- `OUTPUT(<filename>)`: Dump output here.
