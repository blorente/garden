---
publish: true
created: 2026-06-24T15:33:38.074+01:00
modified: 2026-07-29T09:52:27.000+01:00
published: 2026-07-29T09:52:27.000+01:00
---

Alright, so how do we do this?

We can either do a canadian cross, or do it on CI.
If we do it on CI, then Thulio loses the possiblity of doing it locally, and that kind of sucks.
So I don't think I want to do it on CI.
That leaves us with local.

But on local, we have the precedene problem
We have a dockerfile, which produces a tar.
I think we just have to pick the aarch64 tar, aand install it in another dockerfile.

Cool, so because we already use a bootstrap version of gcc, we don't really have to add anything to the image, and we don't need to re-bootstrap!

We just have to parameterize everything.

There is a CMake build there that requires that we set CMAKE\_SYSTEM\_PROCESSOR, which means...

- https://cmake.org/cmake/help/latest/variable/CMAKE\_SYSTEM\_PROCESSOR.html
- https://stackoverflow.com/questions/70475665/what-are-the-possible-values-of-cmake-system-processor
  According to claude, this flag, when cross compiling, should be set to the target platform you're targetting.
  The docs are sparse, but it says that a CMAKE config should set the variable to match the target architecture.
  This is different from CMAKE\_HOST\_SYSTEM\_PROCESSOR, which ostensibly CMAKE will use to figure out which tools it can call.

What does create\_symlinks do?
Oh, just create symlinks to the tools, like how `gcc` can point to `aarch64-gcc` because it can target aarch64.

Alright!
Things seem to be in place, but a test r8un fails:

```
ERROR: failed to build: failed to solve: process "/bin/bash -c case \"${ARCH}\" in                 x86_64) llvm_target=X86 ;;                 armv7) llvm_target=ARM ;;                 aarch64) llvm_target=AArch64 ;;                 *) >&2 echo \"Unsupported ARCH '${ARCH}' for LLVM_TARGETS_TO_BUILD\"; exit 1 ;;         esac         && cmake -G Ninja         -S /build/llvm/llvm         -B .         -DCMAKE_BUILD_TYPE=Release         -DCMAKE_INSTALL_PREFIX=/var/install/lld         -DCMAKE_C_COMPILER=/opt/gcc/${HOST_ARCH}/bin/${HOST_ARCH}-linux-gcc         -DCMAKE_CXX_COMPILER=/opt/gcc/${HOST_ARCH}/bin/${HOST_ARCH}-linux-g++         -DCMAKE_SYSTEM_NAME=Linux         -DCMAKE_SYSTEM_PROCESSOR=${HOST_ARCH}         -DLLVM_ENABLE_PROJECTS=lld         -DLLVM_TARGETS_TO_BUILD=\"${llvm_target}\"         -DLLVM_INCLUDE_TESTS=OFF         -DLLVM_INCLUDE_EXAMPLES=OFF         -DLLVM_INCLUDE_BENCHMARKS=OFF         -DLLVM_ENABLE_ZLIB=FORCE_ON         -DZLIB_INCLUDE_DIR=/var/install/zlib/include         -DZLIB_LIBRARY=/var/install/zlib/lib/libz.a         -DLLVM_ENABLE_ZSTD=FORCE_ON         -DLLVM_USE_STATIC_ZSTD=ON         -Dzstd_INCLUDE_DIR=/var/install/zstd/include         -Dzstd_LIBRARY=/var/install/zstd/lib/libzstd.a         -DLLVM_ENABLE_LIBXML2=OFF         -DLLVM_ENABLE_TERMINFO=OFF         -DLLVM_STATIC_LINK_CXX_STDLIB=ON         -DCMAKE_EXE_LINKER_FLAGS=-static-libgcc" did not complete successfully: exit code: 1
```

What fundamental misunderstanding aobut the code do I have now?

```
1.193   CMake will not be able to correctly generate this project.
1.193 Call Stack (most recent call first):
```

Apparently the c++ compiler doesn't work:

