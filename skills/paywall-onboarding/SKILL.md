---
name: paywall-onboarding
description: Covers paywall, onboarding and permission screens and their copy in React Native and Expo apps, built, fixed, reviewed or checked against App Store and Google Play rules. Load it before answering or acting on any question about these screens, including when to ask a permission. Billed price shown largest, close visible from the first frame, Restore, Terms and Privacy present, no fake countdowns, "was" prices or pre-selected upsells, at most three onboarding screens that show the product, and each permission asked at the moment of use after one priming line. Use when building or reviewing a paywall, subscription or free-trial screen, onboarding, a sign-in wall that blocks the app or permission prompts, or when asking whether Apple or Google will reject one. Not legal advice. Not for other screens' design, whole-app store readiness or visual critique (mobile-screen-review, if installed), ordinary login forms, legal documents, web checkout, or purchase SDK, webhook and store-product setup.
license: MIT
metadata:
  version: "1.0.0"
  source: fluxnative-app/fluxnative-skills
---

# Paywall, onboarding and permissions

Scope: the first-run screens, every permission request and every paywall or upgrade screen. Keep what the app sells, its prices and its purchase code unchanged unless a rule fails. Store rules are paraphrased in the references, with their check date; cite the section and say "check the current text". Not legal advice.

## Pick the mode

- **Question only** (when to ask for a permission, what Apple wants): answer from the Timing table in `references/onboarding-and-permissions.md` or the short rules in either reference, cite the rule ID, say "check the current text", then stop. No render, no edits.
- **Review only** (the user says review, check, or asks whether it will be rejected): do Sniff and Steps 1–3, report, make no edits.
- **Build or fix**: Sniff and Steps 1–4.
- **Build with no existing paywall**: load `references/paywall-checklist.md` (Order on screen) before building: plans come from the purchase library or a typed offering prop; never add a library unasked.

## Sniff

- `react-native` or `expo` in `package.json`; the purchase library, if any (read its docs for setup, not this skill).
- Where the paywall, onboarding and permission requests live: `rg -n -i "paywall|onboarding|requestPermission|requestPermissionsAsync|purchase" app src -g '*.tsx' -g '*.ts'`.
- The first value moment: the first thing a new person saves, sees or completes. Onboarding and the paywall are judged against it.
- Permission declarations: purpose strings in `app.json` / `app.config.*` (`ios.infoPlist`) and `android.permissions`.
- Name your evidence before judging: a sandbox or test-track purchase on a device, a simulator or emulator, a web render, screenshots the user gave you, or the code only. Write it at the top of the report.

## Rules

Load before Step 1; cite rule IDs:

- First-run screens, sign-in walls, permission requests → `references/onboarding-and-permissions.md` (short rules, OB1–OB9, common patterns and fixes, PM1–PM10, timing, checks).
- Paywall or upgrade screens, personal data sent to AI → `references/paywall-checklist.md` (short rules, order on screen, PW1–PW17, states, code counts, web preview).

## Step 1: checklist count

Fill this from the code. Price, timer, struck-price and Restore count commands: `references/paywall-checklist.md` (Code counts).

| Count | Expected |
|---|---|
| permission requests reached on app start (before any user action) | 0 |
| permission requests with no priming line before them | 0 |
| onboarding screens before the first value moment | ≤ 3 (house heuristic); a fourth needs a reason in the report |
| paywall shown before the product has shown anything | 0, unless the product only works with a subscription and says so |
| `Restore` on the paywall | ≥ 1 |
| Terms of Use and Privacy Policy links on the paywall | 1 each |
| hard-coded price strings in paywall code | 0; prices come from the store |
| plans defined as a literal array in paywall code | 0 |
| countdown or timer code on the paywall | 0 without a real deadline |
| "was", strikethrough or `textDecorationLine: 'line-through'` prices | 0 unless the store data has the old price |
| close control rendered conditionally on a timer (`setTimeout` around it) | 0 |
| close hit area in pt, including `hitSlop` | ≥ 44; n/a on a hard paywall that says so (PW1) |

The counts are facts, not a score. A count outside its expected value is a reason to look at that spot, and you decide.

## Step 2: look

Render each onboarding step, each permission moment and the paywall in light and dark at 2x, on one iPhone and one Android size, reached through their real triggers. On the paywall's first frame check: where the close is, which price is largest, whether Restore, Terms and Privacy are readable without scrolling past the purchase button.

States: `references/paywall-checklist.md` (States). No store in the render, or only a web render: load `references/paywall-checklist.md` (Purchases on a web preview) for the dev-only fixture and what stays unverified.

## Step 3: findings

Shape: `screen | rule ID | region | observation | effect | fix | evidence (seen / measured / from code)`.

- Findings carry the top 8: failing store-rule items first, then dark patterns, then layout. Store-rule items are those with an App Store or Google Play cite in the checklist; "house heuristic" and "not a store rule" items rank as layout.
- Every other failing item stays in the Report checklist as `fail`, one line each.
- The fix names a change (move the close, swap the price sizes, add the Restore button), never an adjective.
- Do not list a rule that holds.

**Not findings:** a hard paywall in a product that only works with a subscription and says so before purchase; a recommended plan marked as such when it is real; prices that differ by country.

## Step 4: fix and re-check

- Fix store-rule failures first, then dark patterns, then layout and copy.
- Take a new render of each touched screen and rule each item `pass`, `fail` or `unverified`, naming what the screen now shows.
- A write that succeeded or a build that compiles is not evidence.

**Stop when** every item is pass or unverified, or 3 render rounds have run, or the user stops you.

## Report

- The evidence source.
- The checklist with pass, fail or unverified per item, and the count table before and after.
- Each permission with its trigger and purpose string.
- Paywall entry points and the states you saw, and which prices came from a sample fixture.
- Files changed.
- "Not verified: …". A purchase, restore, trial or renewal seen only in a web render or the code is always unverified. Purchases running with a demo or off source are unverified. The system permission prompts and their denied paths also need a device or simulator. Real purchases: `references/paywall-checklist.md` (Purchases on a web preview).

## Never

- Simulate a purchase as real, or unlock on anything but the store's active entitlement.
- Hard-code prices, trial lengths or "most popular" claims. Sample prices appear only in a dev-only fixture or a screenshot mock, labelled as sample data.
- Force a rating, review or other store action to unlock a feature (App Store 3.2.2(x)), or ask for any other task, such as a social post, before a subscriber gets what they paid for (App Store 3.1.2(a)).
- Call the paywall "approved": only review decides. Say which rules it meets.

## Hand-off

Visual polish, copy and sample prices, or purchase SDK setup → load `references/hand-off.md` (other skills only if installed, with fallbacks).

## If this is a FluxNative app

This project is a FluxNative app when the root `AGENTS.md` starts with `# Generated app source contract` and `src/platform/`, `src/theme/` and `src/features/` exist. When it matches, read `references/fluxnative-apps.md` before Step 1. Any other project, FluxNative-hosted or not, follows its own `AGENTS.md`.

Purchases, if in scope → `references/paywall-checklist.md` (FluxNative apps). Never edit `src/platform/**`. Use the theme tokens.
