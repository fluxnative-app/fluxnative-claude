---
name: preview-verification
description: "Use after changing a FluxNative app or landing page, or when the user asks how a FluxNative screen looks or behaves, why it is blank or why a button does nothing: build the preview, then look, tap and read logs in the live preview, and report only what that evidence shows."
---

# Check a FluxNative project in the live preview

## When to use

- You changed a screen, a flow or styling and need to see the result.
- The user reports something visible: a blank screen, a broken layout, a control that does nothing, wrong data.
- Not a substitute for `run_checks`: compile errors, lint and tests come from there.

Every tool here takes the project's id as `project`, the same id on every call; find it with `list_apps` (the `app-workflow` skill).

## What the preview is

FluxNative runs the project's committed source in a real headless browser. An app runs as its Expo web build inside a phone or tablet frame; a landing page runs as the built site. The preview shows layout, navigation, copy, state and network calls. It does not run native modules (camera, push, in-app purchases, biometrics, native maps). For those, say what the preview could not show; a web fallback is not proof.

The preview runs the real app: a tap or a submitted form can reach the app's connected backend and change its data. Use test input the user gave you.

## Procedure

1. **Build.** `build_preview` compiles the current source and returns diagnostics. Fix compile errors before looking at anything.
2. **Reach the screen the way a user would.** `preview_view` with `route` (for example `/login`) opens a route directly. For a flow, start from its entry screen and tap through it with `preview_act`. A screen that only works when opened by URL is not reachable.
3. **Read the structure, then look.**
   - `preview_view` with `mode` `elements` (the default) lists the interactive elements with `ref` ids. It leaves out headings and plain text.
   - `preview_view` with `mode` `text` returns the visible text: use it for copy, empty states and error messages.
   - `preview_view` with `find` locates an element by its label or role and returns its `ref`.
   - `preview_view` with `mode` `screenshot` returns an image, which costs vision tokens. Take one per state you need to judge visually, not one per step; `region` zooms into one part. Hosts that render MCP Apps show the preview inline.
   - `preview_view` with `inspect` (a `name`, `ref` or `coordinate`) says where an element is written and how it is styled; `sources` adds file and line to the tree. Use them to go from screen to code.
4. **Act.** `preview_act` with `action` `tap`, `double_tap`, `long_press`, `type`, `scroll`, `swipe`, `drag`, `key` or `fill`. Target by `name` (the visible label, preferred), then `ref`, then `coordinate`; a swipe takes a target and a `direction`, a drag two points. `preview_act` with `action` `navigate` and a `route` moves within the app. Read again after every action that should change the screen.
5. **Set the state a screen needs.** `preview_act` with `action` `reset_storage` clears the preview's stored data (a first open); `seed_storage` with `entries` (each a `key` and a string `value`) stores app data, never a session; `set_clock` with `at`, `advance` or `timezone` moves the clock for a reminder, a streak or an expiry. The app restarts on its route, except after a clock advance.
6. **Vary the device.**
   - An app: `preview_view` with `device` `iphone-15-pro` (the default), `ipad-mini`, `pixel-8`, `iphone-duo` (the foldable shut, a short screen) or `iphone-duo-open` (open, near-square); or `preview_act` with `action` `resize` and `preset` `mobile`, `tablet`, `android`, `foldable` or `foldable-open`, `orientation` `landscape`, `colorScheme` `dark`. Check dark mode and a small phone for any screen you restyled; the open foldable ignores orientation locks, so check it in landscape too.
   - A landing page: `preview_view` with `device` `desktop` (the default), `ipad-mini` and `iphone-15-pro`; or `preview_act` with `action` `resize` and `preset` `desktop`, `tablet` and `mobile`. Nothing may scroll sideways at any width, and every link must land somewhere real.
7. **When something is wrong, read the logs before the code.** `preview_logs` with `kind` `console` and `errors_only` shows runtime errors. `preview_logs` with `kind` `network` lists requests; pass a `request_id` to get one call's status, headers and body.
8. **Fix and repeat** from step 1 until the screen and its states match the intent.

## States worth checking

- Loading, empty and error states, not only the happy path with data.
- Long names and large numbers, so text wraps instead of clipping.
- Signed out and signed in, when the app has sign-in.
- Back navigation from a pushed screen, and switching tabs and coming back.

## Never

- Never report a screen as working because the build compiled or a write succeeded.
- Never claim native behavior (purchases, push, camera, sensors) from the web preview.
- Never type real personal data, real passwords or real payment details into the preview.

## Report

- What you checked, on which device and color scheme, and what you saw.
- Errors found in the console or network logs, and what you changed because of them.
- What the preview cannot verify and still needs a real device.
