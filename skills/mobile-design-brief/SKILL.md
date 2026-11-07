---
name: mobile-design-brief
description: Picks a React Native or Expo app's look before screens are built, writes it into the theme and saves it as a short brief. Settles one accent colour with one meaning, a type scale, radius and spacing steps, the route map (tab, push, modal, sheet), each screen's job, lead element and zero, loading and error states, which animations to add and which to skip, and the category's default AI look to avoid. Use when asked to plan or decide the look before building (colours, fonts or a type scale, navigation, how new screens should look to match, one design system for screens that no longer look like one app, which animations to have). When expo-design-system or expo-animation is installed, load this one too; it picks the values, they write the code. For a request to build or make the app, mobile-app-design (if installed) loads first and calls this one. Not for removing an AI look from code (mobile-deslop) or critiquing screenshots (mobile-screen-review), each if installed; logos or websites.
license: MIT
metadata:
  version: "1.0.0"
  source: fluxnative-app/fluxnative-skills
---

# Mobile design brief

> sniff the project → write the brief → critique it against the category default → put the tokens in the theme → build → pre-finish count and flow walk → rule each finding fixed or open → stop at 3 rounds → report, ending with a **Not verified** line

The brief is a short file the app's screens are built from. Write it before the first screen, keep it in the project, and check every new screen against it.

Scope. Fill only the brief sections the request touches. Colours and fonts only: write the Product block (with Category), Colour, and Type faces and roles (skip radius and spacing), put the tokens in the theme, run Pass 2, then stop. A single navigation question: check for a brief (Sniff step 4) only, answer from references/navigation.md in 5 lines (the laws that apply), add the route to the brief only if a brief exists, and do not build. Build, pre-finish and the flow walk apply only when the user asked for screens.

## References

- [brief-template.md](references/brief-template.md): when writing the brief (Pass 1), or starting from existing screens or mockups.
- [category-defaults.md](references/category-defaults.md): when critiquing the brief (Pass 2).
- [numbers.md](references/numbers.md): when you need starting sizes (the project's tokens win).
- [states.md](references/states.md): when filling the brief's States table, or when a screen shows data.
- [navigation.md](references/navigation.md): when drawing the route map, and when reading routes from screens or mockups you were given.
- [motion-gate.md](references/motion-gate.md): when adding any animation.
- [build-and-prefinish.md](references/build-and-prefinish.md): when building screens, deriving a brief from existing screens (counts), and before finishing (rules, flow walk, Done when).
- [verify-without-device.md](references/verify-without-device.md): when rendering for Pre-finish or writing the Report.
- [fluxnative-apps.md](references/fluxnative-apps.md): only when the FluxNative marker matches.

## Sniff (5 checks)

1. `package.json`: Expo or bare React Native, the router, any UI library.
2. The theme file (`src/theme`, `theme.ts`, a tokens file, a Tamagui or NativeWind config).
3. The components folder.
4. An existing brief: `DESIGN.md` at the root, or `.fluxnative/memory/design-dna.md`.
5. `AGENTS.md` or `CLAUDE.md` at the root (the FluxNative marker: last section).

Rules:
- Adopt what exists. If the project already uses NativeWind, a UI kit or a theme object, use it. Never add a second styling system.
- If a brief exists, follow it. Change it only when the user asks or a screen proves it wrong, and add a dated note saying what changed and why.
- Ask at most one question, and only when the answer would change the whole brief (who uses it, or the market). Otherwise assume, and write each assumption on a line in the brief's Notes marked "assumed".

Existing screens, or screens and mockups as input: derive the brief and the route map from them first (routes marked "inferred"); the steps are in references/brief-template.md, "Starting from existing screens or mockups".

## Pass 1: the brief

Fill `references/brief-template.md`. The filled brief is at most about 75 lines: delete the template's guidance lines when you fill it. Save it as `DESIGN.md` at the root (`.fluxnative/memory/design-dna.md` in a FluxNative app).

Its "What each section holds" has the rules (screens with job, claim, proof and lead; one accent with one meaning; dark pairs; contrast ratios): load it while filling.

## Pass 2: critique against the default

Open `references/category-defaults.md` and find the category.

- If the palette, the lead element or the layout equals the category's default, change it and write one line in the brief saying what moved and why.
- Use no hue from the default AI palettes (judge by eye) unless the brand already owns it:
  - violet to pink;
  - cream ground with a terracotta serif;
  - near-black with a neon cyan or acid accent;
  - a UI kit's default blue.
- If a screen-library MCP is connected, take patterns from it, never pixels.

Then put the tokens in the existing theme file (none: create `src/theme/index.ts` per brief-template.md) before any screen is built. Screens read roles from the theme; no colour, size or radius literals in screen files.

## Build, navigation and motion

- Build rules: references/build-and-prefinish.md, load before building.
- Navigation laws: references/navigation.md, "Laws in brief", load when drawing routes or building screens. For the router API itself, use the Expo router skill if installed.
- Motion gate: references/motion-gate.md, load before any animation. For springs, worklets and gesture code, use the Expo animation skill if installed.

## Pre-finish

Run the counts, walk each core flow by taps (one failure included), check long strings, large text and dark mode, and check each part of the request is on a screen. Load references/build-and-prefinish.md now (steps, commands, Done when). Write each failure as `screen | what is wrong | fix` and rule it `fixed` or `open`. At most 3 render rounds.

## Report

- First line `Ran on: <source>`: release build on a device, simulator or emulator, Expo Web, user screenshots, or code only.
- Path of the brief and the theme changes.
- What moved away from the category default.
- Pre-finish counts and findings, each ruled fixed or open.
- Files changed.
- Last line `Not verified: …`: everything the source cannot show, for example device fonts, glass and blur, haptics, motion feel, native sheets, keyboard, flows not walked.

A successful write or a build that compiles is not evidence that a screen looks right.

## Never

- Start screens before the brief exists.
- Invent features or show "coming soon" for what is not built.
- Copy another product's pixels, brand, copy or imagery.
- Fill a chart or list with invented data to make a screen look full.

## Hand-off

- Building the screens from the brief: if `mobile-app-design` is installed, use it.
- Findings on the built screens: if `mobile-screen-review` is installed, use it.
- Polish and removing the generic AI look: if `mobile-deslop` is installed, use it.
- Sample names, figures and copy: if `app-sample-content` is installed, use it.
- Paywall or onboarding: if `paywall-onboarding` is installed, use it. If not, hold these four:
  - the billed price is the largest price on the paywall;
  - close is at least 44 pt and visible from the first frame;
  - Restore, Terms and Privacy are present;
  - each permission is asked at the moment of use, after one line saying what it gives.

## If this is a FluxNative app

This project is a FluxNative app when the root `AGENTS.md` starts with `# Generated app source contract` and `src/platform/`, `src/theme/` and `src/features/` exist. Then read `references/fluxnative-apps.md` (imported and FluxNative Commerce apps too) before writing the brief or touching the theme, and save the brief to `.fluxnative/memory/design-dna.md`.
