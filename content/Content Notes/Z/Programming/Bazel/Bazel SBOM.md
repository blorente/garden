---
publish: true
created: 2026-08-07
modified: 2026-08-07T16:46:45.304+01:00
published: 2026-08-07T16:46:45.304+01:00
links:
  - "[[Bazel]]"
  - "[[DevTools]]"
  - "[[SBOM]]"
sources:
  - https://github.com/bazel-contrib/supply-chain
---

# Bazel SBOM

Apparently there's a slack channel in public, `#supply-chain-security`.
That's cool, last message was just a month ago.
Steve also seems to be using bazel-contrib/supply-chain.

Alright, seems like supply\_chain is the way to go.

https://github.com/bazel-contrib/supply-chain/blob/main/docs/metadata/README.md

- So, we have **packages** that have **attributes**. Attributes are things like licenses or who the maintainers are, or I guess versions or checksums. **attributes** are distinguished by **kind**.
- Packages carry information about where they were downloaded from, with a PURL.
  - A PURL is a universal package url, which abstracts over ecosystems and package managers.

Bazel Module authors can annotate their packages with a `package_metdatada`, so that we can generate provenance from them.

- ✅ how do we export an sbom?
  - We have a `gather_metadata` thing under tools, which seems disjoint from package\_metadata?
  - There is an aspect, `gather_metadata_info`, which traverses every attribute that provides as `TransitiveMetadataInfo`.
  - This is triggered by the `gather_metadata_info_and_write` aspect, which collects said `TransitiveMetadataInfo` and puts it in an output group (ostensibly to be later consumed by an SBOM assembler.).
    - Yep  this is exaclty it, the sbom\_rule takes a `target`, which needs to have the aspect `gather_metadata_info`
  - ✅ Does the transitive metadata info come from the `package_metadata`?
    - Certainly looks like it. It does consume the providers `PackageMetadataInfo` and `PackageAttributeInfo`, both of which come from `@package_metadata`.
- Then we need to pass it to an spdx rule.
