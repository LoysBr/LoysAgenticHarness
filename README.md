# Loys Agentic Harness

After several small projects using Agentic coding workflow (mainly Claude Code so far), I have learned some ways to optimize AI usage. This is the repo where I will store all my reusable skills and harness files.

## Subagents - Simulate a studio

My idea is to simulate game development sprints and use AI **subagents** (a "Game Designer", a "Programmer" and a "QA") to paralellize some tasks, like in a real studio. You can check their config files in `.claude\agents\`

| Role | Owns | Memory file |
|---|---|---|
| **Game Designer** | Destination spec, Release specs, designer-facing wording | `CONTEXT_GD.md` |
| **Programmer** | Architecture, decision records, implementation of tickets | `CONTEXT_PROGRAMMER.md` |
| **QA** | Reviewing tickets and Releases, testing in Unity | `CONTEXT_QA.md` |

Each writes what it learned into its own file, so nothing is researched twice
across sessions. That memory is the reason the work sped up rather than slowed
down across seven days.

## Matt Pocock Skills in Unity

I have adopted several [Matt Pocock's skills](https://github.com/mattpocock/skills) and edited them a bit to fit my own needs: making small Unity Projects. The main difference is the organisation of the work in big blocks I call **Releases**.

### Workflow

1. Write the [Destination Spec]("UnityProject\docs\objectives\destinationSpec.md") + Define a precise glossary in [CONTEXT.md]("UnityProject\CONTEXT.md")
2. Divide the work in big Releases (write a Release Plan)
3. Divide the release you want to implement into Tickets
4. Implement them with (sometimes several tickets can be parallelized)
5. Each ticket should be reviewed
6. At the end of a Release implementation, review it yourself
7. If necessary, re-think your Release Plan or edit Destination Spec
8. Implement next Release...

## Glossary

If you already have a sort of design document or just an idea, before writing the Destination Spec you may want to make your concepts clear, both for you and the Agents. Use **/grill-with-docs** to fill the [CONTEXT.md]("UnityProject\CONTEXT.md"). 

## Destination Spec

Like Matt Pocock suggests, writing a precise destination spec is very useful, to know exactly what you want at the end.
You can use **/wayfinder** skill to create it.

## Tasks Organized in Releases

Agents need to keep their "context" (memory frame) relatively small to stay "smart". 200K tokens seems a good maximum to respect. So the goal of a good task management system is to divide big efforts into smaller ones. A second important thing I learned: you need to keep the control on your code, or "keep ownership" as they say. And of course, control the work done. That's why I chose to split my project development into **Releases**: specs files both Agents or Humans can read (located in `docs\objectives\releases`). At the end of each Release, the project should be in a testable state. So you can test it yourself, review the code, debug, and possibly fix some stuff. It's also a good time to think "are my objectvies still the same?" and maybe edit the **Destination Spec** or the next **Releases** plan.

TODO: create a skill to create a release plan.
TODO: create a skill to write a release spec.

## /to-tickets

Once the release spec is writen, just use **/to-tickets** on Release X. Tickets are created locally in `.scratch/`. You can choose to commit them if you're working in a team but then maybe it makes more sense to use a proper tool for the tasks, and to configure your skills to work with them (original Matt Pocock skills can do that). Each ticket shows also his blockers, and explains each Test Pass/Fail step. So for big tickets I review them myself, but otherwise, Claude can start a QA subagent to review its own job. Some tickets are also my duty.

The last ticket of a release should always be a human review and test. Very important step, taking some time but it's my way to keep control on the project. It also allows me to imagine new features, especially Editor tools, ideas comes only at this moment most of the time.

## Implementation

If a Release is too big to be implemented at once with **/implement-release**, prefer to **/implement** each ticket one by one. It allows you to use **/clear** or **/compact** between each, otherwise your /context will get too big.

## Release Review

Your job.

After this you may want to edit the Release Plan or Destination Spec.

WIP / TODO : improve /review-release-plan
