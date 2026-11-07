# Capture

Take shots the review can trust, and say where they came from.

## Sources, best first

1. a release build on a phone;
2. a simulator or emulator (`xcrun simctl io booted screenshot`, `adb exec-out screencap -p`);
3. Expo Web in a browser tool at 393×852 and device pixel ratio 2, with dark mode emulated;
4. screenshots the user supplies.

Take two kinds of shot: a show shot (one viewport, for the person and the first impression)
and a full-height review shot (or the page set) to find problems in. Never report content as
missing from a viewport crop: check the element tree or the source first (below).

Write what ran the app at the top of the report; `verify-without-device.md`
says what each source can prove. "Code only" is not a review source: with no shots, ask for 2x
screenshots or a way to run the app. A code-only reading is a deslop count, not a review.

## Rules for every source

- 2x pixels. At 1x a model misreads small images and thin text.
- One device size per set: 393×852 pt (a current iPhone) or 412×915 dp (a current Android
  phone).
- Label each file by route, scheme and page: `orders-light-p1.png`.
- Light and dark for every screen you review.
- Remove debug overlays and dev menus before shooting.
- Write the source at the top of the report as the "Ran on" line (release build on a phone,
  simulator or emulator, Expo Web, or user screenshots), and claim only what ran there.

## Two kinds of shot

| Kind | What it is | Use it for |
|---|---|---|
| Show shot | One viewport, as the person sees it on opening the screen | The report, the first impression (page 1), the cold-read panel |
| Review shot | The whole screen top to bottom: one full-height capture, or the page set from "Long screens" | Finding problems below the fold, the list end, the last row above a bar |

- Review from the review shot; put the show shot in front of the person.
- A browser's full-page option often returns only the viewport on Expo Web, because the list
  scrolls inside its own element, not the page. If the capture is no taller than the viewport,
  shoot page by page instead.
- **Never report content as missing because a shot does not show it.** A viewport crop hides
  everything below the fold. Before a "missing" or "not on screen" finding, check the element
  tree or page text (browser tool's accessibility tree or text read; Android
  `adb shell uiautomator dump /sdcard/window.xml`, then `adb pull /sdcard/window.xml`), and
  the screen's source. Report it missing only when the rendered tree lacks it. If the tree has
  it below the fold, the finding (if any) is about order, not absence. With user screenshots
  and no tree or source, ask whether the screen scrolls further, or put it under Not verified.

## Expo Web in a browser tool

Start the web build the way the project does (often `npx expo start --web`), then in your
browser tool:

- set the viewport to 393×852 with device pixel ratio 2 (mobile emulation if the tool has it);
- open each route by URL;
- dark mode: emulate `prefers-color-scheme: dark` (the browser tool's colour-scheme setting or
  its devtools rendering option), then reload;
- if the app keeps its own theme setting, switch it in the app as well.

A web render proves layout, copy, contrast and dark mode. It does not prove device fonts,
native controls, glass or motion (`verify-without-device.md`).

## iOS simulator

```
xcrun simctl io booted screenshot orders-light-p1.png
xcrun simctl ui booted appearance dark
xcrun simctl ui booted appearance light
```

Screenshots come out at the device's native scale (2x or 3x). Larger text, about 200% for body
text: `xcrun simctl ui booted content_size accessibility-large`; the maximum, about 310%, is
`accessibility-extra-extra-extra-large`. Back with `large`.

## Android emulator or device

```
adb exec-out screencap -p > orders-light-p1.png
adb shell cmd uimode night yes
adb shell cmd uimode night no
```

Larger text: `adb shell settings put system font_scale 2.0`, and back with `1.0`.

## Screenshots from the user

- Ask which device and scheme they came from if the file does not say. If nobody knows, treat
  them as `verify-without-device.md` says for screenshots of unknown origin.
- Judge only what they show. Flows, other schemes and other screens go on the Not verified
  line.
- Do not upscale a 1x shot. Judge tier 1 layout problems and structure only, say that details
  and thin text were too small to judge, and ask for a 2x shot.

## Long screens

- Shoot page by page: scroll one viewport height minus about 80 pt of overlap per page.
- Page 1 is the first impression. Judge it as a screen of its own.
- Check the fold on page 1: the first screen ends on a block boundary above the bars, not on a
  section heading alone or text cut by a bar.
- Scroll to the very end once: the last item sits fully above any bar.

## Longest strings and large text

For at least one busy screen, shoot it with:

- the longest real names and labels the app can get (or the longest language it ships);
- text near 200% (Android `font_scale 2.0`, iOS `accessibility-large` or larger).

## Over the FluxNative MCP

For a FluxNative app (the marker in SKILL.md section 11): `preview_view` takes shots (with no
`mode` it returns the element tree, the check before any "missing" finding), `preview_act`
walks flows, and `preview_act` `action: "resize"` with `colorScheme: "dark"` sets dark mode,
one action at a time. Write "Ran on: FluxNative web preview (Expo Web export)" and take Not
verified from the "Headless Expo Web" column. See the `fluxnative-mcp` skill, if installed.
