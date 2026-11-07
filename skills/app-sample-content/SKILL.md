---
name: app-sample-content
description: Writes the sample data and copy a React Native or Expo app shows before people add their own. Names that fit the app's market, uneven figures whose totals add up on every screen, dates relative to today, product words instead of slogans, and the data and copy for the states a mockup leaves out (zero items, one item, loading, error, a saved record reopened). Load it before writing or rewriting any data or copy an app shows. Use when seeding demo or mock data, replacing placeholders such as John Doe, Lorem ipsum, Item 1 or $99.99, fixing round or evenly stepped figures, writing empty, error or not-found copy, preparing content for store screenshots, or when totals, counts or names disagree between screens. Not for layout or visual changes, paywall, onboarding and permission screens and their copy (paywall-onboarding, if installed), backend or production seed scripts, database migrations, marketing sites or translation.
license: MIT
metadata:
  version: "1.0.0"
  source: fluxnative-app/fluxnative-skills
---

# App sample content

> sniff where sample data lives → count placeholders, default names and slogan words (facts) → write one record source with its equations → decide each screen's states → replace → re-check equations and repeats → look at the screens → report, ending with a **Not verified** line

Sample content is every name, figure, date, photo and line of copy a person sees before they add their own: seed and mock modules, fixtures, demo mode, store screenshots. Change the content only. Keep layout, features, navigation and data flow as they are. Data that comes from a real backend is out of scope.

## Sniff

- Where sample data lives: a fixtures, seed, mock or demo module, or arrays inline in screens. Run from the project root (`rg` skips `node_modules` and ignored build output):
  `rg -n -l -i "sample|mock|seed|fixture|demo" -g '*.ts' -g '*.tsx' -g '*.json' -g '!**/theme/**'`
  The folders this lists, plus the folders that hold screens (`app`, `src`, `components`, `lib`, `data`, `constants` or others), are the scope for Step 1.
- The market: the app's language and currency settings, the brief (`DESIGN.md`), the user's request. If none names a market, ask once; if there is no answer, pick one and write it at the top of the record source.
- Today's date: read it from the system clock, never from memory.
- The formatting the app already uses (`Intl`, a format module). Use it; never add a library for this.

## Step 1: count

Load `references/counts.md` now: the six counts (placeholder hosts, placeholder text, bare "Title", default cast, slogan words, "coming soon"), the default scope, false positives and the fallback without `rg`. Run them before you change anything, and again at the end. Write the scope at the top of the report.

The counts are facts, not a score. A hit is a reason to look at that line, and you decide. A real customer named Maya stays Maya.

## Step 2: one source of records

- No sample module yet: create one in the app's existing pattern (for example `src/data/sample.ts`).
- Every figure on every screen derives from the same records. Two screens never hold their own copy of the same fact.
- Store only base facts; derive totals, counts and badges in one selector used by every screen. A stored figure gets its equation in a comment, checked once by a throwaway script or test run, never an assertion in app code.

```ts
// Order 1042: 2 × 4.80 + 3.20 = 12.80; tax 8% = 1.02; total = 13.82
// Unread badge (3) = rows with read: false (3)
```

- Badge counts equal their rows; totals equal their shown lines; the same fact has the same value and format everywhere. When a total does not add up, load `references/numbers-dates-money.md` for the commands and the rounding rule.

## Step 3: states

For each screen that shows sample data, decide zero items, one item, loading, error, not found, failed submit and saved record reopened, in data and in copy; write the decision as a comment beside that screen's records. No storage layer: list reopened under Not verified; don't add one.

Load `references/states-and-screenshots.md` now: the table, where each state's records live, sample-mode labels, store screenshots, and the visual fallback when `mobile-deslop` is not installed (if it is, visual tells go to it).

## Step 4: replace

**People.** Load `references/people-and-places.md` when you write or replace names, photos, places or contact details. Names from the app's market and its users' age band, not the default cast; about a third of people with no photo; a photo only if yours or licensed, listed in `ASSETS.md`.

**Numbers and dates.** Load `references/numbers-dates-money.md` when you write series, totals, prices, dates or plurals. Uneven, lived-in values; prices a local pays; a currency on every amount; one format per screen; dates computed from today at runtime, no bookable slot in the past.

**Copy.** Load `references/copy.md` when you rewrite headlines, buttons, empty, error or AI text, or write sample notes and messages; it holds the before/after pairs for each.

- Product words, not slogans. The headline is the day's fact ("Rent due in 4 days"), not a greeting.
- Each fact once per screen; AI labelled once per section, no pipeline narration.

## Step 5: look

Load `references/verify-without-device.md` before you look: it gives the `Ran on:` and `Not verified:` lines. Open each screen that shows sample data, in each state you decided (Expo Web at 393×852, DPR 2, a simulator, or the user's screenshots). Read the rendered text against your equations and the repeat check. Name the evidence source first in the report; for code-only work list every screen under Not verified.

## Done when

- the placeholder, cast, slogan and "coming soon" counts return nothing, or each remaining hit is explained in the report;
- every written equation holds;
- every screen that shows sample data has every Step 3 state decided, and each state was viewed or is listed under Not verified;
- the same fact matches in value and format on every screen that shows it;
- every screen that shows sample data was viewed, or is listed under Not verified.

## Report

- the evidence source (code only, Expo Web, simulator, user's screenshots);
- the count scope, the market you wrote for, and where the record source lives;
- the count table before and after;
- the equations, each marked holds;
- the states per screen, each marked seen or not verified;
- files changed;
- **Not verified:** what you did not see rendered (for example: screens not rendered, store screenshots not captured, right-to-left layout, long names at large text sizes). Name and price plausibility for the market are your judgment, not a check: list them here when you checked no local reference.

## Never

- Real people: no public figures, no staff, no names from the user's contacts.
- Instructions as copy ("Tap a row to edit it") where a visible control shows it.
- Plurals as `count + " items"`: use `Intl.PluralRules` or the app's i18n plural rules.
- "Coming soon" rows, or notes to App Review or the owner on a working screen.

## If this is a FluxNative app

This project is a FluxNative app when the root `AGENTS.md` starts with `# Generated app source contract` and `src/platform/`, `src/theme/` and `src/features/` exist. When it matches, read `references/fluxnative-apps.md` now. For this skill in particular:

- dates, numbers and prices go through `@/i18n/format`;
- sample data lives behind `src/services/<domain>`, never inline in a screen;
- what the person adds persists through `@/services/storage`; the reopened state reads it back from there after a reload;
- the signed-in person's own name is never invented: greet without a name and show the sign-in or setup step. Sample names are for other people only (staff, sellers, reviewers, contacts of a demo business);
- keep the quiet demo line and the "the business does not receive this yet" confirmation the contract requires; they are not review notes.
