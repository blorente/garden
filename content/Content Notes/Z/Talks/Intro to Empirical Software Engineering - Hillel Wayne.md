---
publish: true
created: 2026-07-17
modified: 2026-07-17T11:15:22.383+01:00
---

# Intro to Empirical Software Engineering - Hillel Wayne

The main points of the talk:

- It's extremely hard to figure out objective truths about software engineering. We have thoughts, and ideas, and things we _like_, and that work better _for us_, but it's very hard to map that into scientific rigor.
- We have to attach _so many caveats_ to any claim, that they are not particularly meaningful.
- Broadly, the talk only addresses **reduction of defects** as a metric for something's effectiveness. Reduction of defects may or may not map to product quality, product usefulness, or any metric we actually care about in a business.
  - The speaker does make this clear: https://youtu.be/WELBnE33dpY?si=NdfE-ITAZKEDLrge\&t=1965

Some things _have_ been measured more-or-less successfully:

- Code review does have an outsized impact on the number of defects.
- TDD (defined, somewhat loosely, as writing tests before writing code) is _not_ proven to decrease defects.
  - https://youtu.be/WELBnE33dpY?si=MlKqEL0QP\_n1Ppqn\&t=1502
  - This doesn't mean that it doesn't, they just didn't find any proof of it.
  - That said, apparently folks who do TDD do spend a significantly higher amount of time _looking at the tests_, which may account for a small reduction in defects.
    - So, maybe, TDD is good even if it's just a prompt to think about the tests more clearly.
- Same with Clean Code:
  - "Effects of Clean Code on Understandability" citation: On a small sample size, it seems that smaller functions are more readable and easier to change, but much harder to debug.
  - Note: Does this mean anything useful? I don't think so.
- Also, testing itself is widely considered **obviously good**.
  - In medicine, apparently this is called a **parachute study**: "There are no studies proving that parachutes save lives, so how do we know that they do?" -> So, it's close to being considered axiomatic.
- However, **the biggest factors are not programming-specific factors**.
  - Sleep, the right level of stress, and whether the people actually want to do the task has by far the biggest impact.
  - This is roughly extrapolated from measuring white collar work, but it also replicates in software engineering.

Some more fun notes:

- Hilarious mention of the deeply flawed "A Large Scale Study of Programming Languages and Code Quality in Github": https://youtu.be/WELBnE33dpY?si=K4qNjCwqnijwocQN\&t=1005. Get rekt.

https://www.youtube.com/watch?v=WELBnE33dpY
