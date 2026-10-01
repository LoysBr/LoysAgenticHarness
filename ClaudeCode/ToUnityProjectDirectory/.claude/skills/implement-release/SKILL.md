---
name: implement-release
description: "Implement a piece of work based on a spec or set of tickets."
disable-model-invocation: true
---

Should be called with the name of a release ('release-3' for example). Read the 3 subagent contexts, the release spec, and the release tickets. 

This Release tickets should already exist when skill is called. If not, report a problem to the user and wait.

For each ticket, execute /implement on it. 

If a ticket needs to be implemented by Human, report it to the user and wait for confirmation before continuing to implement next ticket, you can also propose to implement other tickers if they are not dependent on the ticket that needs to be implemented by Human.

Once you reach the last review ticket which should be "Human review", report to the user that all tickets have been implemented and ask him to review the release. Also report if we now have to update the destinationSpec and/or the IndicationsForEvaluators.md. If yes, report to the user those changes.
