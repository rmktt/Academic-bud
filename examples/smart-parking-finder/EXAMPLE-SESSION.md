<!--
A worked example (fictional) of one advisory round on the ParkFinder
project in ./PROJECT-CONTEXT.md. It shows the point of having several
advisors: they answer independently, they disagree, and the disagreement
is where the useful answer comes from.
-->

# Example session — "Is our 5-second debounce the right call?"

The user asked the lead: *"Is our 5-second debounce a good decision, and
how should we write about it in the report?"*

That's a design decision, so the lead ran the **researcher**, **critic**,
and **engineer** in parallel. None of them saw another's answer. Then it
gave all three answers to the **communicator**.

---

### Researcher

> Requiring presence to persist before changing state is standard practice
> in occupancy sensing. Motion-activated office lighting uses the same idea
> to avoid flicker. Published parking-occupancy systems typically use
> windows of roughly 3–10 seconds. On precedent, 5 seconds is a normal,
> defensible choice.
>
> **Bottom line:** the value is fine and matches common practice —
> *confidence: medium* (the typical range is from general knowledge; the
> team should cite one specific published system before relying on it).

### Critic

> The number may be fine. The justification isn't. The context file says it
> was "validated by timing one team member walking across a spot." That's
> one person, once, timed informally. It's an anecdote described as
> validation. A grader who reads "validated" and then learns what it means
> will start doubting every other claim in the report. Either test it
> properly (several people, several walking speeds, repeated trials) or stop
> calling it validated.
>
> **Bottom line:** the word "validated" is an overclaim and a real grading
> risk — *confidence: high*.

### Engineer

> Both of the other questions miss the failure that matters. Debounce
> covers the case where the sensor is working. What happens when it isn't?
> A dead, unplugged, or disconnected node sends nothing, and under the
> current logic "no detection" means **free**. So every broken sensor
> shows up on the map as an empty space and sends drivers to spots that may
> be taken. In a live demo, one loose wire turns into a confident, wrong
> answer. Fix: have each node send a heartbeat, and mark a spot **unknown**
> (grey) if nothing arrives for, say, 60 seconds. That's small, testable,
> and it closes the worst failure mode.
>
> **Bottom line:** the debounce is fine; missing failure handling is the
> real problem — *confidence: high* (verified from the state logic described
> in the context file).

---

### Communicator — the resolution

> **The disagreement:** the Researcher says the design is fine, the Critic
> says the claim is overstated, and the Engineer says the real risk is
> somewhere neither of them looked.
>
> **Who's right about what:** all three are right, about different things.
> The Researcher is right that 5 seconds is a sound value, so don't change
> it. The Critic is right that "validated" overclaims. That's a wording
> problem, not a design problem. The Engineer found the only actual
> **design** flaw: a dead sensor looks exactly like a free space.
>
> **The call:** keep 5 seconds. Spend the build time on the Engineer's
> heartbeat and "unknown" state. It's the one change that fixes a real
> failure, and it will help in the live demo. Don't spend time on a formal
> timing study. It isn't worth it at this stage. Instead, rewrite the claim
> honestly.
>
> **How to say it:** *"The 5-second window is an initial estimate based on
> limited informal testing, consistent with ranges used in comparable
> systems; it will be recalibrated after deployment. Nodes send a periodic
> heartbeat so that a failed sensor is shown as unknown rather than free."*
>
> **Bottom line:** keep the value, fix the failure handling, soften the
> claim — *confidence: high*.

---

### What the user saw from the lead

In full mode (the default), the user saw all four answers above, then this
summary. In **short** mode, they would have seen only this summary and the
decision below.

| Advisor | Bottom line | Confidence |
|---|---|---|
| Researcher | 5s is a normal, defensible value | medium |
| Critic | "Validated" is an overclaim | high |
| Engineer | A dead sensor reads as free — real flaw | high |
| Communicator | Keep 5s, add heartbeat/unknown, soften the claim | high |

**Your decision:** whether to build the heartbeat before the demo. The
advisors think you should; only you know whether the hardware team has
time.

---

No single advisor would have produced this answer. One says the idea is
fine, one says the claim is too strong, and one finds the bug. The
disagreement is what gives the user the full picture.
