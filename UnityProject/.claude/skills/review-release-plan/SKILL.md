---
name: review-release-plan
description: "Review a release plan before implementation."
disable-model-invocation: true
---

Should be called with the name of a release ('release-3' for example). Read the 3 subagent contexts, the release spec, release tickets and [SCOPE_AND_AUTHORING.md](../../../docs/agents/SCOPE_AND_AUTHORING.md)

The GameDesigner agent will load the [destinationSpec](../../../docs/objectives/destinationSpec.md) and has to answer the following questions: what will be the workflow to edit or extend the game after this release ? Human needs to validate this. 

The Programmer agent will load the existing relevant Module scripts. The Programmer agent has to answer the following questions: what will be the architecture choices and different possibilities to implement this release? What are the different module dependencies ? Human needs to validate this. Also he has to give an estimation of the time needed to implement this release. 

After this chat session with the user, you can edit the release spec if necessary and edit or improve tickets to add all these informations you have gathered. You can also create new tickets if you think some work is missing. 

Once you have completed this, you extract from the talk any general rule (if new) about how to simplify the release and implement faster / simpler, how I want to edit the Scene, and write this into [docs/agents/SCOPE_AND_AUTHORING.md](../../../docs/agents/SCOPE_AND_AUTHORING.md). Goal is to save those rules somewhere so you save time at next release. And of course you need to update the [CONTEXT.md](../../../CONTEXT.md) with any new domain term you have learned, plus GameDesigner and Programmer context files, if necessary.