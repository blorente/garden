---
publish: true
created: 2026-06-25
modified: 2026-06-25T11:50:03.199+01:00
---

Of course, CMake doesn't know about cross-compiling out of the box. It needs to be told about it (e.g. where to find the gcc distribution).

This is usually done through a **Toolchain File**, which sets some CMake `set(CMAKE_*)` statements.

- ❓Is the toolchain file mandatory, or can we just specify the same values with good ol' `-DCMAKE_*` flags?
  - Well, `CMAKE_C_COMPILER` is the same as setting it in the `CC` env var.
  - Yeah no, it's not necessary.

Some options:

- `CMAKE_SYSTEM_NAME`: Mandatory. Lets the name of the _target_ system. It's going to be Linux. It automatically sets `CMAKE_CROSSCOMPILING` to true.
- `CMAKE_SYSTEM_VERSION`: "Sets the version of your target system." Wut. What version, why does this matter?
- `CMAKE_{C,CXX}_COMPILER`: The paths to the compilers. It also marks the search path of
- `CMAKE_FIND_ROOT_PATH`: Where to look for programs, libraries, and includes.
- `CMAKE_FIND_ROOT_PATH_MODE_{INCLUDE,LIBRARY}`: Will control what we search when we run `find_file()` and `find_path()`. We should set it to `ONLY`, so that we only search `CMAKE_FIND_ROOT_PATH`, and we ignore the host sysroot. [Docs](https://cmake.org/cmake/help/latest/variable/CMAKE_FIND_ROOT_PATH_MODE_INCLUDE.html).
