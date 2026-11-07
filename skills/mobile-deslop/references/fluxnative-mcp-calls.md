# Over the FluxNative MCP

Load this only when the app is hosted on FluxNative and you reach it through its MCP server. If
the `fluxnative-mcp` skill is installed, follow it for these tools.

- `preview_act` with `action: "resize"` sets the device preset and `colorScheme`; `preview_view`
  with `mode: "screenshot"` takes the shot.
- `preview_view`'s default `elements` mode returns the element tree: check it before calling
  anything missing from a screenshot.
- With no local checkout, read and edit source over the MCP (with `fluxnative-mcp` installed,
  use its hash-checked writes and its "remove the AI look" playbook), and take the before and
  after shots with `preview_view`.
- Read `kernel_api` before using any component or token name.
- The preview is an Expo Web export: write "Ran on: Expo Web export (FluxNative preview)" and
  take Not verified from the "Headless Expo Web" column of `verify-without-device.md`.
