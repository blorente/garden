---
publish: true
created: 2026-07-16
modified: 2026-07-16T10:49:48.192+01:00
---

# Open Network Instal Environment - ONIE

ONIE is an environment that allows you to provision switches (and, I guess, other networking devices in a data center) with your choice of [[Network Operating System]].

And by "provision" here, I don't mean "get a machine to run", but rather:

- ONIE is installed in the bare metal of the switch.
- And you can tell it (remotely, probably), to install a given [[Network Operating System]] of your choice.

So, the relationship with SONiC is that we can load ONIE-compatible Linux images into switches that have ONIE installed.

Cool cool.
