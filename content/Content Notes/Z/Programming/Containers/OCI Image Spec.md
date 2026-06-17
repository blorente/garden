---
publish: true
created: 2026-02-03T11:04:55.211+00:00
modified: 2026-06-17T15:32:45.885+01:00
---

Links: [[Containers]], [[Docker]], [[Linux]]
Date: 2026-02-03
Visibility (remove one):

- [[Public]]

---

# OCI Image Spec

- A **layer** is a set of filesystem changes bundled into a blob. This is usually a `tar`.
  - There are 3 changes: Additions, removals, and changes.
  - Additions and changes are easy: New files override the old files.
  - Removals have a special entry, a [**whiteout** entry.](https://github.com/opencontainers/image-spec/blob/main/layer.md#whiteouts)
    - Essentially, a file prefixed by `.wh.` (e.g. `.wh.<file>`) will be seen as "remove `<file>`".
  - There exist non-distributable layers, which will never be uploaded and have a different media type.
- An **image** is a bunch of layers that are applied sequentially, plus an optional image index, plus an image config determining things like "which ports to expose" and "which app to run at startup".
- An **image Manifest** is a document that describes how the configuration and layers work together.
- An **index** is just a list of manifests, perhaps to separate by architecture, that belong to the same image. For instance, you may have an index say "these two platforms share all the layers except the application".
- A **container** is a running image mounted into a system.

### Sources

- https://github.com/opencontainers/image-spec
