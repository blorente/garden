---
publish: true
created: 2026-07-24
modified: 2026-07-24T16:09:33.703+01:00
published: 2026-07-24T16:09:33.703+01:00
links:
  - "[[Linux]]"
  - "[[Debian]]"
sources:
  - https://www.debian.org/doc/debian-policy/ch-relationships.html#virtual-packages-provides
  - "[Alpine HLD](https://github.com/sonic-net/SONiC/blob/master/doc/alpine/alpine_hld.md)"
  - https://wiki.debian.org/DebianRepository
---

# Debian Repositories

Debian repositories have two important files:

- `Packages.<ext>` (most commonly `.xz`): Defines which packages are available, and what they depend on.
  - The file itself is made up of groups, separated by two newlines, which are made of package declarations, separated by single newlines.
  - A package declaration looks like this:
  - ```
    ```

Package: gccgo-12-powerpc-linux-gnu
Source: gcc-12-cross-ports (12)
Version: 12.2.0-13cross1
Installed-Size: 27836
Maintainer: Debian GCC Maintainers <debian-gcc@lists.debian.org>
Architecture: amd64
Depends: gcc-12-powerpc-linux-gnu-base (= 12.2.0-13cross1), gcc-12-powerpc-linux-gnu (= 12.2.0-13cross1), libgo-12-dev-powerpc-cross (>= 12), libc6-dev-powerpc-cross (>= 2.23-1~), libc6 (>= 2.36), libgmp10 (>= 2:6.2.1+dfsg1), libisl23 (>= 0.15), libmpc3 (>= 1.1.0), libmpfr6 (>= 3.1.3), libzstd1 (>= 1.5.2), zlib1g (>= 1:1.1.4)
Suggests: gccgo-12-doc
Conflicts: golang-go (<< 2:1.3.3-1ubuntu2)
Description: GNU Go compiler
Multi-Arch: foreign
Homepage: http://gcc.gnu.org/
Built-Using: gcc-12 (= 12.2.0-13)
Description-md5: 58c2a4ce4d3fe6815f7a6ee86b4db16d
Section: devel
Priority: optional
Filename: pool/main/g/gcc-12-cross-ports/gccgo-12-powerpc-linux-gnu\_12.2.0-13cross1\_amd64.deb
Size: 8854092
MD5sum: 1795202dc7745a25a8b4593f9903cb63
SHA256: d942113df9c6fa731be6f00a2173b2c0b3daa1b441e8f0a3de2fe9dd5ca36c1f
\`\`\`

- `Contentx.<ext>` (most commonly `.gz`), which contains a map of "files to which packages export these files".
  - This is optional.
  - The spec is "zero or more lines of text, followed by a two-column table"
  - I could go deeper, but I shan't.

### Virtual Packages

https://www.debian.org/doc/debian-policy/ch-relationships.html#virtual-packages-provides

A package can `Provide` more than one package.
Package relationship fields (`Depends`, `Recommends`, etc.) are where virtual packages will show up.

For instance. If we have package `bar`:

```
Package: bar
```

And foo depends on bar:

```
Package: foo
Depends: bar
```

And someone releases `bar-plus`, which does everything `bar` does, but better and more, it can say:

```
Package: bar-plus
Provides: bar
```

Now `bar-plus` will satisfy `foo`

Virtual packages can be keyed by version. This is valid:

```
Package: bar-plus
Providers: bar (=1.0)
```

If `foo` depends on `bar (=1.1)`, `bar-plus` won't do.
