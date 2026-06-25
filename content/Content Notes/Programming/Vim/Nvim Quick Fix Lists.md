---
publish: true
created: 2026-06-25
modified: 2026-06-25T12:39:25.117+01:00
published: 2026-06-25T12:39:25.117+01:00
links:
  - "[[Vim]]"
  - "[[Nvim]]"
  - "[[DevTools]]"
sources: https://neovim.io/doc/user/quickfix/
---

Src: https://neovim.io/doc/user/quickfix/

This is vim's way of doing mass operations to the system.
Or just to store a temporary list of results and operate on them.

So, let's say you have a file with some properly-formatted output:

```
# In this case, rg results that contain filenames and locations
...
toolchains/gcc/tools/BUILD.bazel:142:51:        "//toolchains/constraint:linux_x86_64": "@gcc-linux-x86_64//:linker_builtins",
toolchains/gcc/args/BUILD.bazel:20:22:    "gcc-builtin": "@gcc-linux-x86_64//:builtin_headers",
toolchains/gcc/args/BUILD.bazel:28:22:    "gcc-builtin": "@gcc-linux-x86_64//:builtin_headers",
```

We can load that into a quick fix list:

```
vim -q <file>
```

And that will give the list a unique ID inside the session.
What can we do with it?

Well, we can use `:cdo` to run a command on all entries of it (we can also use :cfirst and :cnext to actually iterate over the list, and then do individual commands on them)
We can go to those locations.

Or we can do mass replace, like this:

```
:grep ...        # run your grep of choice and create a list with the results
:cdo s/.../.../g # run vim's `s///g` command on each entry,
				 # and write to the file buffers
:cfdo update     # write back each file that changed to disk
                 # The `f` stands for "do this for the files",
                 # as opposed to "the entries in the quick fix list"
```