```
1.193   The C++ compiler
1.193
1.193     "/opt/gcc/aarch64/bin/aarch64-linux-g++"
1.193
1.193   is not able to compile a simple test program.
1.193
1.193   It fails with the following output:
1.193
1.193     Change Dir: /build/llvm/build/CMakeFiles/CMakeTmp
1.193
1.193     Run Build Command(s):/usr/bin/ninja cmTC_ebd99 && [1/2] Building CXX object CMakeFiles/cmTC_ebd99.dir/testCXXCompiler.cxx.o
1.193     [2/2] Linking CXX executable cmTC_ebd99

...

1.193     /opt/gcc/aarch64/aarch64-linux/bin/ld: /opt/gcc/aarch64/bin/../lib/gcc/aarch64-linux/14.3.0/../../../../aarch64-linux/lib/../lib64/libstdc++.so: undefined reference to `_Unwind_GetTextRelBase@GCC_3.0'

```

SO, CLaude suspects it has to do with the test program thinking it can link dynamically against libstdc++. I think it makes sense that it can't, because the standardl ibrary it's trying to link against is the aarch64 one.

In general, I think we're trying to statically link against stdlibc++, because we have this flag in the CMake:

```
   -DLLVM_STATIC_LINK_CXX_STDLIB=ON \
```

Which does mean we should link statically againstlibstdc++, but does it mean that we shouldn't see this error? Probably.

But why?
Let's try to undersatnd the error.
Ld, the linker, is trying to link the test binary against libstdc++. That's cool. Why does it fail? Because libstdc++ didn't have the symbils required.
Why didn't it have the symbols?

Is it libstdc++ the one that doesn't have the symbols, or is it our binary?
The former. The symbol we didn't find is `_Unwind_GetTextRelBase@GCC_3.0`, and the thing referencing it is libstdc++.

Next question is: What does the static linking change?
Well, what we're statically linking is the libstdc++ library itself, which will mean that it will resolve the symbols from the libgcc we passed to it, not the system one.

The suggestion, to use a `STATIC_LIBRARY` as the [# CMAKE\_TRY\_COMPILE\_TARGET\_TYPE](https://cmake.org/cmake/help/latest/variable/CMAKE_TRY_COMPILE_TARGET_TYPE.html), seems to be what CMake wants us to do. It wants us to set this while cross-compiling.

Now ninja failed to cross-build. Huh
Let's see if I can get a repro.

#### 2026-06-25

So the process fails at the lld stage.
That inherits from build\_image, and copies from llvm.
Eventually, that's the thing that calls the CMake BUILD.
Cool. What's the actual error?

```
66.24 /opt/gcc/aarch64/aarch64-linux/bin/ld: /opt/gcc/aarch64/bin/../lib/gcc/aarch64-linux/14.3.0/../../../../aarch64-linux/lib/../lib64/libstdc++.so: undefined reference to `_Unwind_GetLanguageSpecificData@GCC_3.0'
```

it looks like more undefined references in libstdc++
So it looks like we're not building statically for lld.
Even though we have this:

```
        -DLLVM_STATIC_LINK_CXX_STDLIB=ON \
```

So it looks like we have to actually understand the failure.

Link: CMake cross compiling guide: https://cmake.org/cmake/help/book/mastering-cmake/chapter/Cross%20Compiling%20With%20CMake.html
Let's read that doc.

To use CMake for cross-compiling, we need to create a "TOOLCHAIN FILE": [[CMake Cross Compilation]].
We don't do any of that. So we probably should.

Well, the low down here is that we should figure out why the test executable is not linking, because it's an actual error. And then we should just not build it and build a static lib, because we won't be able to run it. Once we have a CMake build that can build that executable, we can change it back to a static lib.

So, the sysroot we need to specify is

```
 -DCMAKE_SYSROOT=/opt/gcc/${HOST_ARCH}/sysroot
```

We also set `-static-libgcc `, which I have to look up what it does. It uses a statically-compiled version of libgcc.

Why shouldn't we just do the same for libstcd++? If it's libstdc++ that's bothering us by reaching to other stuff, why wouldn't we want it to be static?

In fact, we already attempt to make it static:

```
        -DLLVM_STATIC_LINK_CXX_STDLIB=ON \
```

