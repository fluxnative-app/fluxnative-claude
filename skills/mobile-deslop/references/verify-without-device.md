# What each evidence source can prove

Name your evidence source on the first line of every report: `Ran on: <source>`. Claim only what ran there. Then use this table: anything marked **cannot** for your source goes on the report's last line, `Not verified: …`, in those words. Anything marked **partly** goes there too, with what was not covered.

Sources:
- **Code only**: you read the source; nothing ran.
- **Headless Expo Web**: the app's web export in a browser tool, at 393×852 and DPR 2 (412×915 for an Android size).
- **Simulator or emulator**: an iOS simulator or Android emulator running a debug or release build.
- **Release build on a phone**: a release build on a real device. Feel is judged on the slowest device you have.

Screenshots the user gave you count as the source they came from. If you do not know, treat them as "simulator or emulator" for layout and "cannot" for everything that moves.

| Check | Code only | Headless Expo Web | Simulator or emulator | Release build on a phone |
|---|---|---|---|---|
| Layout and hierarchy | cannot | proves (web fonts) | proves | proves |
| Contrast | partly (token pairs only) | proves | proves | proves |
| Clipped or truncated text | cannot | partly (web text metrics) | proves | proves |
| Dark mode | partly (both palettes exist) | proves (emulated scheme) | proves | proves |
| Content under bars and safe areas | partly (insets are used) | partly (no real notch or home indicator) | proves | proves |
| Flows by taps (landing, back, dismiss) | cannot | proves for JavaScript navigation | proves | proves |
| Device fonts and Dynamic Type | cannot | cannot | proves | proves |
| Glass and blur | cannot | cannot | partly | proves |
| Native sheets, pickers, menus and switches | cannot | cannot (web stand-ins) | proves | proves |
| Keyboard covering inputs | cannot | cannot | partly | proves |
| Haptics | cannot | cannot | cannot | proves |
| Motion feel at 60 fps | cannot | cannot | partly (debug builds run slower) | proves |
| Native modules | cannot | cannot | partly | proves |
| Push notifications | cannot | cannot | partly | proves |
| Purchases | cannot | cannot | partly (local test configuration) | proves with a sandbox account |
| Camera | cannot | cannot | partly (Android emulator's virtual camera; none on iOS simulator) | proves |
| Permission prompts | cannot | cannot | proves | proves |

Rules:
- A successful write, a passing type check or a build that compiles is not evidence of how a screen looks or behaves.
- Never claim a device result from a web render. Write "on Expo Web" next to any web result.
- Judge screenshots at 2x. At 1x, small images and thin text are misread.
- A shot shows only its own frame. Never report content as missing from a crop: check the element tree, the page text or the source first. With still shots only, write "not seen in the shots", not "missing".

## Capture

Two kinds of shot:

- **Review shot**, for you: the whole screen, top to bottom. On Expo Web a full-page capture often stops at one viewport, because the app scrolls inside the page; then shoot it one viewport at a time.
- **Show shot**, for the person: one viewport, as a phone shows it. Use these for before and after.

Commands:

- **Expo Web:** open the web export in a browser tool at 393×852, device pixel ratio 2. For dark mode, emulate `prefers-color-scheme: dark` if the app follows the system scheme.
- **iOS Simulator:** `xcrun simctl io booted screenshot shot.png`; dark mode with `xcrun simctl ui booted appearance dark`.
- **Android emulator:** `adb exec-out screencap -p > shot.png`; dark mode with `adb shell cmd uimode night yes`.
- **Screenshots from the person:** ask which device and scheme each shows. You cannot tap or scroll them, so flows go on the Not verified line.

Label each shot by route and scheme.
