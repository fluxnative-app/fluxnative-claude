# Paywall checklist

Each rule has an ID to cite in findings, a check you can do in a screenshot, a check in the code, and the store rule behind it. Store text is paraphrased from the App Store Review Guidelines and Google Play's Subscriptions policy (Play Console Help); section numbers were checked against both on 2026-11-04. They change: name the section and say "check the current text". Not legal advice.

Report each item as `pass`, `fail` or `unverified`. A purchase, restore, trial or renewal seen only in a web render or in the code is `unverified`.

## Short rules

A summary; the PW and States tables below rule.

### Paywall

- The billed price and period are the largest price text. Trial length, per-week or per-month breakdowns and savings are never larger, bolder or higher in contrast than the billed amount.
- 2–3 plans; 3–4 concrete benefits the app actually unlocks (house heuristic; App Store 3.1.2(c) asks that the person sees what they get for the price).
- Close ≥ 44 pt (HIG), visible in the first frame, never delayed or shrunk. Exception: a hard paywall that says before purchase that the product needs a subscription needs no close, but must still show Restore, Terms and Privacy and a way out of the app, such as sign out.
- Restore (on the paywall and in settings or the account screen), Terms of Use and Privacy Policy present and working (App Store 3.1.1 for Restore; 3.1.2(c) and 5.1.1(i) for Terms and Privacy).
- Auto-renewal stated: price, period, renews until cancelled, where to cancel (App Store 3.1.2(c); Google Play Subscriptions policy).
- Trial text only when the store says this person is eligible; say when it ends, what it then costs and how to cancel (App Store 3.1.2(a); Google Play Subscriptions policy).
- No countdown without a real deadline; no invented "was" price; no pre-selected upsell; dismiss copy is neutral ("Not now"). App Store 3.1.2(a) (tricking people into a subscription), 5.6 (unwanted purchases, manipulative practices), 2.3.1(a) (a false price); Google Play: never mislead users about a subscription.
- States: loading, plans unavailable (with retry, never fake plans), purchasing, cancelled (neutral), pending, already owned, store error (what happened and retry), success (lands on the unlocked feature).

### AI and personal data

- Before personal data is sent to a third-party AI service, the app says what goes where and asks for explicit permission (App Store 5.1.2(i)). Check the current text.

## Order on screen, top to bottom

Building with no existing paywall: read plans, prices and trial eligibility from the purchase library when one is installed. If none is installed, build the screen against a typed offering prop and report the purchase wiring as Not verified. Do not add a library the user did not ask for.

1. Close, top corner.
2. Headline saying what the person gets ("Plan every meal in two minutes"), not "Go Premium".
3. 3–4 benefits, one short line each, one icon family.
4. Plans, each with its period and billed price.
5. Trial terms, when the selected plan has one for this person.
6. One primary button naming the action ("Start free trial", "Subscribe for {price}/year").
7. Restore, as a text button.
8. Legal line: auto-renewal, where to cancel, Terms of Use and Privacy Policy links.

## Rules

