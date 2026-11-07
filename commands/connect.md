---
description: "Pick the FluxNative app or landing page Claude works on next, by name or id, and read its project contract."
argument-hint: "<app name or id>"
---

Pick the FluxNative project the user named: "$ARGUMENTS".

1. Call `list_apps` with `query` set to that name or id. If nothing matches, or more than one project does, show the candidates and ask which one; stop there. If the argument is empty, list the projects and ask.
2. Call `read_file` with the chosen id as `project` and `path` `AGENTS.md`: the contract the project's source follows. Follow the `app-workflow` skill from here.
3. From now on, pass that id as `project` to every project tool for this project. There is no server to add and nothing to sign in to again: the FluxNative connection already reaches every project the account can edit.
4. End with one line: the project's name, kind and id, and that Claude will work on it next.
