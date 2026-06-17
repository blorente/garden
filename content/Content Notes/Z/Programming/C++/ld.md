---
publish: true
created: 2026-02-04T13:54:04.616+00:00
modified: 2026-06-17T15:32:09.891+01:00
---

Links: [[C++]], [[C++ Toolchains]]
Date: 2025-10-31
Visibility (remove one):

- [[Public]]

---

# ld

So ld is the gcc linker.

Interesting tidbit around when files are opened: Some flags, like `-l or -T`,
cause the files referenced to be read when they are specified in the command line.

### The linker script

The linker operates on a linker script.
There is a default linker script, which dictates what has to happen.

If the linker can't recognize the format of a file, it will interpret it as a linker script.
That way, we can make .sos that appear like .sos, but are scripts.
This linker script is appended to the linker script with more commands.

### Interesting Options

- `--exclude-libs lib,lib...`: A list of archives, from which symbols should not be exported.
  - Does that mean "Should not be exported into the thing we're linking, or _from_ the thing we're linking?

- `--remap-inputs=<pattern>=<filename>`: Any attempt to open anything in pattern will open the other thing instead.
  - If the target is `/dev/null`, the file will be ignored instead.
  - Note that this only affects inputs that come _after_ this flag in the CLI.

- `-M` or `--print-map`: Print a linking map to stdout.

- `-Map=<file>`: Print the linking map to a file. If you pass `-`, it'll print to stdout.

- `-rpath=<dir>`: Add `dir` to the RPATH of the linked object.

- `-rpath-link=<dir>`: Add `dir` to the path of files searchable when resolving symbols across shared objects.
  - You can use `$ORIGIN` (the abs path to the directory containing the program being linked),
    or `$LIB`, which is `/lib` or `/lib64`, depending on your system.
  - This is useful if the shared object you want to link against has been linked with an `-rpath` that doesn't apply to your system.
    - For instance, imagine `foo` wants to link against `bar.so`, which was linked against `/lib/baz.so`.
    - In this case, `bar.so` will have an RPATH entry including `/lib`.
    - But we want to link against our own `baz.so`, maybe from Bazel.
    - We can pass `--rpath-link=<bazel-out/...>` so that it'll look there for `baz.so` before trying to find it in the rpath.

- `--sysroot`: Path to the sysroot.

- `--wrap=<symbol>`: Replace every call to `<symbol>` with `_wrap_<symbol>`. We should supply the wrap code.
  If the wrapper wants to call the real `<symbol>`, they can call `_real_<symbol>`.
  We may want to supply a default `_real_<symbol>` just in case we build without `--wrap`.

- `--whole-archive`: Load the entire static archive, regardless of symbol use.
  For purposes of deduplication, it means that:
  If an executable links both a static library and a dynamic library that has (in part) the same object code as the static library then the —whole-archive will ensure that at link time the code from the static library is preferred. This is usually what you want when you do static linking.
  Ref: https://stackoverflow.com/a/821287
