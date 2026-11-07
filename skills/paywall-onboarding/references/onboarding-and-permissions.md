# Onboarding and permissions

Rules with IDs to cite in findings. Store text is paraphrased; section numbers were checked against the App Store Review Guidelines on 2026-11-04. They change, so name the section and say "check the current text".

## Short rules

### Onboarding (short)

- At most 3 screens before the first value moment (house heuristic). A fourth needs a reason, stated in the report. Priming screens count only when they appear before the first value moment; a permission asked there already fails PM1.
- Each screen shows the product or changes it (a choice the next screen uses, a required setup step). No slogan slides.
- Skip or continue is one tap away, unless the step is a required one-way setup.
- It ends inside the product, as a one-way door: back never re-enters onboarding.
- An account wall only when the product needs an account (App Store 5.1.1(v)); sign-in at the first sync, share or purchase.
- Conversion: this skill cannot measure it. Report which rules fail, and suggest tracking the share of installs that reach the first value moment. Claim no uplift.

### Permissions (short)

- Ask at the moment of need, after the person's own action ("Scan" → camera).
- One priming line or screen first, naming the benefit, with one button that opens the system prompt. It has its own title and app styling, does not reuse the system alert's text, and its button is not "Allow" or "Don't Allow".
- Never all permissions at launch.
- Denied is a working state: say what is off, offer the way without it, and link to the system settings (App Store 5.1.1(iv)). Never re-prompt in a loop.
- Nothing is locked behind granting notifications, location, tracking or another permission (App Store 5.1.2(i); 5.1.1(iv) for data access the feature does not need; 4.5.4 for notifications).

## Onboarding

First find the first value moment: the first thing a new person saves, sees or completes (a logged meal, a first result, a finished lesson). Every onboarding screen is judged by whether it gets the person there sooner or makes it fit them.

| ID | Rule | Check |
|---|---|---|
| OB1 | At most 3 screens before the first value moment (house heuristic). A fourth needs a reason, stated in the report. Priming screens count only when they appear before the first value moment; a permission asked there already fails PM1 | Count screens from a fresh install to the first value moment |
| OB2 | Each screen shows the product or changes it: a choice the next screen uses, a required setup step, or one sentence of promise over a real screen | Cover the headline: is something of the product still on screen? |
| OB3 | A question is asked only when its answer changes what the person sees next, and the answer is stored and used | Find where the answer is read in the code |
| OB4 | An interactive first step (pick interests, set a goal, import) over a slide deck | Count swipe-only slides with no input |
| OB5 | Skip or Continue is one tap away, unless the step is a required one-way setup | Find the skip control on each screen |
| OB6 | Onboarding ends inside the product and back never re-enters it | After finishing, press back or swipe back: the app stays in the product |
| OB7 | Sign-in only when the product needs an account; otherwise at the first sync, share or purchase (App Store 5.1.1(v), which also asks for in-app account deletion when the app creates accounts) | Can a new person reach the first value moment without an account? |
| OB8 | With a third-party sign-in as the main option, an equally private option is offered on iOS (App Store 4.8) | Sign-in screen lists the options |
| OB9 | Concepts that are not obvious stay reachable later ("How it works" in settings) | Find it after onboarding |

Common patterns and their fixes:

- 4–6 slogan slides → one promise screen, or an interactive first step.
- Sign-up before anything → guest use, sign-in at the first sync, share or purchase.
- Every permission on screen 2 → each at its feature's first use.
- Paywall as the first screen → after the first value moment, with a visible close.
- Questions whose answers change nothing → remove them.

## Permissions

### The priming step

Before the system prompt, one line or one screen says what the person gets ("Get a reminder when your plant needs water").

| ID | Rule | Check |
|---|---|---|
| PM1 | The request follows the person's own action that needs it | Trace the call: it runs from a press handler, not from app start or a screen's first render |
| PM2 | One priming line or screen first, naming the benefit, not the permission | Read it: it says what the person gets |
| PM3 | The priming step has one button ("Continue" or "Next") that opens the system prompt; it is not titled "Allow" (HIG) | Count buttons and read the label |
| PM4 | The priming step does not imitate the system alert | It has its own title and app styling; it does not reuse the system alert's text or button labels (Allow / Don't Allow) |
| PM5 | Never all permissions at launch | Fresh install: no system prompt before the person acts |
| PM6 | The purpose string says what, for what and the benefit ("Lets you scan receipts to add expenses", not "This app needs camera access") (App Store 5.1.1(ii)) | Read `ios.infoPlist` in the app config |
| PM7 | Every permission used is declared in the config, and none is declared that is not used | Compare the config with the calls in the code |
| PM8 | Denied is a working state: what is off, the path without it, and a link to system settings (`Linking.openSettings()` in React Native) (App Store 5.1.1(iv): offer an alternative where possible) | Deny on a simulator and look |
| PM9 | Never re-prompt in a loop after a denial | Deny, leave, come back: no second system prompt |
| PM10 | No feature, reward or paid content depends on granting notifications, location, tracking or another permission (App Store 5.1.2(i): system functions never required; 5.1.1(iv): no forced consent to data access the feature does not need; 4.5.4: notifications not required for the app to work) | Deny each and walk the app |

### Timing per permission

| Permission | Ask when | Denied path |
|---|---|---|
| Notifications | After the person makes something worth a reminder, or taps "Remind me". Android 13 and later ask at runtime | An inline note and "Turn on in Settings" |
| Camera | On "Scan" or "Take photo" | Explain, link to settings, offer importing a photo instead |
| Photos | On "Choose photo"; prefer the system photo picker, which needs no library permission | The picker |
| Location (while using) | On "Near me" or recentring a map | Manual city or address search |
| Contacts | On "Invite from contacts" | A share link |
| Microphone | On the first record or voice action | Typed input |
| Tracking (iOS) | Only if the app tracks across other companies' apps, and after the first value moment | The app works in full |

### Checking it

- In the code: `rg -n "request\w*Permission|requestPermissionsAsync|requestTrackingPermission" app src -g '*.tsx' -g '*.ts'`, then read each caller. A call inside a root layout, an app-start effect or a screen's first render is a PM1 or PM5 fail. False positives: a permission status read (`get…PermissionsAsync`) that asks nothing. Fallback: `grep -rnE 'request[A-Za-z]*Permission' app src`.
- On screen: the system prompt and its denied path need a simulator, emulator or device. In a web render or the code alone, rule PM5, PM8 and PM9 `unverified` and put them on the Not verified line.

The counts are facts, not a score.
