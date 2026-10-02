# Scripts In Scope

Defines **which C# files an agent should read** when it needs to know this codebase.

This is a Unity project: most of the repo is Unity-generated assets, packages and
metadata. Reading everything wastes context. Read only what this file lists.

## Include

All `.cs` files under `Assets/Scripts/`, recursively.

## Rules for agents

- Treat this set as "the codebase" for exploration, search, review and TODO scans.
- Files outside this scope may still be **edited on request** — the scope limits
  what is read proactively, not what may be touched.
- Do not summarise the files unless asked.
