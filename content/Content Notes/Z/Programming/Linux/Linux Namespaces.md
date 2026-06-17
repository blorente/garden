---
publish: true
created: 2026-02-03T10:36:51.514+00:00
modified: 2026-06-17T15:31:30.628+01:00
---

Links: [[Linux]], [[Containers]]
Date: 2026-02-03
Visibility (remove one):

- [[Public]]

---

# Linux Namespaces

A linux namespace is a kernel mechanism to partition resources so that only specific _processes_ can see them.

So, let's say a process opens `/var/tmp/hello.txt`. That process will always have _a namespace_ that it opens files in, which will dictate which underlying file it accesses.

PIDs are an easier example: Two processes can have the same PID if they are in different PID namespaces.

Processes can create namespaces, like Docker. Docker creates namespaces for containers.

Kinds of namespaces:

- Mount,for filesystem.s
  - It's important to note that this only isolates mounts, not specific files.
  - If a dir is mounted into two namespaces, and the first namespace creates a file in that dir, the second namespace will see it.
  - however, if we add a mount in the second namespace to a mount that didn't exist before, then the files start to diverge.

- PID,

- Network

- IPC

- UTS -> a single system can appear to have different hostnames and domain names if they are in different namespaces.
  - I'm guessing this is what allows to do host.docker.stuff

- User ID -> Different processes can have different user IDs and privileges. For instance, you can give admin rights to a user that only exists on a namespace, and it will only be able to be an admin for the things in that namespace.

- cgroup -> Not sure what is the control group.

- Time

- ❓If two processes in two namespaces access exactly the same file, will they get the same FD?

- ❓When i create a namespace, do I have to create _all_ kinds at once?

- ❓Could I learn more about mounts?

### Sources

- https://en.wikipedia.org/wiki/Linux\_namespaces