| ID | Rule | Check in a screenshot | Check in the code | Store rule |
|---|---|---|---|---|
| PW1 | Close is visible in the first frame and at least 44×44 pt (HIG). Exception: a hard paywall that says before purchase that the product needs a subscription needs no close, but must still show Restore, Terms and Privacy and a way out of the app, such as sign out | First frame after the paywall opens shows the close (or, on a hard paywall, the way out); measure its hit area | No timer, animation delay or opacity fade before the close appears; hit area ≥ 44 pt including `hitSlop`; it has an accessibility label | Google Play Subscriptions policy: a missing or hard-to-see dismiss button is a violation when a subscription is not required |
| PW2 | The billed price and period are the largest price text. Trial length, per-week or per-month breakdowns and savings are never larger, bolder or higher in contrast than the billed amount | Compare type size, weight and contrast of every price, trial and savings text on the screen | The billed amount uses the largest price style; derived per-week or per-month, trial and savings text use a smaller or equal, never bolder, style | App Store 3.1.2(c), which points to the subscription terms in Schedule 2 of the Apple Developer Program License Agreement (check the current text); Google Play Subscriptions policy: an annual plan shown most prominently as a monthly cost, or a hard-to-read post-trial price, is a violation |
| PW4 | 2–3 plans (house heuristic) | Count plan options | Plans come from the store's offering, not a literal array | App Store 3.1.2(b): no accidental purchase of several variants |
| PW5 | 3–4 concrete benefits, each one the app actually unlocks (house heuristic) | Count benefit lines; each names a feature or quantity | Each benefit maps to a gated feature in the code | App Store 3.1.2(c): say what the person gets for the price |
| PW6 | Restore is on the paywall and in settings or the account screen | Find the Restore button | It calls the store's restore and checks the result; it never shows "Restored" on a call that only resolved | App Store 3.1.1: restorable purchases need a restore mechanism |
| PW7 | Terms of Use and Privacy Policy links work | Both links visible near the purchase button | Both open a real URL | App Store 3.1.2(c) (Schedule 2 terms); 5.1.1(i) for the in-app privacy policy link |
| PW8 | Auto-renewal stated before purchase: price, period, renews until cancelled, where to cancel | Read the legal line | Text built from the store's price and period | App Store 3.1.2(c); Google Play Subscriptions policy: cost, billing frequency, renewal terms and how to cancel disclosed with no extra action |
| PW9 | Trial text only for an eligible person, and it says when and at what price it converts | With an ineligible test account the trial line is gone | Trial copy depends on the store's eligibility data, not a constant | App Store 3.1.2(a) (free trials set up in App Store Connect); Google Play Subscriptions policy: how long the trial lasts, when and at what price it converts, how to cancel |
| PW10 | No countdown without a real deadline | No timer on the paywall, or the deadline exists in the offer data | No `setInterval` or timer component counting down to a made-up end | App Store 3.1.2(a): no tricking people into a subscription; 5.6: no tricking people into unwanted purchases |
| PW11 | No invented "was" price or invented "save N %" | Every struck-through price or percentage matches a real price | Old prices and savings are computed from store prices, never literals | App Store 2.3.1(a): no false price; 5.6; Google Play Subscriptions policy: never mislead users about a subscription |
| PW12 | No pre-selected upsell; dismiss copy is neutral | Dismiss text names the action only ("Not now", "Close"), never a judgement of the person ("No, I don't want to save"); no add-on ticked by default | No default `true` on an extra purchase | App Store 5.6: no manipulative practices or unwanted purchases; 3.1.2(b): no accidental purchase of several variants |
| PW13 | Legal line readable: text ≥ 4.5:1 against its ground (WCAG) | Measure contrast in light and dark | Legal text uses a body or caption role, not a hand-lightened grey | WCAG 1.4.3 |
| PW14 | Nothing required in exchange for access: no forced rating, review, permission or tracking | Walk the paywall and the gated feature | No unlock tied to a review prompt or a permission result | App Store 3.2.2(x): no forced rating or review; 3.1.2(a): no extra task before a subscriber gets what they paid for; 5.1.2(i): no required notifications, location or tracking |
| PW15 | Placement after the first value moment, or at a gate that shows what the feature does | Fresh install: the product shows something before the paywall | The paywall route is not the initial route | Not a store rule; a hard paywall is allowed when the product only works with a subscription and says so before purchase |
| PW16 | Unlock only on the store's active entitlement | Not checkable in a screenshot | Access reads the current entitlement; no local "premium" flag, no unlock on a resolved promise | App Store 3.1.1 |

## States

Render each one through the purchase service's outcomes. Rule a state `unverified` when you could not reach it.

| State | Expected |
|---|---|
| Loading | Plan cards as skeletons at their final size |
| Plans unavailable | "Plans unavailable right now" with Retry; never free or invented plans |
| Purchasing | One button in a loading state; no double taps |
| Cancelled by the person | Back to the paywall, neutral, no error message |
| Pending (approval, slow payment) | "Waiting for approval"; nothing unlocked |
| Already owned | Offer Restore |
| Store error | What happened and Retry; no codes or provider names |
| Success | Dismiss to the unlocked feature |

## AI and personal data

| ID | Rule | Check | Store rule |
|---|---|---|---|
| PW17 | Before personal data goes to a third-party AI service, the app says what is sent and to whom, and asks for consent | A consent screen or sheet appears before the first such request; declining leaves the rest of the app working | App Store 5.1.2(i): disclose sharing with third-party AI and get explicit permission first; check the current text |

## Code counts

Run from the project root. Each line ends with known false positives.

- Hard-coded prices: `rg -n "[\$€£¥]\s?\d|/(week|month|year)\b" app src -g '*.tsx' -g '*.ts' -g '!**/*.test.*'`. False positives: price formatting helpers, sample data for screenshots. Fallback: `grep -rnE '[$€£]\s?[0-9]' app src`.
- Timers on the paywall: `rg -n "setInterval|setTimeout|countdown" <paywall files>`. False positives: a debounce or a retry delay.
- Struck prices: `rg -n "line-through|wasPrice|originalPrice" app src`. False positives: a store field holding a real intro offer.
- Restore and legal links: `rg -n -i "restore|terms|privacy" <paywall files>`. Zero hits is a fail.

The counts are facts, not a score.

## Purchases on a web preview

A web render shows layout, copy, contrast and the states you can force from code. It cannot run a store purchase, restore, trial, renewal or eligibility check. Those items stay `unverified` until an App Store sandbox account (development build or TestFlight) or a Google Play test-track purchase shows them, and the report says so in those words.

No store in the render: force each state with a dev-only fixture (guarded by `__DEV__` or a debug query param) that feeds the screen a fake offering. Label the fixture prices as sample data in the report and keep the fixture out of production code. A web render covers one viewport only; rule the Android size unverified.

In the report's Not verified line: a purchase, restore, trial or renewal seen only in a web render or the code is always unverified; real purchases need an App Store sandbox account (development build or TestFlight) or a Google Play test-track purchase. Purchases running with a demo or off source are unverified. The system permission prompts and their denied paths also need a device or simulator.

## FluxNative apps

When the project is a FluxNative app (see SKILL.md): Purchases go through `src/services/iap`, the module the purchases service record adds (`search_services` names it), never from a screen. A new app may not have it yet. Read its `PURCHASES_SOURCE` and treat `"demo"` or `"off"` as unverified: no purchase is real.
