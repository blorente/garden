---
publish: true
created: 2026-06-23
modified: 2026-06-23T12:17:22.206+01:00
---

# Canadian Cross Build

This is a technique to compile _compilers_ that are native to an architecture we don't have access to.

Let's say we have an x86 machine, and we want to build gcc to _run on_ aarch64 (not _generate binaries_ for aarch64. _Run on aarch64_).

Let's also say we don't have access to an aarch64 machine where we can do the build.

In those cases, we can actually do multiple cross-compilation hops:

- In the x86 machine, we build an x86 gcc that can cross-compile to aarch64.
- In that same machine, we take that x86 gcc we've built, and use it to **build gcc again**, this time targeting aarch64.

In gcc, you accomplish this by setting bot the `--host` ,  `--target`, and `--build` flags to whatever you want to acompilsh:

| Hop                                               | Host        | Target  | Build |
| ------------------------------------------------- | ----------- | ------- | ----- |
| 1. Bootstrap an x86 gcc binary targetting aarch64 | x86         | aarch64 | x86   |
| 2. Use that binary to compile gcc                 | **aarch64** | aarch64 | x86   |

You will not believe the etymology:

> The term **Canadian Cross** came about because at the time that these issues were under discussion, Canada had three national political parties.[[1]]\(https://en.wikipedia.org/wiki/Cross\_compiler#cite\_note-1).

Nerds are awesome.

### Sources

- https://en.wikipedia.org/wiki/Cross\_compiler#Canadian\_Cross
