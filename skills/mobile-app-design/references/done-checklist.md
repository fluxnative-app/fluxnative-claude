# Done checklist

Tick each item with its evidence, or move it to the report's Not verified line with the reason.
Run the commands from the project root; change `app src` to the folders that hold screens.
A hit is a reason to look at that line; you decide whether it is wrong.

## Code

- [ ] Type check, lint and tests pass (for example `npx tsc --noEmit`, `npx expo lint`, `npm test`).
- [ ] No colour literal in screens. Returns nothing:
      `rg -n -i "#[0-9a-f]{3,8}\b|rgba?\(" app src -g '*.tsx' -g '!**/theme/**'`
- [ ] No colour the parser drops. Returns nothing:
      `rg -n -i "\b(oklch|lab|lch|color-mix)\(|hsla?\(\s*\d[^)]*deg|var\(--|['\"]currentcolor" app src -g '*.ts' -g '*.tsx'`
- [ ] No core `SafeAreaView`. Returns nothing:
      `rg -U -n "SafeAreaView[^;]*from ['\"]react-native['\"]" app src -g '*.tsx'`
- [ ] The tab bar is drawn once. Lists only layout files:
      `rg -l -w Tabs app -g '*.tsx'`
- [ ] No empty handlers. Returns nothing:
      `rg -n "on[A-Z]\w*=\{\(\)\s*=>\s*\{\s*\}\}" app src -g '*.tsx'`
- [ ] No placeholder content. Returns nothing:
      `rg -n -i "lorem|john doe|item [0-9]|coming soon" app src -g '*.ts' -g '*.tsx'`
- [ ] No number left of `&&` in JSX (item 3 of rn-silent-failures.md). Each hit read:
      `rg -n "\{\s*[\w.]+\s*&&\s*<" app src -g '*.tsx'`
- [ ] Every item in rn-silent-failures.md re-read against the code written in this task.

## Routes and states

- [ ] Every route in the route map exists, with the presentation the map gives (tab, push, modal, sheet).
- [ ] Every route is reachable by taps from the first screen, and back and dismiss go where the map says.
- [ ] A one-way door (sign-in, onboarding, purchase done) cannot be re-entered with back.
- [ ] Every data screen has its states decided (build-screens.md, section 6), and each one is
      shown in a render or listed under Not verified: zero items, one item, longest text,
      loading, error, reopening a saved record.
- [ ] Every enabled control does what it says; every form has its submit.

## Screens

Two kinds of screenshot, at device pixel ratio 2 (393×852 for an iPhone size, 412×915 for Android):

| Kind | What | Use |
|---|---|---|
| Show shot | one viewport, as the person sees it on opening the screen | the report and the person |
| Review shot | the whole screen, top to bottom: a full-height capture, or pages with about 80 pt overlap | judging, including the end of every list |

- [ ] A show shot and a review shot of each screen built, in light and dark.
- [ ] Judged from the review shot. A web build's full-page capture often returns only the
      viewport when the list scrolls inside its own element; if the capture is no taller than
      the viewport, shoot page by page.
- [ ] Nothing reported as missing from a viewport crop. Before saying an element is missing,
      read the element tree or page text (the browser tool's accessibility tree, or
      `adb shell uiautomator dump`) and the screen's source.
- [ ] One element leads on each screen, and it is the lead the brief names.
- [ ] At most one filled primary button per viewport.
- [ ] Nothing under the status bar, header, tab bar or home indicator; the last row scrolls clear of the bottom bar.
- [ ] No clipped price, name or date with the longest real string and text near 200%.
- [ ] Dark mode checked control by control: switches, chips, outlines, pale images.
- [ ] Each part of the request is visible on a screen.

## Ran on

- [ ] The report starts with one line naming what actually ran the app:
      `Ran on: Expo Web at 393×852 DPR 2`, `Ran on: iOS simulator <model>`,
      `Ran on: Android emulator <model>`, `Ran on: <device model>`, or `Ran on: code only`.
- [ ] Every claim in the report was exercised on that source. Android edge-to-edge, shadows,
      device fonts, native sheets and the keyboard are not proven by a web render.
- [ ] The Not verified line lists every "cannot" for that source in verify-without-device.md,
      plus screens not reached and states not shown.
- [ ] At most 3 rounds of fix, re-render and re-check have run.