This doesn't work for us because the flag is only to be applied while linking llvm stuff itself (this flag is for the LLVM project), not the test binaries.

But setting `-static-libstdc++` seemed to work.

###### Next: testing

So I want to test with sonic-build-infra, and for that I need to do something like "get an exec format error when trying to build something".

Ideally, we'd get an aarch64 server, and run there ,but I don't have an aarch64 server.

i need to register the toolchains to be the host toolchains, and do the execution selection there.

Alright, so the thing I built was still x86 binaries. Which sucks.
I had to configure the ./configure.sh to stop it from hardcoding the x86 host binaries.

What is the problem?
The linker, of course:

```
/opt/gcc/aarch64/aarch64-linux/bin/ld: /opt/gcc/aarch64/bin/../lib/gcc/aarch64-linux/14.3.0/../../../../aarch64-linux/lib/../lib64/libstdc++.so: undefined reference to `__eqtf2@GCC_3.0'
```

And for some reason, it didn't statically link libstdc++.so
So, which phase is this?

Well, we add LDFLAGS in ./configure.sh, which may mean that we need to add the -static-libgcc -static-libstdc++ flags.
Nother failure. The same failure. Fuck. WHyere does it happen?
CLaude suggests that this happens in gprofng, which is part of binutils, because it has a tool that doesn't respect `--with-static-standard-libraries`.
It doesn't because the flag was only meant for gdb.

Alright, so we're past that point, and now we're messing up in....
Building gcc!
This bit:

```
RUN make install-gcc
ENV PATH="/var/install/gcc/bin:${PATH}"
RUN make --jobs $(nproc) << this fails
```

The error is :

```
10.96 checking for suffix of object files... configure: error: in `/build/gcc/build/aarch64-linux/libgcc':
10.97 configure: error: cannot compute suffix of object files: cannot compile
10.97 See `config.log' for more details
```

Let's see if I can cat the config log to see the actual error:
I can't see it, not in the log, wait

```
.388 /build/gcc/libgcc/configure: line 2752: /var/install/gcc/bin/aarch64-linux-gcc: cannot execute binary file: Exec format error
```

So the problem is we're trying to run an aarch64 gcc, where we should be running an x86\_64 gcc.
Which is fair, because we're using the newly built gcc to compile gcc. Which is fucked honestly, it means I don't really udnerstnd it.
Nope, the problem was that we were configuring docker wrong.

Now that's fixed.

##### 2026-06-26

Let's see what Claude did overnight.
Well, it identified that there are no binutils as prodiced by the gcc\_x86 stage, but its solution was just to symlink the ones from the bootstrap distro. We don't want that. We want to consume the ones built with x86. So we're going to do the same as before: A phase `binutils_x86` that always runs, and then depend on that phase frfrom the gcc\_aarch64 phase.

Let's make those edits now.

Cool. Let's run a build and think about the other changes in the meantime:

1. -Wincompatible-pointer-types error: Bootstrap GCC 14 treats this as an error when compiling GCC 12's
   libgcc. Fixed by 3-stage build where CC\_FOR\_TARGET points to same-version GCC 12.
2. as: unrecognized option '-EL': GCC searches for assembler as {exec\_prefix}as (no bin/ subdirectory). With
   same\_version\_cross having no binutils, this fell through to PATH's native x86\_64 as. Fixed by symlinking the
   bootstrap's aarch64-linux-as at three locations: same\_version\_cross/as, same\_version\_cross/bin/as, and
   same\_version\_cross/bin/aarch64-linux-as.

So, the first one, it solves by setting GCC\_FOR\_TARGET. What the heck is that?
According to some source [code spelunking](https://github.com/gcc-mirror/gcc/blob/6047ed1bbc3818d9fbb1976c1fe3112928d40930/Makefile.tpl#L625), it looks like it's just the location of the gcc (or wrapper) we want to use. It would make sense that we have to set it, but I'm not sure why now. Surely the other env vars and theones we set in ./configure should be enough?
td binutils should have the as symlink anyway.

LEt's try removing stuff and seeing if it works...
And the x64 stuff should be fine to build as well!!
