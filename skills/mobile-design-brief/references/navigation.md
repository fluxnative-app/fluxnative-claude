# Navigation

How a screen is presented tells the person what it is. Choose the presentation for its meaning (or read it from the screens you were given), write it in the brief's route map, and check it by walking the flow with taps. For the router's API, use the Expo router skill if it is installed.

## Laws in brief

- Push goes deeper into the same task.
- A modal is a separate task: Cancel leading, Save trailing, in its header.
- A sheet is a quick choice with a grabber; it dismisses by swipe.
- One-way doors (sign-in done, onboarding done, purchase done) replace the stack and are guarded so back cannot re-enter them.
- Back undoes navigation, never an event.
- Tabs are 3–5 peers, each with icon and label.
- An action lands on the record it changed.
- The tab bar and headers are set once in the navigator's layout; no screen draws its own.

## Presentation as meaning

| Presentation | Means | Header | Leaves by |
|---|---|---|---|
| Tab | a peer area of the app, always one tap away | the area's title | another tab |
| Push | deeper into the same task (list → detail) | back control at the start, title | back control or edge swipe |
| Modal | a separate task that ends in save or cancel (compose, edit, checkout) | Cancel leading, title, Save or Done trailing | Cancel, Save, or swipe down when nothing is unsaved |
| Sheet | a quick choice or a short form that keeps the screen behind in view | grabber; a title only when needed | swipe down, tap outside, or the choice itself |
| Full-screen cover | a step that owns the whole screen (camera, a player, onboarding) | a close control at the start or end | close, or finishing the step |

Checks:
- A pushed screen has one back control, never an X and never two backs.
- A modal has no back arrow; its first header control is Cancel or Close.
- A sheet has a grabber and dismisses by swipe.
- No Cancel or Save buttons in the middle of a scrolling page.

## Reading routes from screens

When the input is a set of mockups, screenshots or existing screens, read each screen's chrome before writing the route map. The chrome says what kind of route it is.

| What the screen shows | What it is |
|---|---|
| The same bottom bar as other screens, with its own item selected | a tab; every screen with that bar is a sibling tab |
| A back arrow or back label at the start of the header | a push child of the screen it was opened from |
| Cancel or Close leading, Save or Done trailing | a modal |
| A grabber, with the screen behind still visible | a sheet |
| A three-line menu icon that opens a side panel | a drawer; for 3–5 destinations use tabs instead (see Tabs) |
| No bar and no back control | a root step: onboarding, sign-in, or a full-screen cover |

Not routes:
- A segmented control or a row of tabs inside the content is a component of that screen. Switching it changes the content, not the route.
- A floating button is an action on the screen. What it opens may be a route (often a modal); the button is not.
- A bottom bar that appears on one screen only is a toolbar for that screen, not a tab bar.

Write each inferred route in the brief with the cue it came from ("Orders: tab, shares the bar with Menu"). When cues disagree (a screen has the tab bar and also a back arrow), pick the reading that keeps tabs as peers, and write it as assumed.

## Chrome once, in the navigator

The tab bar, headers and back controls belong to the navigator, set once in its layout file. With Expo Router that is the `_layout.tsx` of each group, using `Tabs`, `Stack` or `Drawer` and their screen options. A screen never draws them.

- A screen that needs a header button sets it through the navigator (`headerRight` in the screen's options), not as a row of views at the top of the screen.
- The navigator's header and tab bar already keep their own edges clear of the notch and home indicator. A screen adds safe-area insets only for edges no bar covers, from `react-native-safe-area-context`, once per edge (React Native's own `SafeAreaView` is deprecated in 0.86).
- Check in the code: each tab label string appears in one file, the tabs layout (`rg -n "<label>" app src`). A back icon or a tab icon row inside a screen file is drawn chrome; move it to the layout.
- Check in a screenshot: the header and tab bar sit at the same position on every screen of the group, and no screen shows two headers or two bottom bars.

## Tabs

- 3–5 tabs, each a peer area, each with an icon and a one-word label on one line.
- No centre "+" tab; a create action lives in the area it creates for, or in a header control.
- No hamburger menu for 3–5 destinations.
- Tapping the current tab scrolls it to the top or pops it to its root.

## One-way doors

A one-way door is a step the person must not walk back through: sign-in done, onboarding done, a purchase or payment done, an account deleted.

- On passing the door, replace the stack: the screen before the door is no longer in history.
- Guard the routes behind it: a signed-out person cannot reach signed-in routes, and a signed-in person cannot reach sign-in. Use the router's protected or guarded routes if your version has them; otherwise redirect at the layout.
- After payment, back from the confirmation goes to the area's root, never to the basket or the payment form.

## When blocking back is right

Back is blocked in only two cases:

1. **Unsaved input.** Ask first: "Discard changes?" with Discard and Keep editing. Never lose typed input silently, and never block back when nothing changed.
2. **An irreversible step in flight.** While a payment or a delete is being sent, back waits until it finishes or fails, then behaves normally.

In every other case back works, including edge swipe and the Android back button.

## Back undoes navigation, not events

- Back after sending a message returns to the conversation list or stays on the conversation; it never un-sends.
- To undo an event, offer an Undo action in the confirmation (for example, in a toast), not back.

## Where an action lands

- An action lands on the record it changed: after "Save" on an edit, show the saved record; after "Send", show the sent item in its list.
- Never land on a dashboard or overview the action did not change.
- A confirmation toast says only what the screen does not already show, and never covers the figures the action changed. Place it above the bottom bar and clear of the changed row.
- Write the landing route in the brief's route map and check it when walking the flow.

## Non-native smells

Each is visible in a screenshot or one tap:

- The header drawn as a solid coloured block on iOS.
- An X on a pushed screen, or a back arrow on a modal.
- A tab label wrapping to two lines.
- A sheet with no grabber, or one that cannot be swiped away.
- A drawn status bar, notch spacer or home indicator.
- A tab bar or header drawn inside a screen, which shifts or vanishes between sibling screens.
- A second bottom bar stacked on the tab bar.
- Content hidden under the header or the tab bar; the last row cannot scroll clear of the bottom bar.
- Two controls that both go back.
