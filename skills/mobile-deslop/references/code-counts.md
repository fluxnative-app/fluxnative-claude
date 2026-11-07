# Pre-flight code counts

Run these from the project root. Each block gives the `rg` command, the usual range, the known false positives and a `grep` fallback for machines without ripgrep.

- Scope: `app` and `src`, TypeScript files, the theme folder left out. If screens live elsewhere (`screens/`, `components/`), add those paths.
- Tests and fixtures are not the UI. Add `-g '!**/*.test.*' -g '!**/__tests__/**' -g '!**/fixtures/**'` when they show up in the hits.
- Usual ranges are house heuristics from reviewing generated apps, not platform rules.
- The `grep` fallbacks use `-E`; the emoji one needs GNU grep's `-P`.

## The count table

Fill this at Step 1 and keep it: the report shows it before and after.

| Count | Usual range (house heuristic) | Found |
|---|---|---|
| hex colours outside the theme | ≤ 8 distinct | |
| distinct `borderRadius` literals | ≤ 4, from one scale | |
| distinct `fontSize` / `fontWeight` | ≤ 6 / ≤ 3 | |
| files with a gradient (then count uses per screen) | 0, or 1–2 the brand needs | |
| emoji in JSX text (raw / kept) | 0 kept | |
| `textTransform: 'uppercase'` | ≤ 2 per screen | |
| lines joined with "·" | < 3 per screen | |
| empty handlers | 0 | |
| `Alert.alert('Success'…)` with no write before it | 0 | |
| `ActivityIndicator` outside buttons and pull-to-refresh | 0 | |
| `allowFontScaling={false}` on content | 0 | |
| looping animations (`withRepeat`, `Animated.loop`) | 0 without a stop | |
| placeholder hosts and names | 0 | |

In a FluxNative app the target for hex colours, font sizes and radii outside `src/theme` is 0, not the usual range, and so is the FluxNative block below (`Alert.alert`, `Share.share`, look `Platform.OS` branches).

Then add the two counts that need a screenshot (last section).

## Hex colours outside the theme (C5, C6, C11)

```sh
rg -o --no-filename -i "#[0-9a-f]{6}\b|#[0-9a-f]{3}\b" app src -g '*.tsx' -g '*.ts' -g '!**/theme/**' | sort | uniq -c | sort -rn
```

- Usual range: ≤ 8 distinct values.
- False positives: colours in test fixtures, SVG assets or chart sample data.
- Fallback: `grep -rhoiE '#[0-9a-f]{6}\b' app src --include='*.ts' --include='*.tsx' --exclude-dir=theme | sort | uniq -c | sort -rn`

Also look at `rgb(` and `rgba(` literals with the same scope.

## Raw colour literals in screens (C5, C11): manual fallback

Every colour a screen uses should come from one token module (the theme) by role name. Use this check when you have code only, or to find where a colour seen in a shot comes from. The screenshot counts below come first; this does not replace them.

```sh
rg -n -i "#[0-9a-f]{3,8}\b|rgba?\(|hsla?\(" app src -g '*.tsx' -g '!**/theme/**' -g '!**/*.test.*' -g '!**/fixtures/**'
```

- Expected: no hits in screen and component files. Each hit is a place to look; a brand mark drawn in its own fixed colours may stay, with a comment saying so.
- False positives: an issue number in a comment (`#185`); a shadow or scrim defined once in a shared component; an SVG brand mark; chart sample data. Named colours (`'white'`) are not caught; look for them by eye.
- Fix: move the value into the theme under a role name (`accent`, `textMuted`, `border`), then use the role. Do not add a second theme.
- Fallback: `grep -rniE '#[0-9a-f]{3,8}\b|rgba?\(|hsla?\(' app src --include='*.tsx' --exclude-dir=theme`

## Distinct radii (L9)

```sh
rg -o --no-filename "[bB]order\w*Radius:\s*\d+" app src -g '*.tsx' -g '*.ts' -g '!**/theme/**' | sort | uniq -c | sort -rn
```

