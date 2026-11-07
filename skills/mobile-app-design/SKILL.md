---
name: mobile-app-design
description: Designs and builds React Native and Expo app screens from a brief, an idea, a mockup, a screenshot, a Figma frame or HTML. Load it before writing the first screen file, alongside the Expo skills; they cover the APIs, this covers the design. Infers routes, draws tab bars and headers once in the navigator, keeps colours and sizes in one token module, decides each screen's empty, loading and error states, avoids React Native port mistakes that fail silently, and checks screenshots before done. Use when asked to design, build or make a mobile app or its screens, add screens to an app, build dark mode, or turn a mockup, screenshot, Figma frame or HTML prototype into React Native or Expo code, also when a brief exists. Not for websites, native Swift or Kotlin UI, paywall, onboarding or permission screens (paywall-onboarding), restyling finished screens that look generic (mobile-deslop), choosing a look when no brief exists (mobile-design-brief) or a critique with no edits (mobile-screen-review), each if installed.
license: MIT
metadata:
  version: "1.0.0"
  source: fluxnative-app/fluxnative-skills
---

# Mobile app design

> sniff → brief → build → review → deslop → sample content → verify → report, starting with a **Ran on** line and ending with a **Not verified** line

Each step runs here with a short fallback, or in another skill of this pack if installed. Load a
reference when its step starts.

## References

- [build-screens.md](references/build-screens.md): at Build.
- [rn-silent-failures.md](references/rn-silent-failures.md): at Build when porting a mockup, HTML or a web design, and again before Verify.
- [done-checklist.md](references/done-checklist.md): at Verify, before you say done.
- [verify-without-device.md](references/verify-without-device.md): when you write the Ran on and Not verified lines.
- [fluxnative-apps.md](references/fluxnative-apps.md): only when the FluxNative marker matches (last section).
- [fluxnative-mcp-calls.md](references/fluxnative-mcp-calls.md): only when working over the FluxNative MCP (last section).

## Sniff

- `package.json`: `expo` or `react-native`, the Expo SDK version, the router, any UI library.
- The token file (`src/theme/`, `theme.ts`, a tokens module), a brief (`DESIGN.md`), and `AGENTS.md` or `CLAUDE.md` at the root.
- Adopt what exists: one styling system, one token module, one icon family. Never add a UI kit or NativeWind. An existing brief wins over this skill; name any conflict in the report.
- No `expo` or `react-native` in `package.json`: ask before creating a project.

## Pick the job

| The request | Steps |
|---|---|
| A new app or screens from an idea | 1 to 6 |
| A mockup, screenshot, Figma frame or HTML to code | 1 (route map and tokens from the images), then 2 to 6 |
| One new screen in an existing app | its job, lead and states, then 2, 3 and 6 with the app's tokens |
| A better look, no new screens | 4 only |
| A critique, no edits | 3 only |

## 1. Brief

If `mobile-design-brief` is installed, use it, then come back for Build. Otherwise write `DESIGN.md`, under 40 lines:

- **Product and person:** one line each: what it does; who opens it, and when.
- **Style direction:** one paragraph: the mood; the colour logic (a neutral ground, one accent with one meaning, status colours for status only); how the type feels; how components look (radius, surface, separation); one or two details that belong to this product. Personality goes in colour, type and imagery; layout and navigation stay conventional.
- **Screens:** each screen's job and its lead (the element seen first). Leads differ between screens.
- **Route map:** each route marked tab, push, modal or sheet.
- **Tokens:** hex values, at most 6 type sizes, 3 to 4 radius steps, spacing 4/8/12/16/24/32.

## 2. Build

Load `references/build-screens.md`. In short:

- Token module, then navigator layouts with the tab bar and headers, then shared parts, then screens in route-map order.
- Screens hold no colour, size or radius literals.
- Each screen's states are decided before its code: zero items, one item, many, loading, error, reopening a saved record.
- Porting from an image or HTML: check every item in `references/rn-silent-failures.md`.
- From a reference, take structure and spacing only, never its brand, copy or imagery.

For router, native control and animation APIs, use the Expo skills if installed.

## 3. Review

If `mobile-screen-review` is installed, use it; it does not edit. Otherwise, on a full-height shot of
each screen in light and dark, write each problem as `screen | what is on screen | fix`, and check:
one element leads; one filled primary button per viewport; nothing under a bar; no clipped text;
every enabled control does something.

## 4. Deslop

If `mobile-deslop` is installed, use it. Otherwise run the Code commands in
`references/done-checklist.md`, plus these two, and look at each hit:

```sh
rg -n '[\x{1F300}-\x{1FAFF}\x{2600}-\x{27BF}]' app src -g '*.tsx'   # emoji used as icons: usually 0
rg -n "textTransform:\s*['\"]uppercase" app src -g '*.tsx'        # usually at most 2 per screen
```

A hit is a reason to look; you decide.

## 5. Sample content

If `app-sample-content` is installed, use it. Otherwise: names common in the app's city or market,
never the generated default cast (Maya, Priya, Theo, Marcus, Leah, Ines, Hana, Jules), uneven
figures, every total derived from the same records, dates computed from today, and no "Lorem",
"John Doe", "Item 1" or "coming soon".

**Paywall, onboarding, permissions:** if `paywall-onboarding` is installed, use it. Otherwise: the
billed price is the largest price; close is at least 44 pt and visible in the first frame; Restore,
Terms and Privacy are present; each permission is asked at the moment of use, after one line
saying what it gives.

## 6. Verify

Tick `references/done-checklist.md`, or list each unticked item under Not verified.

- Type check, lint and tests first; a clean build says nothing about how a screen looks.
- A **show shot** (one viewport) for the person; a **review shot** (the whole screen, top to bottom) to judge from.
- Never report content as missing from a viewport crop: read the element tree or the source first.
- Name what ran (web export, simulator or emulator, device) and claim only what it proves.
- At most 3 rounds of fix, re-render and re-check.

## Report

```
Ran on: <Expo Web 393×852 DPR 2 | iOS simulator <model> | Android emulator <model> | device <model> | code only>
Routes: <route → tab, push, modal or sheet>
States: <screen: zero, one, loading, error, reopened → shown, or not reached>
Checks: <type check, lint, tests>
Hand-offs: <skills used, or fallback>
Files changed: <paths>
Not verified: <every "cannot" for the source, screens not reached, states not shown>
```

## Never

- A status bar, notch spacer, home indicator or second tab bar drawn inside a screen.
- "Coming soon", or a control that does nothing.
- Invented data to make a chart or list look full.
- A phone result claimed from a web render.

## If this is a FluxNative app

The root `AGENTS.md` starts with `# Generated app source contract` and `src/platform/`, `src/theme/`
and `src/features/` exist: read `references/fluxnative-apps.md` before Build. Any other project
follows its own `AGENTS.md`; change the minimum. Over the FluxNative MCP with no local checkout,
read `references/fluxnative-mcp-calls.md` before Build.
