---
publish: true
created: 2026-03-02T12:35:57.903+00:00
modified: 2026-06-17T15:32:20.388+01:00
---

Links:  [[Linux]], [[C++]], [[GNU]], [[ld]]
Date: 2026-03-02
Visibility (remove one):

- [[Public]]

---

# Shared Library Versioning autotools

An interesting analogy: An ABI is like an API, but for the dynamic loader and the consumers of the library. So, breaking the ABI is breaking binary compatibility.

The ABI and the API can evolve separately: We can change the parameter of a function from Int to Long. I this case, I guess a user of the function will have to recompile, even if it hasn't changed the code at all (because an Int can fit into a long).

The ABI version changes independently to the API version, and usually less often. In ELF and Mach-O, this ABI version is listed in the SONAME entry. Dependencies will include this version in their `DT_NEEDED` entries.

Generally, the SONAME is only one number, so `libfoo.so.0`.

You can pass `-version-info` to autotools to set the SONAME and ABI versions. On linux, the mapping for `-version-info=x:y:z` is `libfoo.so.(x-z).y.z`.

### Sources

- https://autotools.info/libtool/version.html
