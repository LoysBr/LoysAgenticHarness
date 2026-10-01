# Scope and authoring rules

Read this before a release plan review, before cutting scope, before
adding any generated content, and before deciding where a system's
numbers live.

Terms are defined in [CONTEXT.md](../../CONTEXT.md).

---

## Part 1 — How to simplify a choice

When a release is over budget, these are the cuts to reach for, in this order.

### 1. Cut the axis of variation, not the feature

The feature stays; the number of ways it can vary goes to one.

> Release 3 shipped ChattingSpot and WatchingSpot as real, separate types — but
> deleted authored Slots. Capacity became **fixed by the kind** (5 and 1) rather
> than authored per Spot. The conversation still reads as a conversation; there is
> simply nothing inside a Spot to configure.

Ask: *can this be a property of the type instead of a property of the instance?*
If yes, that is almost always the cut.

### 2. Configure on the type, not per instance

A number that could live on an asset, on an instance, or as a constant on the
class starts as a constant on the class. When it is later promoted to an asset,
the constant stays where it is and becomes that asset's default.

Example:
> Behaviour durations (Idle 5 s, Chatting 20–40 s, the 15 s wait before an NPC
> gives up on a conversation) shipped as constants on the Behaviour classes, so
> every NPC running a Behaviour ran it for the same range. Once Release 3 had been
> validated they were promoted — without moving: each Behaviour grew a nested
> `[Serializable] Timing` struct whose `Default` reads those same constants, and
> one `BehaviourSettings` asset aggregates the structs. Per-Spot and per-instance
> tuning is still parked.

