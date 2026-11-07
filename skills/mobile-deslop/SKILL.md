---
name: mobile-deslop
description: Mobile app deslop for React Native and Expo. Changes the code to remove the generic AI-made look (purple gradients, card soup, tracked-capital labels, emoji icons, letter discs, dot-joined meta lines, competing buttons, spinner-only loading, dead buttons, hard-coded colours, radii and font sizes). Counts tells in the code, checks light and dark screenshots, names each tell by ID, fixes the worst tier first without changing what the app does, re-renders, and reports which platform ran it and what it could not verify. Use when an RN or Expo app looks AI-made, generic, templated, cheap or vibe-coded, or an AI tool built its UI, and the user wants it fixed, polished, cleaned up or made to look designed; when asked to remove gradients, emoji icons or card soup; or to count and fix hard-coded colours and radii (with expo-design-system, if installed). Not for websites, landing pages or web CSS, a critique or verdict with no edits, building new screens, performance, TypeScript errors or code review.
license: MIT
metadata:
  version: "1.0.0"
  source: fluxnative-app/fluxnative-skills
---

# Mobile deslop

> sniff → count (facts) → findings → fix the top tier → new render, rule each finding fixed or open → stop at 3 rounds → report, ending with a **Not verified** line

## Scope

A tell is a choice the model made by default, not for this product. This skill finds tells and removes them.
Keep features, data flow and navigation unchanged: no new screens, no removed features, no changed routes.

## References

- [code-counts](references/code-counts.md): load at Step 1.
- [screenshot-pass](references/screenshot-pass.md): load at Step 2; its Emit at Step 4.
- [tells](references/tells.md): load at Step 3 to look up tell IDs.
- [fixes](references/fixes.md): load at Step 5.
- [hand-off](references/hand-off.md): load for sample content, a second opinion or a new screen the user asked for.
- [verify-without-device](references/verify-without-device.md): load when you name your evidence.
- [fluxnative-apps](references/fluxnative-apps.md): load only if the FluxNative marker matches.
- [pass-log](references/pass-log.md): load at Step 5 for a commit per tier.
- [fluxnative-mcp-calls](references/fluxnative-mcp-calls.md): load before Step 2 only when working through the FluxNative MCP (`preview_view` connected: shots from its preview, or no local checkout).

## Sniff

Check before changing anything:

- [ ] `package.json` lists `react-native` or `expo`. If neither, stop.
- [ ] The styling system and its token file: `src/theme/`, `theme.ts`, a tokens file, a Tamagui config, a NativeWind or Tailwind config, a shared styles module.
- [ ] A design brief: `DESIGN.md` at the root. In a FluxNative app, also `.fluxnative/memory/design-dna.md`.
- [ ] The FluxNative marker (last section of this file).

Rule: fix inside the existing system first. Never add a UI kit, NativeWind or a second theme. Where the brief and this skill disagree, the brief wins, except on WCAG contrast and working controls; name the conflict in the report.

## Name your evidence

Before judging, write your best source at the top of the report: 1. a release build on a phone; 2. a simulator or emulator; 3. Expo Web at 393×852, 2x; 4. screenshots the user gave you; 5. the code only.
Everything `references/verify-without-device.md` marks "cannot" for your source goes on the Not verified line.
Write which platform ran the app on `Ran on:`. Claim only what ran there: a web render proves nothing about iOS or Android, and an iOS run proves nothing about Android.

## Step 1: pre-flight count

Run the commands in `references/code-counts.md` and fill its count table. Keep it: the report shows it before and after.
Its two screenshot counts per viewport (filled primaries, non-neutral hues) come from Step 2's baseline shots, or are `n/a, code only`.

## Step 2: look

Follow `references/screenshot-pass.md`. Code only: skip the shot checks, read the JSX, mark results `from code`.

- Screens: the ones the user names, else the default set there; at most 6.
- Shoot each at 2x, in light and dark: a review shot and a show shot. These are the baseline shots.
- On each shot, run its checks (squint, grayscale, cover the titles, bottom 120 pt, 200% text, dark mode), then walk each core flow (Walk flows there).

## Step 3: name tells

Name each tell with its ID from `references/tells.md`: C colour, T type, L layout, D data, W words, I imagery, N navigation, S states, M motion.
Never invent a tell to fill a list. Do not list a rule that holds.

**Not tells, do not flag:**

- a gradient or saturated colour that is the brand;
- sample labels the brief requires;
- native platform defaults (system tab bar, system sheet, system switch);
- a reference's structure the user asked for.

## Step 4: findings

One row per finding, in this shape:

`screen | tier | pattern | region | observation | effect | fix | evidence (seen / measured / from code)`

At most 8 findings per pass across all screens, in tier order, the biggest first impression first; tier 1 is never cut. The `Not listed:` line, field rules and an example: `references/screenshot-pass.md`, Emit.

## Step 5: fix in tier order

Never fix a lower tier while a higher one is open.

1. **Broken:** dead controls, content under bars, clipped text, wrong figures, a part of the request missing, words, palette or brand copied from a reference the user gave.
2. **Hierarchy:** nothing leads, competing primaries, a small hero figure.
3. **Structure:** card soup, identical tiles, a stamped hero, web chrome.
4. **Colour:** one accent with one meaning, gradients, dark mode.
5. **Surfaces:** radii, one way to separate, stacked borders and shadows.
6. **Density:** row heights, meta stacks, dead bands.
7. **Details:** icon family, tabular figures, press feedback, copy.

Default tier per tell ID: the table at the top of `references/tells.md`.
A finding may be raised one tier when its effect says so; write the reason in the effect field.
A finding you cannot fix this turn (needs a decision, an asset or a backend) is ruled `open: blocked, <reason>` and does not block lower tiers.
When the cause is shared (a theme value, a row component, a card wrapper), fix it there once, not per screen. `references/fixes.md` holds the **Hard rules** every fix must meet, recipes per tell group, and "Thin screens" for freed space.

## Step 6: re-check

- Run the project's type check, lint and tests (`npx tsc --noEmit`, `npm run lint`, `npm test`) before the new render. A failure is a tier 1 finding.
- Take a new render of each touched screen and re-run the counts you changed.
- Rule each finding `fixed`, naming what the screen now shows, or `open`.
- Check each new render against the Hard rules in `references/fixes.md` (contrast: pair and ratio).
- A successful write or a build that compiles is not evidence.

## Stop

A round is one pass of fix, type check, new render and ruling. Stop when no tier 1–3 finding is open (other than `open: blocked`) and no new one appears; 3 rounds have run; only details (tier 7) remain; or the user stops you.

## Report

```
Evidence: <source from "Name your evidence">
Ran on: <Expo Web export | iOS Simulator | Android emulator | phone (model) | not run>; claims cover only this platform
Counts: <table, before → after>
Findings: <rows, each ruled fixed (what the screen shows now) or open>
Before/after: <route, scheme: baseline shot → after shot, one line on what changed, or n/a, code only>
Files changed: <paths>
Not verified: <every "cannot" for your source, in the words of verify-without-device.md, and screens not reached>
```

## Never

- Copy another product's pixels, brand, copy or imagery. If a screen-library MCP is connected, take patterns from it, never pixels.
- Fake data to fill a chart.
- Change what a screen does to make it look better.

## If this is a FluxNative app

FluxNative app: the root `AGENTS.md` starts with `# Generated app source contract` and `src/platform/`, `src/theme/` and `src/features/` exist; then read `references/fluxnative-apps.md` before Step 1. That file decides imported apps.
