# About this file

This will describe the terms and concept Claude will use in his skills and operations. Those skills should be usable for any projects, so I just have to copy them to a new project or an existing codebase to use them.

## Reporting design decisions and "how-to" explainations

`docs/howTo/assetsPipeline.md` file will be used only to talk about the 3D assets pipeline. You can write it in.

## Files in the Scope

Since we are in a Unity project, you should not try to read each file of the project when you explore the codebase. Instead, check [Scripts In Scope](SCRIPTS_IN_SCOPE.md) to know which scripts to read.

## Language

**Releases**:
The whole project is divided into several **Release** versions: a testable version, used as steps to validate features and tech choices before going further.
**Issue tracker**:
The tool that hosts a repo's issues: GitHub Issues, Linear, a local `.scratch/` markdown convention, or similar. Skills like `to-tickets`, `to-spec`, and `triage` read from and write to it.
For this project user used to work with local files in `.scratch/`.
_Avoid_: backlog manager, backlog backend, issue host

**Issue**:
A single tracked unit of work inside an **Issue tracker**: a bug, task, spec, or slice produced by `to-tickets`.
_Avoid_: ticket (use only when quoting external systems that call them tickets, or for a **Decision ticket**, see below)

**Decision ticket**:
A `wayfinder` unit: a child **Issue** of a `wayfinder:map` holding a _question_ whose resolution is a decision, not a slice of a build to execute. The **decision** qualifier is what keeps it distinct from an implementation ticket; `wayfinder` introduces the term, then uses "ticket".

## Relationships

- An **Issue tracker** holds many **Issues**
- A **Decision ticket** is an **Issue** (a child of a `wayfinder:map`)
- A **Release** holds several **Issues**

## Flagged ambiguities

- "backlog" was previously used to mean both the _tool_ hosting issues and the _body of work_ inside it. Resolved: the tool is the **Issue tracker**; "backlog" is no longer used as a domain term.
- "backlog backend" / "backlog manager". Resolved: collapsed into **Issue tracker**.
