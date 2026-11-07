# Screenshot pass

How to look at a screen so the tells show. Work through the sections below in order on each screen; write notes, not findings, until Emit.

## Before you start

- **Shoot at 2x.** At 1x a model misread food thumbnails as the wrong dishes. Small text, hairlines, radii and icon strokes need 2x.
- **Which screens:** the ones the user names. If none, the first screen of each tab, plus the first-run screen and the screen with the most list rows or money figures; 3 if the user asked for 3, otherwise at most 6. Fill any room left with the entry screen, the screen people use most, one detail screen, one form or task, one list, and the paywall or onboarding if the app has one. At the cap, the user's screens come first, then the tab, first-run and busiest screens.
- **Reach each screen by its route in `app/`.** If a screen needs sign-in or data you cannot supply, shoot what you can reach and put `screens not reached: <routes>` on the Not verified line.
- **Light and dark** of each, labelled by route and scheme ("/orders, dark").
- **Two kinds of shot per screen:**
  - *Review shot*, for you: the whole height of the screen. On the web try a full-page capture; on Expo Web the page often stops at one viewport because the `ScrollView` scrolls inside the page, so then scroll one viewport at a time and shoot each page. Judge page 1 as the first impression; later pages only for consistency.
  - *Show shot*, for the user: one viewport, as it looks on the phone. Use it for the before and after in the report.
- **Never call content missing from a crop.** A viewport shot, a single page or a zoomed region shows part of the screen. Before writing that something is missing, cut off or absent, check the element tree (the browser tool's accessibility tree, or the simulator's inspector) or the screen's source. If you only have still shots, write `not seen in the shots`, not `missing`.
- **Standard sizes:** 393×852 pt for a current iPhone and 412×915 dp for a common Android phone (house choice, not a platform rule).

Capture without a device:

- **Expo Web:** serve the Expo Web export (`npx expo export -p web`; run the `rg` counts first, so a missing simulator does not block them), open it in a browser tool at 393×852 with device pixel ratio 2, and take one screenshot per viewport. Emulate `prefers-color-scheme: dark` for dark mode, if the app follows the system scheme.
  - Serve the `dist/` folder the export writes: `npx serve dist -l <port>`, or a small static server such as `python3 -m http.server <port> -d dist`. Note its PID and stop it by PID when the shots are done.
  - If the export fails or its web dependencies (`react-native-web`, `react-dom`, `@expo/metro-runtime`) are missing, do not install them without asking. Use a simulator or emulator, or go code only, and say so on the `Ran on:` line.
- **iOS Simulator:** `xcrun simctl io booted screenshot shot.png`; dark mode with `xcrun simctl ui booted appearance dark`.
- **Android emulator:** `adb exec-out screencap -p > shot.png`; dark mode with `adb shell cmd uimode night yes`.
- **Screenshots from the user:** ask which device and scheme each one shows; you cannot tap or scroll them, so flows go on the Not verified line.

What each source can prove is in `verify-without-device.md`. Write which of these actually ran the app on the report's `Ran on:` line, and claim only what ran there.

**Code only:** skip the shot checks (Squint, Grayscale, the two cover tests in Craft lines, and Light, dark, longest strings, Android); flows you cannot walk go on the Not verified line (Walk flows). Read the screen's JSX instead (which element has the largest `fontSize`; whether state styles differ only by colour; one component mapped over a list) and mark each result `from code`.

## Baseline

Shoot each screen as it is, before any change. Keep the files and the page text: the report compares against them. Write one line per screen: its job, and what should lead.

## Squint

Blur the shot or shrink it to about a quarter (`sips -Z 200 shot.png --out small.png` on macOS), or look from a distance.

- What leads? There should be one thing, and it should be what the screen's job says.
- Three or more things at the same weight: T1 or L2.
- A decoration leads instead of content: C1, C3, L7.

## Grayscale

Convert the shot (`magick shot.png -colorspace Gray gray.png` with ImageMagick, or a grayscale filter in browser dev tools).

- Can you still tell the selected chip, the current step, a gain from a loss, an error from a hint? If not: C8.
- Does the primary action still stand out by weight and position alone?

If you cannot make a grayscale copy, list every state marker and check each has a second cue (icon, weight, outline, sign or word).

## Craft lines

Walk each screen through these:

- Weight marks what leads; one hero with room around it.
- No filler cards: every card is one self-contained object.
- Figures plausible for the market, and the same on every screen.
- Content rows 64–76 pt (60 only when one line) (house heuristic).
- Few colours, each with one meaning.
- Drawn, not stock: charts from real series, photos that fit the content.
- Alive, not a spreadsheet: something on the entry screen shows the product's real data.
- Not PowerPoint: no shape that carries nothing.

Two quick tests here:

- **Cover the titles.** Hide the title column of each list. If every row looks the same (same tile, same second line, same trailing chevron), the rows are stamped: L12.
- **Cover the lower halves.** Put the top halves of the main screens side by side. If the same hero composition repeats, that is L4.

## Catalog

Run `tells.md` group by group over each screen. Note the ID, the screen and the region. Skip anything on the Not tells list in `SKILL.md`.

## Light, dark, longest strings, Android

- **Dark, control by control:** switch off-state track, chips, segmented controls, input outlines, dividers, pale illustrations and photos, the accent on dark fills. Each control boundary ≥ 3:1 (WCAG). C9 covers the rest.
- **Longest strings:** put the longest real name, price and title in (a German or Vietnamese string is a good test). Then set text to 200% (iOS Larger Text, Android font scale). Look for clipped text, overlaps, and prices, names or dates ending in "…": T8.
- **The fold, the bottom 120 pt:** the first screen ends on a block boundary above the bars. A section heading alone at the fold, or a row half under the tab bar, is L11 or N6.
- **Android size:** the same screens at 412×915. Check that nothing assumed iPhone insets.

## Walk flows

Screenshots miss what happens between them. For each core flow (sign-in, the main task, one edit, one delete):

- tap through to the end, and once with a failure (no network, invalid input, an empty account);
- note where each action lands (N8) and whether a toast covers a changed figure;
- tap every enabled control on the way; one that does nothing is N4.

If you cannot tap (code-only or still screenshots), say so: flows not walked go on the Not verified line.

## Emit

Turn notes into findings in the shape from `SKILL.md`, Step 4:

- at most 8, in tier order, the biggest first impression first;
- one finding per cause: six screens with the same card wrapper is one finding on the shared component;
- drop notes that rest on taste alone, and any rule that holds.
- if more causes exist, end with `Not listed: <n> further tier <X> findings (IDs)`. Tier 1 findings are never cut. Fix the unlisted ones in later rounds only if the user asks;
- `evidence` is `seen`, `measured` or `from code`;
- the observation says what is on screen; the fix is a separate field;
- the fix names a change: a token, a role, a component, a spacing step or a removal. Never an adjective ("make it pop", "cleaner").

Example:
`Home | 3 | L2 Dashboard of identical tiles | first viewport | six equal tiles, each a tinted glyph disc, a label and a number; none leads | hierarchy also fails: the balance reads no stronger than "Rewards" | make the balance a hero figure at display size; move the other five to a list below | seen`
