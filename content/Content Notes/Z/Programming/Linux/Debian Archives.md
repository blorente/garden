---
publish: true
created: 2026-06-17T15:20:42.232+01:00
modified: 2026-06-17T15:29:28.372+01:00
---

Links: [[Debian]], [[Linux]]
Date: 2026-06-17
Visibility (remove one):

- [[Public]]

---

# Debian Archives

A Debian archive is just two tars in a single archive. One tar has installable data, another has control information about such data.

### Tools

There is a tool called `alien` that can convert to and from Debian archives (from other Linux package formats like rpm): https://en.wikipedia.org/wiki/Alien\_(file\_converter). Interestignly, it looks like it _can_ convert from a single `tar.gz` into a deb archive.

- Not great reviews on sourceforge.

Another tool, called `checkinstall`, claims to be able to make debs from source.

### Actual Spec

From Debian 0.93 onwards (anything after 1995), a `.deb` is just an ar archive with three files:

- `debian-binary`, which contains one line with the package format (usually 2.0).
- `control.tar[.gz|.xz]`, which contains the metadata, like deps, package name and version... -> [Docs](https://en.wikipedia.org/wiki/Deb_\(file_format\)#Control_archive).
- `data.tar[.gz|.xz]`, which contains the files, in a rootfs layout.

### Sources

- https://en.wikipedia.org/wiki/Deb\_(file\_format)
