---
publish: true
created: 2026-07-20
modified: 2026-07-23T09:34:34.097+01:00
---

# Crisis Engineering - Layer Aleph

Main idea:

- When a system is in crisis, it's the best time to change it.
- A complex system is formed of both people and machines. Machines are easier to change over time, but people very often require a shattering of the status quo in order to change.

1.1: Sensemaking

- Sensemaking is the process by which we make up stuff to work in our day to day. All the assumptions, habits, and other less-than-conscious mental processes.
  - It's also called System 1 thinking.
- Sensemaking is deeply flawed, it's where fallacies live.
- However, sensemaking is vital to function. Neither humans _nor systems_ can function entirely on conscious, rational deliberation.
  - Complex systems are self-same and fractal, so a part of them will exhibit similar properties to the bigger whole.
  - This includes the need to return to sensemaking.
- A crisis disrupts sensemaking, because it makes it impossible to hold on to our current assumptions (if they worked, we wouldn't be in a crisis).
  - When sensemaking is disrupted, _going back to it becomes the highest priority_.
  - You have the opportunity to go back to a better status quo, or a worse one.

1.3: Three Mile Island

- Placing blame on specific machines or people is useless (in this case, a malfunctioning valve, or a particularly lazy operator).
  - Machines are going to break, humans are going to make mistakes. **this is predictable, and should be accounted for**.
  - A more interesting question is: Why was this mistake/breakdown the "crisis breakdown" (as opposed to the other mistakes, which are ostensibly not causing crises).

1.4: HealthCare.gov

- In a complex system, cleverness is usually bad.
  - Cleverness assumes you can predict problems. You can't.
  - Simplicity says "I know there will be unpredicted problems, so let me make it as easy as possible to deal with them".
- I need to read the SRE book
- At some point, someone declared there would be a "Vast Majority Sunday", where the vast majority of users would be able to file their health care applications or whatever. This date was weeks into the future. This was good for two reasons:
  - It kept the press off the backs of the actual people handling the crisis. They knew there'd be nothing to report until VMS, so they just... forgot about it.
  - It turned a progessive, relatively predictable, grindy, **boring** upward slope of reliability into a much more marketable **event**. If VMS was successful, the media could call it done.
    - It's very hard to report on "And today, 85% percent of people were able to file their claims, which is a 2% increase over last week..."
    - It's much more exciting to make a big deal about VMS.
  - Also, VMS was declared without technical consultation. This is probably a bad idea.

> [!important]
> I feel like there is a tie between Crisis Engineering and personal development.
> We, as individuals, usually resist change much more when it's not forced by outside factors (e.g. a crisis).
> I'm sure there's a self-help book in here somewhere.

## 2: Crisis Engineering Toolkit

2.1:

- the first goal is to converge on a shared version of the truth, and keep it updated
- ❗️**Information asymmetry is the mind killer.**
- Things in the toolkit (after establishing a crisis centre) should be done in parallel.
  - Establish a War Room
  - Map it out
  - Find your people
  - Take novel actions
  - Manage the story
  - Measure progress
- Accuracy is less important than consensus, or plausibility.
  - We're going for **Shared, Plausible, & NOW**, over several, disjointed but accurate stories.

2.2: Crisis Centre (War Room)

- COmponents:
  - An authority that converges there and can give you powers.
  - A physical venue
  - Decision-making authority and access permissions.
  - One single means of low-latency comms (Slack is too high latency, because not everyone will read it at once. A shared, 24h conference call is better.)
  - ONE incident lead.
  - A single shared journal
  - A broad, prominent, and advertised kickoff meeting
- Sensemaking is retrospective, so keep a log!
- Contacts:
  - The example table is great, I should post it somewhere (:D)

2.3: Map it out

- Do not rely on pre-existing maps. They are often inaccurate, and biased.
- A model of control (from cybernetics). An actor in a system has three components: Internal rules (**controller**), inputs (**sensors**), and available actions (**actuators**). All three form a **control loop**.
  - This is true of machines, machine systems, and also people, and people systems (and complex systems).
  - ANy piece of a system that matters must be performing one of the three functions.
  - Moreover, any piece _must be part of a control loop._
  - Control loops may be human/machine hybrid
- You can zoom in or zoom out if you're having trouble spotting control loops.
- Systems, especially people systems, tend to self-encapsulate: To limit how much they can be acted upon.
  - This is a good mental model, for instance, to explain how most managers massage the information they send upwards -- they want to minimize how much the systems above them act on them, and the only lever they have is the sensors of the control loop above.
  - Ergo, do not trust your manageres.

2.4: Get your people

- People have levers and fears. You have to figure out what the fears
