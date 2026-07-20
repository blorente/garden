---
publish: true
created: 2026-01-16T15:52:16.500+00:00
modified: 2026-06-17T15:32:02.587+01:00
---

Links: [[C++]], [[C++ Toolchains]]
Date: 2025-10-31
Visibility (remove one):

- [[Public]]

---

# C++ Toolchains

## General Terminology:

- `LD_LIBRARY_PATH`: Is the path where the loader should look for dynamic libraries.
  - Useful when we're loading a program that has been linked dynamically for use. For instance, clang may have been linked against certain versions of pthread, so we can put those in the LD\_LIBRARY\_PATH.
  - Some loaders allow command line flags as replacement for this, [e.g. `--library-path`](https://man7.org/linux/man-pages/man8/ld.so.8.html)
- `-sysroot`: Is where we point the tool (compiler or linker) to find header files and library files to compile against.
- `-isysroot`
- `-B`: it tells GCC (and by extension, clang) where to find the compiler and linker binaries, as well as the libraries that the compiler itself needs, and the crt\* files. Does a bunch of stuff.
- [`--gcc-toolchain`](https://clang.llvm.org/docs/ClangCommandLineReference.html#cmdoption-clang-gcc-toolchain): Specify a directory where Clang can find ‘include’ and ‘lib{,32,64}/gcc{,-cross}/$triple/$version’. Clang will use the GCC installation with the largest version. This only finds _binaries_.
- [`--gcc-install-dir`](https://clang.llvm.org/docs/ClangCommandLineReference.html#cmdoption-clang-gcc-install-dir): Same as --gcc-toolchain, but for libraries and headers, _not_ binaries.
- [`--gcc-triple`](https://clang.llvm.org/docs/ClangCommandLineReference.html#cmdoption-clang-gcc-triple): Search for the gcc installation with the specified triple. Clang expects this to be strictly under `<sysroot>/usr/lib/gcc/<triple>`. It will pick the highest version there.
- `-E` -> Run preprocessor
- `-###` -> dryrun. Don't run the compilation commands
- `-v`  -> Show the commands you're about to run.
- `-x <language>` -> Treat subsequent input files as having type language\`

## Sysroots and cross-compilation weirdness

Compilers need some files from the sysroot in order to operate at all. For instance, they need `inttype.h` or some thing like that to know what the heck ints the target machine has.

Usually, these are found under the sysroot, in a subdirectory keyed by the `<target>/<version>` subdirectories. For instance, `usr/lib/aarch64-unknown-linux-gnu/9.5.0/`. Clang (and gcc ostensibly) will make a best effort to figure out where these include directories are, going so far as to take the name of the binary into acount. For instance, a binary named `aarch64-unknown-linux-gnu-clang` will try to look in the sysroot under an `*/aarch64-unknown-linux-gnu/*` subdirectory.

In cases where changing the name of the binary is not an option, we can use the `-B<path/relative/to/sysroot>` flag to tell clang where to find these include files.

In [[Bazel C++ toolchains]], this is probably done through the [cxx\_builtin\_include\_directories](https://bazel.build/rules/lib/toplevel/cc_common#create_cc_toolchain_config_info.cxx_builtin_include_directories) attribute.

# Clang Modules

From https://clang.llvm.org/docs/Modules.html

> [!warning]
> These are different from the Standard C++ Modules! Those were introduced in C++20.

Raw-importing C headers has problems, both in namespacing and performance (most tools will parse every header in the transitive closure of headers for every compilation unit, so roughly every C/C++ file). Another problem: Which header files belong to which libraries? We can't know that in pure-import land, so we're screwed.

Modules are another way to fix this, which introduces the "import" keyword (different from "include"). Modules are enabled with `-fmodules`.

They introduce boundaries to libraries, which is good for:

- Parsing, as you know that you only have to parse each module once.
  - Could we have solved this by hashing the headers' parse results in the first place? Yes, but oh well.
- Compiling, so that we only compile each module once.
  - See above comment, but probably harder because some macros need to be resolved.
- Tooling, so that IDEs and stuff can know where to look for files and how to index things.

Modules are backwards-compatible. If you're using a compiler that supports modules, it'll just re-write `include`s into `import`s under the hood.

## Module Maps

A Module map is how a module defines its interface.
So, if `std` is a hypothetical module for the C standard library, then its module map would define which headers are included in the module.

### Module Headers

Part of the modulemaps is defining the headers.
Headers can just be there, with a `header <file>` declaration, in which case they'll be compiled as part of the module.

#### Textual Headers

`textual header <file>`, on the other hand, will _not_ be compiled when the module is built, and instead will behave as just a regular header, where the text is "pasted into" anything that `#include`s it. Macros are a great example.

## llvm-config

It's a file, usually in `<sysroot>/bin/llvm-config`, that can print useful llvm configuration for programs that want to link with LLVM, or use llvm as a library!!

So, I have a program that needs to link against llvm for something (e.g. to generate bindings or do some code introspection): What flags do I need to pass to compile/link against it? How do I use its headers? That's what llvm-config tries to solve.

It is _not_ a way to ask about "how do I compile something _with_ LLVM". It's always to compile _against_ LLVM.

It's located in the sysroot:

```
$ ./sysroots/x86_64-unknown-linux-gnu/bin/llvm-config --ldflags

-L./sysroots/x86_64-unknown-linux-gnu/lib -target x86_64-unknown-linux-gnu   --sysroot=./sysroots/x86_64-unknown-linux-gnu/ ...

you get the gist
```

The values were baked in when we built this particular LLVM distribution.
It has many interesting options, but some of the cool ones are :

- `--prefix`: Prints the installation prefix, so the prefix where this llvm-config is installed.

### Sources

- Clang modules: https://clang.llvm.org/docs/Modules.html
