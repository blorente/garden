---
publish: true
created: 2026-02-05T11:40:49.547+00:00
modified: 2026-06-17T15:31:19.559+01:00
---

Links: [[Linux]], [[Debian]]
Date: 2026-02-03
Visibility (remove one):

- [[Public]]

---

# dh and debhelper

### debhelper

debhelper is a tool suite for deb packages.
It's used to build debian packages.
The idea is that we have a bunch of `dh_***` tools that all play together.
Then, we can use in a `debian/rules` file to sequence them and transform the commands.
In `debian/rules`, we can use `dh` to automate parts of this process.

### dh

`dh` will just run a sequence of debhelper commands.
It has some predefined sequences, like `build`, `build-arch`, `install`, and `binary`.
We can modify one of those sequences by modifying `debian/rules`.
For instance, we can override a command by inserting an `override_<dh_command>:` rule in `debian/rules`.

We can pass the `--no-act` flag to see which sequences of commands we're going to do.
But on new versions, you may be able to use `dh_assistant`.
