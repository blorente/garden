---
publish: true
created: 2026-06-26
modified: 2026-06-26T16:11:43.841+01:00
---

So, what is the Alpine switch emulator?
Let's start with the SONiC HLD: https://github.com/sonic-net/SONiC/blob/master/doc/alpine/alpine\_hld.md

Alpine is a SONiC virtual switch (like `vs`), but it provides a virtual ASIC, _and_ allows you to plug in your own ASIC (from vendors). This provides what they call **dataplane capabilities**.

- ✅ What are dataplane capabilities?
  - The data plane (a.k.a. **user plane**), is the part of a router that defines what to do with an inbound packet. It's usually just a table that maps `[destination address] -> [switch route]`, usually with a cache. The destination address comes from the packet.
  - Ref: https://en.wikipedia.org/wiki/Data\_plane
- ❓Why are the dataplane capabilities noteworthy?
  - Maybe this talk will help: https://www.youtube.com/watch?v=55X1\_2utiVg
  - They don't really say, but they do mention the vendor integrations as a really important bit.
  - Hypothesis: Because the dataplane is just the important bit of the switch, so if we can replace it, that itself is noteworthy. Sometimes a big creature is its own reward.

There is a tool, called **KNE**, that uses k8s to emulate **network topologies**.
Alpine features a virtual dataplane called **lucius**, which is a software-level dataplane, it's OSS, but doesn't even try to emulate the hardware.

Alpine is distributed as a Docker image. I wonder if we just build it like other docker rules in the SONiC repo.

Back to the HLD, we have a thing that uses the vendor software, but connects to the software-based SAI pipeline that goes to Alpine. So, in this case, how does Alpine know what hardware  to emulate?

Following the HLD, we have two containers:

- A "full SONiC VM", a "SwitchStack container".
  - A **Switch Stack** is an abstraction layer over several switches that makes them look as one. At least according to this: https://documentation.meraki.com/Switching/MS\_-\_Switches/Design\_and\_Configure/Architecture\_and\_Best\_Practices/Switch\_Stacks.
    - ❓Is that what the HLD is referring to?
      - No idea, I don't think so
  - Well, whatever this is, it's a VM with all the SONiC services in it.
  - You can build containers for this platform with `PLATFORM=alpinevs`.
- An ASIC simulation container which runs the magic simulator that we don't know how it works yet.
  - Ahhhh!!!! This comes with **Lucius** by default, and vendors can provide their own if they want it!
  - ❓So this sounds like something we should be able to just download, right?
    - No idea from the HLD, need to look into the actual code.
    -

Oh, the readme for alpine gives us some clues: `platform/alpinevs/README.md`

- We can override rules/config.user for different toggles for different components.
- Doesn't mention anything about customizing  the simulator.
- But it does mention how we can deploy with KNE
  - Oh, it also mentions how we can _load Lucius into the KNE deployment_. So ostensibly we can load other dataplanes.
  - And _then_ we create the Alpine topology.
  - And the KNE topology.

Cool

So, ostensibly, if we want to use Alpine in our testing for Bazel tests, we need a way to:

- At the very least,
  - connect to a running Alpine instance.
- Ideally:
  - Build the alpine stack with Bazel. Lucius is already built with Bazel.
  - Find a way to stand that stack up with KNE. I miss systemtest.