- Usual range: ≤ 4 distinct values, from one scale.
- False positives: `0`, a full pill (`999`, `9999`), a circle computed as `size / 2`.
- Fallback: `grep -rhoE '[bB]order[A-Za-z]*Radius: *[0-9]+' app src --include='*.ts' --include='*.tsx' --exclude-dir=theme | sort | uniq -c`

## Font sizes and weights (T1, T2, T3)

```sh
rg -o --no-filename "fontSize:\s*\d+" app src -g '*.tsx' -g '*.ts' -g '!**/theme/**' | sort | uniq -c | sort -rn
rg -o --no-filename "fontWeight:\s*['\"]?\w+" app src -g '*.tsx' -g '*.ts' -g '!**/theme/**' | sort | uniq -c | sort -rn
```

- Usual range: ≤ 6 sizes, ≤ 3 weights.
- False positives: sizes inside a chart library's config; an icon's `size` prop is not a font size.
- Fallback: `grep -rhoE 'font(Size|Weight): *[^,}]+' app src --include='*.ts' --include='*.tsx' --exclude-dir=theme | sort | uniq -c`

## Gradients (C1, C3)

```sh
rg -l "LinearGradient|RadialGradient|linear-gradient|radial-gradient" app src -g '*.tsx' -g '*.ts'
```

- Usual range: 0 files, or 1–2 the brand needs.
- False positives: a skeleton shimmer; a scrim under text on a photo (that one is right).
- Fallback: `grep -rlE 'LinearGradient|linear-gradient' app src --include='*.ts' --include='*.tsx'`

## Emoji in JSX (I1)

```sh
rg -n '[\x{1F000}-\x{1F2FF}\x{1F300}-\x{1FAFF}\x{2600}-\x{27BF}]' app src -g '*.tsx'
```

- Usual range: 0 kept hits.
- False positives: an emoji the user typed as content (a chat message, a note); a check mark or star used as a text glyph.
- Report raw hits and kept hits separately: kept hits are the ones that act as an icon or decoration. Only kept hits count against the range, so check marks and stars typed as text do not fail it.
- The ranges: Mahjong, domino and enclosed signs (U+1F000–1F2FF), pictographs and emoji (U+1F300–1FAFF), and the older symbols and dingbats (U+2600–27BF). Without ripgrep, search the same ranges with any PCRE-capable search tool.

## Tracked capitals (T4)

```sh
rg -n "textTransform:\s*['\"]uppercase" app src -g '*.tsx' -g '*.ts'
rg -n ">\s*[A-Z][A-Z0-9 ]{5,}\s*<" app src -g '*.tsx'
```

- Usual range: ≤ 2 per screen.
- False positives: acronyms and codes (PIN, OTP, a booking reference); a theme role used for one overline.
- Fallback: `grep -rnE "textTransform: *['\"]uppercase" app src --include='*.ts' --include='*.tsx'`

## Dot-joined meta (D7)

```sh
rg -n '·|\\u00b7| • ' app src -g '*.tsx' -g '*.ts'
```

- Usual range: fewer than 3 lines per screen.
- False positives: one date-and-time join in a header; a separator inside a translation file.
- Fallback: `grep -rnE '·| • ' app src --include='*.ts' --include='*.tsx'`

## Empty handlers (N4)

```sh
rg -n "on[A-Z]\w*=\{\(\)\s*=>\s*\{\s*\}\}" app src -g '*.tsx'
rg -n "on[A-Z]\w*=\{\(\)\s*=>\s*(null|undefined|console\.log\([^)]*\))\}" app src -g '*.tsx'
```

- Usual range: 0.
- False positives: a no-op that stops a parent press from firing (it should carry a comment saying so).
- Fallback: `grep -rnE 'on[A-Z][a-zA-Z]*=\{\(\) *=> *\{ *\}\}' app src --include='*.tsx'`

## Success alert with no write (N4)