The order is the point. Promoting a constant that has already been played with is
a mechanical edit against numbers known to work; authoring an asset for a number
nobody has tuned yet is a guess with an Inspector around it. Where it is promoted
*to* is [Part 3](#part-3--a-system-the-designer-tunes-ends-as-one-asset).

### 3. Keep the seam, cut the implementation

Never remove the extension point — that is what the project is selling. Remove
what is behind it.

### 4. A logging stub is a legitimate implementation

When the real subsystem belongs to a later release, implement the *interface* and
log. The seam is the deliverable; the behaviour behind it is not.

> Gestures in Release 3: `IGesturePlayer` + `GestureLogPlayer` printing the clip
> name. Release 5 swaps in `GestureAnimatorPlayer` and no Behaviour changes.

### 5. Compensate with content placement before writing code

If a simplification makes something rare and not testable, edit the content — do not build a new
system to force it.

Example:
> Chatting requires two NPCs, which nine NPCs across twelve Spots would almost
> never produce. The fix was three extra ChattingSpots near the middle of the
> District plus "raise the crowd count for the test pass" — not a matchmaking
> system.
>
> Release 4's AreaLeans pull an NPC toward a Route, so they can only pull as finely
> as the Routes allow — and all five Routes crossed the whole District, which would
> have scored every Route at about 0.5 for everybody and made the dial mathematically
> correct and visually invisible. The fix was to cut the Routes into shorter ones
> that sit inside one area, not to score individual waypoints and stitch sub-routes
> at runtime.

The tell is worth learning, because it is easy to miss: the feature works, the
numbers are right, and **nothing is visible**. Before building the system that
would make an effect show, check whether the content is simply too coarse for the
effect to land in, or suggest the human to check if content is relevant / enough to test the feature.

### 6. Never cut a defect fix to buy time

Distinguish *scope* from *correctness*. Scope is negotiable; a thing that silently
does not work is not.

### 7. Everything cut goes to `docs/objectives/outOfScope.md`, with its reasoning

A cut with a written price is Report material — "what the next one costs" quotes
real estimates. A cut that is merely forgotten is a gap.

The list is only worth quoting while it is true, so when a cut later ships, say so
on its row in the same pass. An entry still claiming a delivered thing is missing
costs more credibility than the entry ever bought.

**A promise withdrawn is not the same as a promise deferred, and must not be filed
as one.** When something the destinationSpec committed to is decided against
outright, record it as *withdrawn*, with what replaced it and what the replacement costs. "Later" implies it is still wanted;
saying so when it is not is the version of this list that stops being read.

### 8. When a simplification invalidates the glossary, fix `CONTEXT.md` **and the code identifiers** in the same pass

Example:
> Deleting authored Slots made the `Slot` entry wrong ("an ordered list… the
> number of Slots *is* the capacity"). It was rewritten to "derived from the Spot,
> not authored" in the same edit, not left to rot.
>
> Release 4 retired the word *Sandbox* from the glossary. The module was called
> `CityBox.Sandbox`, so the rename went with it — four files, about ten minutes.
> A module named after a term the project no longer uses is a trap for the next
> reader, who has to work out whether it means something.

When a retired term cannot be removed everywhere at once, leave one line in
`CONTEXT.md`'s **Flagged ambiguities** saying it is stale, so a reader who meets it
knows it is debris rather than a distinction they have missed.

### 9. Debug text is a designer-facing API — write it as one

- A **closed set of short plain phrases**, never free text. Scannable in a crowd,
  greppable in a bug report, and quotable in documentation.
- **No abbreviations and no internal jargon.** `Chat_SqNE` and `top score` were
  both rejected; `Square` and `best option` replaced them.
- Every phrase must name a **circumstance the designer can act on**. "It scored
  highest" is not one. "Every spot was full" is.
- A state the designer will see and not understand needs its own phrase. The
  15-second alone-at-a-Spot wait got `waiting for someone` for exactly this
  reason: without it, a lone NPC standing still looks like a bug.

### 10. Compute derived data; never store what a handle can invalidate

Anything derived from authored content is recomputed on demand, not cached in a
field, not written into an asset by an editor pass. The scene is edited with
ordinary handles by definition ([Part 2](#part-2--the-scene-authoring-loop)) — so
a stored derivation is a value that goes silently wrong the moment the designer
does the thing the tool was built for.

Write it as **one pure function with several callers** — the gizmo on every
repaint, Populate once, the decision every time — so what the designer sees drawn
and what the crowd acts on cannot disagree. If profiling later says it is too
expensive, cache it *then*, with an explicit invalidation, as a measured decision.

### 11. Ask about the cut, do not just take it

Present the trade with its cost named, and let the User choose. The cuts above are the
ones that survived that conversation; several proposed cuts did not.

---

## Part 2 — The scene authoring loop

This part depends on the project itself but it is worth describing the process once and always respect the same logic.
For example, do we have a script to generate the base scene content? Or is it done only by hand by a designer? Is it
then allowed to edit the positions of the objects? Do we save new positions to a generator script? Do we simply use the usual Unity scene
flow? And so on, describing the scene authoring loop.

---

## Part 3 — A system the designer tunes ends as one asset

**The rule, in one sentence:** any system complex enough that the User will want to
change how it feels ends up as **one ScriptableObject they open in the Inspector**,
not as constants they have to ask for an edit to and not as fields scattered over
scene instances.

### When

Not first. [Part 1 rule 2](#2-configure-on-the-type-not-per-instance) still holds:
numbers start as constants on the class, and the asset is where they are promoted
*once the release is validated and someone has actually played with them*. What
makes the promotion cheap is that nothing moves — see the shape below.

### The shape

1. **One asset per system, not per Behaviour and not per instance.** The numbers
   only mean anything against each other: a Chatting weight of 4 reads as "half the
   walkers" only alongside the nearness curve and the length of a conversation, and
   a designer tuning the crowd is moving all three at once. One file is what lets
   them.
2. **The numbers stay beside the code that reads them; the asset only aggregates.**
   Each Behaviour owns its nested `[Serializable] Timing` / `Approach` struct and
   its `Default`, whose values are the same private constants as before;
   `BehaviourSettings` holds one field per struct. Adding a dial is a field on that
   struct plus its default — one file, next to the code that acts on it — and the
   asset picks it up for free.
3. **What names the asset is the archetype, not the system.** `NPCTemplate` *names*
   a `BehaviourSettings` rather than carrying weights, so a second kind of NPC that
   should behave differently is a second asset pointed at, not a second set of
   fields grown on the first.
4. **An empty reference must still behave.** A shared, hidden `Default` instance
   backs anything nobody handed an asset (`NPCTemplate.Behaviour`,
   `BehaviourSettings.Default`). An unset field is a crowd that runs on the shipped
   numbers, never a crowd that stands still.
5. **Read at decide time, not cached at spawn.** The director resolves the asset
   once when an NPC is configured and reads its fields fresh on every decision, so a
   value changed mid-Play reaches the next decision while the Behaviour already
   running keeps what it was constructed with. That is what makes the asset tunable
   with the game running — which is the whole point of it.
6. **The Inspector is designer-facing API, so write it as one.** Every field gets a
   `[Tooltip]` in the designer's words and a `[Min]`/range guard; the same plain
   language standard as [rule 9](#9-debug-text-is-a-designer-facing-api--write-it-as-one).
   A field a designer cannot act on without asking what it means is not finished.
7. **It is generated get-or-create, so it needs the additive ensure-pass**  A weight the designer sets to zero survives
   the pass; a Behaviour added by a later release is appended to an asset written
   before it existed.
8. **A dial goes on the asset the designer would look in, and the module that needs
   it reaches for it through a narrow interface.** Decide the file by asking where
   the User would go looking, not by which module happens to read the number — Release
   4's Clustering sits on `CrowdSettings` beside Crowd Size and Faction Mix, because
   "how segregated is the District" is a crowd question, even though AmbientLife is
   what consumes it. AmbientLife must not depend on the Crowd module, so it defines
   the one-value interface and `CrowdSettings` implements it, the same shape as
   `IGesturePlayer`. About twelve minutes, and it keeps the module graph acyclic
   without making the designer hunt.
9. **Two assets is the ceiling before they need a sentence telling them apart.**
   `BehaviourSettings` is *what a kind of person is like*; `CrowdSettings` is *what
   crowd fills the District*. If a proposed third asset cannot be added to that
   sentence in a clause, it is a field on one of the existing two.
