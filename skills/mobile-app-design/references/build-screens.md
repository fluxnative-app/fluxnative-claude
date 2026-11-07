# Build screens

From a brief, an idea, a mockup, a screenshot, a Figma frame or an HTML page to React Native
screens. Work in this order: inputs → route map → token module → navigator → shared parts →
screens → states. Do not start a screen file before the route map and the token module exist.

## 1. Read the inputs

| Input | Take from it | Leave |
|---|---|---|
| Brief or idea | screens, each screen's job and lead, the route map, tokens | features nobody asked for |
| Mockup or screenshot | the screen list, the chrome on each screen, hex values, type sizes, radii, spacing, real copy | status bar, notch, home indicator, device frame, browser chrome |
| Figma frame | the same, plus named styles and auto-layout direction (it maps to `flexDirection`) | frame names as route names without checking them |
| HTML or a web page | structure, copy, the CSS values (convert them: rn-silent-failures.md) | hover-only states, `position: fixed`, grid, CSS variables |

- A reference from another product gives structure, spacing and reading order only. Never its
  palette, brand, copy or imagery.
- Write down what the images do not say: what happens on tap, what a list shows at zero items,
  where an action lands. Decide each one; do not leave it to chance in code.

## 2. Infer the route map

Lay the screens side by side and read their chrome.

| You see | It means | In Expo Router |
|---|---|---|
| The same bottom bar on several screens, a different item highlighted | tab siblings, one per bar item | one `Tabs` layout, one file per tab |
| A back arrow at the start of the header | a push child of the screen it came from | a file in that tab's `Stack` |
| Cancel or a close X at the top, Save or Done opposite | a modal: a separate task | a `Stack.Screen` with `presentation: 'modal'` |
| A grabber, the screen behind still visible | a sheet: a quick choice | `presentation: 'formSheet'` with `sheetAllowedDetents` |
| A hamburger icon | a drawer, but with 3 to 5 destinations prefer tabs | `expo-router/drawer` only when the brief keeps it |
| No chrome at all, before the main app | a one-way door (sign-in, onboarding) | a route outside the tabs, guarded so back cannot return |

These are parts of a screen, not routes:

- tabs or segmented controls inside the content;
- a floating action button;
- a carousel, an accordion, a filter row, a search field;
- a dialog that asks one question.

Write the map before code, one line per route:

```
/(tabs)/index        tab    Home: lead = this month's balance
/(tabs)/orders       tab    Orders: lead = the open order
/orders/[id]         push   Order detail; Save lands here
/orders/new          modal  New order; Cancel leading, Save trailing
/filter              sheet  Order filter; dismiss by swipe
/sign-in             door   replaced by /(tabs) on success
```

Count the tabs: 3 to 5, each an icon and a one-word label. Two items or six are a sign the map is
wrong, not that the bar needs a new style.

## 3. Draw the chrome once

- The tab bar lives in the `Tabs` layout's options. Headers live in the `Stack` layout's
  `screenOptions` or in each screen's `Stack.Screen` options. Screens never render their own
  tab bar, a header copy, a status bar or a home-indicator spacer.
- Check: the tab bar component (or `Tabs`) is imported in exactly one layout file.
  `rg -l -w Tabs app -g '*.tsx'` lists only `_layout.tsx` files.
- Safe areas: the navigator header owns the top inset, the tab bar owns the bottom inset. A
  screen adds an inset only on an edge no bar covers (rn-silent-failures.md, item 4).

## 4. One token module

One file (for example `src/theme/index.ts`) exports every colour, type role, radius and spacing
step, with a light and a dark value for each colour role:

```ts
import { useColorScheme } from 'react-native';

const light = { ground: '#F6F5F2', surface: '#FFFFFF', ink: '#1C1B19', muted: '#6B6862', accent: '#1F6F5C', danger: '#B3261E' };
const dark  = { ground: '#121211', surface: '#1C1B1A', ink: '#ECEAE6', muted: '#A19E98', accent: '#5CC3A6', danger: '#F2B8B5' };
export const type = { display: 34, title: 22, body: 17, label: 15, caption: 13 };
export const space = { xs: 4, s: 8, m: 12, l: 16, xl: 24, xxl: 32 };
export const radius = { s: 6, m: 12, l: 20 };
export function useColors() { return useColorScheme() === 'dark' ? dark : light; }
```

- Use the project's existing token file if it has one; add roles to it rather than a second file.
- Screens import roles; they hold no `#hex`, `rgb(` or `rgba(` literal. Manual check (a fallback,
  not a measurement): `rg -n -i "#[0-9a-f]{3,8}\b|rgba?\(" app src -g '*.tsx' -g '!**/theme/**'`
  returns nothing. If `mobile-deslop` is installed, its counts are the measured check.
- Every hex is one the React Native parser accepts (rn-silent-failures.md, item 8).
- Record the contrast ratio of each text colour on its ground in the brief (body text at least 4.5:1).

## 5. Shared parts, then screens

- Build the parts several screens use first: a list row, a section header, a lead figure, an
  empty state, a skeleton row. A fix to one then fixes every screen.
- Build screens in route-map order. After each one, check it against its brief line: the lead is
  what reads first, and the job can be done from this screen.
- Map elements, do not translate tags one by one:

| Web or design | React Native |
|---|---|
| `div` | `View` |
| text in `p`, `span`, `h1` | `Text` (all text, always) |
| `img` | `Image` from `expo-image`, with a width and height or an `aspectRatio` |
| `button`, a tappable card | `Pressable`, with a pressed style |
| a long list | `FlatList`, or `FlashList` when the project has it |
| a scrolling page | `ScrollView` |
| `input`, `textarea` | `TextInput` |
| `select` | a sheet or the platform picker |

- Keep platform controls (switch, date picker, menus, sheets). Do not redraw them.

## 6. Decide every screen's states

A mockup shows one good moment. Before writing a screen that shows data, fill this table for it,
in the brief or in a comment at the top of the screen file:

| State | Decide | Check |
|---|---|---|
| Zero items | the screen's own layout at zero, one sentence, one action that fills it | not a lone icon in the middle |
| One item | the list does not look broken with one row; totals still read | no grid hole, no "1 items" |
| Many items, longest text | rows wrap or truncate where you chose; prices and dates are never cut | longest real string and 200% text |
| Loading | a skeleton with the loaded layout's rows and heights | no full-screen spinner |
| Error | what happened, what to do, Retry, inline where the data goes | no codes or provider names |
| Reopening a saved record | the detail or edit route loads by id on a cold open or a link; the form shows the saved values | open the route directly by URL or deep link |

A screen is not done until each row is shown in a render or listed under Not verified.

## 7. Then

Go to Review in SKILL.md. Before Verify, re-read rn-silent-failures.md against the code you wrote.