```sh
rg -n -B6 "Alert\.alert\(\s*['\"](Success|Saved|Done|Sent|Booked)" app src -g '*.tsx' -g '*.ts'
```

- Usual range: 0 alerts with no awaited write in the lines above.
- Read the six lines before each hit: an `await` on a save, a request or a mutation means it is real.
- False positives: a write in a called helper; follow the call once.
- Fallback: `grep -rnE -B6 "Alert\.alert\( *['\"](Success|Saved|Done)" app src --include='*.ts' --include='*.tsx'`

## Spinner as loading (S1)

```sh
rg -n "<ActivityIndicator" app src -g '*.tsx'
```

- Usual range: 0 outside buttons and pull-to-refresh.
- Check the parent of each hit: inside a button or a control is right; as a screen's whole body is S1.
- False positives: a spinner inside a shared button component.
- Fallback: `grep -rn '<ActivityIndicator' app src --include='*.tsx'`

## Font scaling disabled (T8)

```sh
rg -n "allowFontScaling=\{false\}|maxFontSizeMultiplier=\{1(\.0)?\}" app src -g '*.tsx'
```

- Usual range: 0 on content text.
- False positives: fixed chrome labels such as a tab label or a badge, where a cap around 1.3–1.5 is acceptable.
- Fallback: `grep -rnE 'allowFontScaling=\{false\}' app src --include='*.tsx'`

## Looping animations (M1)

```sh
rg -n "withRepeat\(|Animated\.loop\(|iterationCount=['\"]infinite|loop=\{true\}|<LottieView" app src -g '*.tsx' -g '*.ts'
```

- Usual range: 0 without a stop condition and a Reduce Motion check.
- Lottie animations loop by default: check each `LottieView` hit for `loop={false}` or a stop.
- False positives: a skeleton shimmer that stops when data arrives and respects Reduce Motion; a loader inside a button.
- Fallback: `grep -rnE 'withRepeat\(|Animated\.loop\(' app src --include='*.ts' --include='*.tsx'`

## Placeholder hosts and names (D8, I5, D1)

```sh
rg -n -i "picsum\.photos|placehold\.co|placeholder\.com|pravatar\.cc|loremflickr\.com|unsplash\.it|dummyimage" app src -g '*.tsx' -g '*.ts'
rg -n 'John Doe|Jane Doe|Lorem|Item [0-9]|Description here|\$99\.99|1,234' app src -g '*.tsx' -g '*.ts'
```

- Usual range: 0.
- False positives: unit-test fixtures; a real product that costs 99.99.
- The hosts are spelled out so a text field's `placeholder` prop does not match.
- Fallback: `grep -rniE 'picsum\.photos|placehold\.co|placeholder\.com|pravatar\.cc|loremflickr\.com|John Doe|Lorem' app src --include='*.ts' --include='*.tsx'`

## FluxNative apps only

Run these only when the FluxNative marker matches (`fluxnative-apps.md`).

```sh
rg -n 'Alert\.alert|Share\.share|Platform\.OS' app src -g '*.tsx' -g '!**/platform/**'
rg -n '#[0-9a-fA-F]{3,8}\b' src/features src/components
```

- Usual range: 0.
- False positives: a `Platform.OS` branch for behaviour (insets, keyboard, edge-to-edge), not look.

## Counts that need a screenshot

- Filled primary buttons per viewport: usually 1 (N5).
- Non-neutral hues outside photos: 1 action hue, plus status hues and labelled category hues (C5).
- Accent uses per viewport: 1–3 (C6).

Measure them per viewport on the baseline shots (`screenshot-pass.md`, Baseline), at the end of Step 2 and before any change; with code only, write `n/a, code only`. Count filled primary buttons by looking at each shot. Count hues by naming each distinct non-neutral colour you see.

With code only, or to find where a colour in a shot comes from, run the raw-colour check above (screens should hold no raw colour literals). It is a fallback, not a replacement for the screenshot counts.

---

The counts are facts, not a score. A count outside its usual range is a reason to look at that spot, and you decide. A gradient in a brand built on one is right.
