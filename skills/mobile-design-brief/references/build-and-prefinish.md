# Build rules and pre-finish

## Rules the build keeps

Each can be checked in a screenshot or the code.

- One display moment per screen.
- Text is start-aligned. Centre only a single-purpose moment (a success screen, an empty state).
- Strong colour: by eye, no more than one filled block or one filled button per viewport, plus the accent on 1–3 small elements. The percentage is a house heuristic, not a test.
- At most one filled primary button per viewport.
- Content rows 64–76 pt (60 only when one line); settings rows 48–56 pt (house heuristic). Read the height from the style (paddingVertical + line height), or measure a 2x shot and divide by 2.
- Touch targets: 44 pt minimum on iOS, 48 dp on Android; in one shared style use 48.
- No fixed heights on text containers; text scales with the system setting. Changing or aligned numbers use tabular figures. In code: `fontVariant: ['tabular-nums']` on those text styles.
- Every data screen shows its zero, one, loading, error and reopen states as the brief's States table says, in light and dark (`states.md`).
- Every enabled control does what it says. Every form has its submit.

## Build

If `expo-design-system` is installed, its token module is the theme: extend it, never add a second one. If `mobile-app-design` is installed, hand the brief to it for building. Otherwise build screens in route-map order. After each screen, check it against its brief entry: the lead is the lead, the claim is visible, the proof is on screen.

## Pre-finish

1. Run the `mobile-deslop` pre-flight count. If that skill is not installed, run these from the project root and look at each hit:
   ```
   rg -o --no-filename -i "#[0-9a-f]{3,8}\b|rgba?\([^)]*\)" app src -g '*.tsx' -g '*.ts' -g '!**/theme/**' -g '!**/theme.ts' | sort | uniq -c | sort -rn
   rg -o --no-filename "borderRadius:\s*\d+" app src -g '*.tsx' -g '*.ts' | sort | uniq -c
   rg -o --no-filename "fontSize:\s*\d+" app src -g '*.tsx' -g '*.ts' | sort | uniq -c
   rg -n '[\x{1F000}-\x{1F2FF}\x{1F300}-\x{1FAFF}\x{2600}-\x{27BF}]' app src -g '*.tsx'
   rg -n "on[A-Z]\w*=\{\(\)\s*=>\s*\{\s*\}\}" app src -g '*.tsx'
   ```
   Adjust the folders and the theme exclusions to the project. The counts are facts, not a score: a hit tells you where to look, and you decide.
2. Walk each core flow by taps from start to end, including one failure (offline, invalid input or an empty account). Note where each action lands. No simulator: export Expo Web (`npx expo export --platform web`), open it in a headless browser at 393×852 and screenshot each route; if that is impossible, review the code only and say so in Report. Then list the walk steps as "not walked" under Not verified.
3. Check the longest real strings, the largest text size and dark mode on every screen in scope.
4. Check that each part of the user's request is visible on a screen.

Write each failure as one line, `screen | what is wrong | fix`; use the full finding shape from `mobile-screen-review` if it is installed. Fix what fails, take a new render, and rule each finding `fixed` (naming what the screen now shows) or `open`.

## Done when

- Every screen in scope has a job, a claim and a lead in the brief, and the render shows them.
- The tokens are in the theme, and screens use them.
- The critique names what moved away from the category default.
- Every route in the map is reachable by taps.
- Every data screen has a filled row in the States table, and each state was forced once and seen (or listed under Not verified).
- At most 3 render rounds have run.
