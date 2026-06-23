---
publish: true
created: 2026-06-22T14:51:20.087+01:00
modified: 2026-06-22T15:03:16.514+01:00
---

Links: [[GNU Make]], [[GNU]]
Date: 2026-06-22
Visibility (remove one):

- [[Public]]

---

# GNU Make - Static Pattern Rules

Ah! They are rules where we can add a target pattern between the regular target name and the prerequisites:

```
targets: **target-pattern**: prereq-patterns ...
	recipe
```

Each **target** is matched against the **target-pattern** to extract the **stem**. Then the stem is replaced in the prereq patterns to make the prerequisite names.

The stem is delineated by `%`.

So, in the rule: `$(objects): %.o %.c`, where `objects = foo.o bar.o`, we're saying that:

- If we're going to build any object file in objects,
- We need %.c as a prerequisite,
- Where % in %.c is "the name of the file, minus the .o",  as delineated by "%.o"

In the recipe, we can use `$*` to refer to the **stem**.

> [!warning]
> Please note that, for this to be considered a static pattern, you need _some_ prereq-patterns, even if they're empty. Essentially, you need two colons.
> This is a static pattern rule:
>
> - `foo.o: %.o: # Note the empty space after the second :`
>
> Whereas  this isn't
>
> - `foo.o: %.o`

### Sources

- https://www.gnu.org/software/make/manual/html\_node/Static-Pattern.html
