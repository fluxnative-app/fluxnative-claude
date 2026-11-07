# Over the FluxNative MCP

Read this only when the app is hosted on FluxNative and you work through its MCP server instead
of a local checkout. If the `fluxnative-mcp` skill is installed, it has the full method; follow it.

- `list_apps` finds the app; `read_file`, then `edit_file` with the SHA-256 `read_file` returned as `expected_sha256`.
- `kernel_api` for component and token names; `search_screens` for patterns, never pixels.
- `run_checks`, `build_preview`, then `preview_view` and `preview_act` for shots and taps; `preview_logs` when something fails. Write "Ran on: FluxNative web preview".
- One `run_checks` or `build_preview` at a time per app; never a second while the first's outcome is unknown. Every wait has a deadline; say when one passes.
- Take each id from the field it came from (project id from `list_apps`, `exemplar_id` from `search_screens`); never guess one.
- `list_apps` rows ending "(imported: the customer's own Expo source)" or "(FluxNative Commerce)" are hints; the files decide, as `fluxnative-apps.md` says when it is present.
