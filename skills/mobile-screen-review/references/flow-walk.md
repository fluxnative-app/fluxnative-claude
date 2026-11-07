# Flow walk

Screenshots of single screens pass while a flow is broken. Walk each core flow to its end.

## Pick the flows

- The flows the brief or request names, or the 3–5 things the app exists for (order, book,
  log, pay, send).
- At most 5 per pass.
- Each flow runs once as intended and once with a failure.

## Record format

One line per step:

```
F1 order a coffee
1  /menu            tap "Flat white"           → /item/flat-white, price shown
2  /item/flat-white tap "Add"                  → toast "Added", stays on item; cart badge 1
3  /item/flat-white tap cart                   → /cart, 1 line, total equals line + tax
4  /cart            tap "Pay"                  → /order/1042, status "Preparing"   LANDING OK
5  /order/1042      back                       → /menu (cart cleared)              ONE-WAY OK
F1-fail
3  /cart            offline, tap "Pay"         → inline error at the button, cart kept   OK
```

Mark each step `OK` or name the finding (with its tell ID if one fits).

## Failures to try

- **Offline:** turn off the network (browser tool offline mode, simulator network link
  conditioner, emulator airplane mode) before the step that loads or sends.
- **Invalid input:** an empty required field, a too-long name, a bad email, letters in an
  amount.
- **Empty account:** a fresh install or cleared storage; every list screen at zero.

Each failure gives one signal at its location (not a banner plus a toast plus an inline
message), says what to do, and keeps what the person typed.

## Meaning checks

| Check | Right | Finding |
|---|---|---|
| Push vs modal | Push goes deeper and has back. On iOS, a modal is a task with Cancel leading and Save trailing. | An X on a pushed screen; a back arrow on a modal; Cancel and Save in the page body. |
| One-way doors | After sign-in, onboarding or a purchase, back does not return to it. | Back re-opens onboarding or the paywall. |
| Landing | An action lands on the record it changed (the new order, the edited entry). | Send returns to an overview; the person has to find the record. (N8) |
| Toasts | A toast says only what the screen does not show, and never covers the figures it changed. | A toast sits over the updated total. (N8) |
| Sheets | A sheet dismisses by swipe down and by tapping outside; a grabber where the platform draws one. Android: system back closes the sheet. | A sheet that only closes with a button. |
| Back | Back undoes navigation, never an event. | Back cancels a payment or deletes a draft without asking. |
| Blocking back | Only for unsaved input (ask first) or an irreversible step in flight. | Back disabled on a plain screen. |

## Every enabled control works

Tap every enabled control on the flow's screens. Each one does what its label says.

- A control that does nothing is tier 1 (`N4`).
- A success message with no change behind it is tier 1.
- A form without a submit control is tier 1.
- A disabled-looking control that is enabled, or the reverse, is tier 1.

## Ask before live data

Taps that send, pay, delete or post may hit a real backend. Ask the user before those steps,
or use test data they name. Stop before the final step that pays, sends or deletes unless the
user confirms test mode or test data, and record it as `not tapped: <reason>`. Never type real
personal or payment data.

## What drives the taps

Without a dedicated app-driving tool:

- **Expo Web in a browser tool:** click and type through the browser tool's own actions. Good
  for JavaScript flows; native modules have web stand-ins or none.
- **Android emulator:** `adb shell input tap <x> <y>`, `adb shell input text "<text>"`,
  `adb shell input keyevent KEYCODE_BACK`. Coordinates are in device pixels.
- **iOS simulator:** `xcrun simctl` takes screenshots and sets appearance but does not tap. Use
  a UI test runner the project already has, or ask the user to run the flow and send a shot per
  step.
- **No driver at all:** read the route map in code to list the flows, then write
  "Flows not walked: F1–F3" on the Not verified line. Never describe a flow as working from
  code alone.

## Per flow

For each core flow (at most 5):

- record each step as `route → action → result`, including one failure (offline, invalid
  input or an empty account);
- check back and dismiss at every step;
- check that a one-way door (sign-in done, onboarding done, purchase done) cannot be re-entered
  with back;
- tap every enabled control on the way. A control that does nothing is tier 1.

Ask before any tap that writes to live data.

- Stop before the final step that pays, sends or deletes unless the user confirms test mode or
  test data; write that step as "not tapped: <reason>".
- Flow steps need a 2x shot only where you write a finding.
- If you cannot tap, ask the user to run the flow and send a shot per step, or write
  "Flows not walked: F1" under Not verified.
