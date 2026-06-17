---
publish: true
created: 2026-02-03T10:31:39.469+00:00
modified: 2026-06-17T15:32:37.694+01:00
---

Links: [[Docker]], [[Containers]] , [[Linux]]
Date: 2026-02-03
Visibility (remove one):

- [[Public]]

---

# How A Container FS Works

So, at its core, the root filesystem of a container (rootfs) is just "a bunch of directories in the host's OS that we mounted as the root dirs (e.g. `/dev`, `/proc`...) in the container's mount namespace"

So, questions:

- ✅ What is a namespace? [[Linux Namespaces]]
- And the tutorial cuts off there.
  But the gist of it is that we use Linux Namespaces to make sure that all resources in a container are isolated.

### Sources

- https://labs.iximiuz.com/tutorials/container-filesystem-from-scratch
