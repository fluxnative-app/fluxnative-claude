---
description: "List the FluxNative apps and landing pages you can edit, optionally filtered by name."
argument-hint: "[name]"
---

Call `list_apps` on the FluxNative connection, with `query` set to "$ARGUMENTS" when it is not empty, otherwise with no query.

Show the result as a short table: name, kind (app or landing page), workspace, last updated, and id. Mention any workspace the result says is left out because its plan does not include remote MCP. If more projects exist than one listing returns, say so and suggest a name to narrow by. Do not call any other tool. To work on one of them, the user can name it, or run `/fluxnative:connect` with its name.

If the FluxNative tools are missing or the call fails with an authentication error, FluxNative is not signed in yet: in Claude Code, open `/mcp`, select the FluxNative server and choose Authenticate; on claude.ai or in Cowork, connect it from the plugin's Connectors tab.
