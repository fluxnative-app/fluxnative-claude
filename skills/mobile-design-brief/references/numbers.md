# Numbers

In a FluxNative app, keep one look on every platform: read references/fluxnative-apps.md before using the platform-specific numbers.

Starting defaults; the project's tokens win. Each number is labelled with its source: HIG (Apple Human Interface Guidelines), M3 (Material Design 3), WCAG (WCAG 2.2), or house heuristic (a value that worked in our own reviews; change it when the product needs to). Guideline values are restated, not quoted, and the guidelines change: check the current text before citing one.

## Type

| Item | Value | Source |
|---|---|---|
| iOS text sizes (pt) | large title 34, title 28 / 22 / 20, headline 17 semibold, body 17, callout 16, subheadline 15, footnote 13, caption 12 / 11 | HIG |
| Android text sizes (sp) | display 57 / 45 / 36, headline 32 / 28 / 24, title 22 / 16 / 14, body 16 / 14 / 12, label 14 / 12 / 11 | M3 |
| Sizes per app | at most 6 | house heuristic |
| Weights per app | at most 3 | house heuristic |
| Faces per app | at most 2 | house heuristic |
| Display vs body | display at least 1.6× body | house heuristic |
| Body floor | 14 or larger; nothing below 11 | house heuristic |
| Line height | body 1.3–1.5× size; display 1.1–1.25×; at least 1.2× for scripts with stacked marks (for example Vietnamese) | house heuristic |
| Hero figure | about 36 pt, one line; unit about half size and raised | house heuristic |
| Text scaling | text must reflow at 200% | WCAG 1.4.4 |
| Line length | 60–75 characters for reading text | house heuristic |

One shared scale for both platforms: body 16, display at least 26. FluxNative apps use the theme's sizes.

## Spacing and size

| Item | Value | Source |
|---|---|---|
| Grid | 4 pt base | M3 |
| Steps | 4 / 8 / 12 / 16 / 24 / 32 | house heuristic |
| Screen margin | 16 | house heuristic |
| Between blocks / sections | 16 / 32 | house heuristic |
| Touch target | 44 × 44 pt | HIG |
| Touch target | 48 × 48 dp | M3 |
| Touch target floor | 24 × 24 CSS px | WCAG 2.5.8 |
| Content rows | 64–76 pt (60 only when one line) | house heuristic |
| Settings rows | 48–56 pt | house heuristic |
| Empty band under the status bar | under 56 pt | house heuristic |

## Radius and shadow

| Item | Value | Source |
|---|---|---|
| Shape scale | 4 / 8 / 12 / 16 / 28 | M3 |
| Steps per app | 3–4 from one scale | house heuristic |
| Nested radius | outer radius minus the padding between them | house heuristic |
| Shadow opacity | under 13%, neutral colour, never with a hard border | house heuristic |
| Border width | 1 pt hairline; 1.5 pt or more reads drawn | house heuristic |

## Colour and contrast

| Item | Value | Source |
|---|---|---|
| Body text | at least 4.5:1 | WCAG 1.4.3 |
| Large text (24 px, or 18.66 px bold) | at least 3:1 | WCAG 1.4.3 |
| Control boundaries, focus, meaningful icons | at least 3:1 | WCAG 1.4.11 |
| Strong colour on a working screen | 2–7% of the area | house heuristic |
| Strong colour on a home hero | about 20% | house heuristic |
| Over 40% strong colour | only onboarding, paywall or a photo detail | house heuristic |
| Accent uses per viewport | 1–3 | house heuristic |
| Filled primary buttons per viewport | at most 1 | house heuristic |
| Gradients | 0, or 1–2 per screen when the brand is built on one | house heuristic |
| Ink | never #000; dark ground never pure black | house heuristic |
| Pressed overlay / disabled content / disabled container | 10% / 38% / 12% opacity | M3 |

A common failure: `#999` on white is about 2.8:1 and fails body text.

## Icons and images

| Item | Value | Source |
|---|---|---|
| Icon size | 24 standard | M3 |
| Icon inline with body | 20 | house heuristic |
| Icon in a tile | 20 glyph in a 36–40 tile | house heuristic |
| Empty-state icon | 40 | house heuristic |
| List thumbnail ratio | 1:1 or 3:2 | house heuristic |
| Hero on a list screen | 3:2 (a 4:5 full-width picture fills the whole first screen) | house heuristic |
| Product photo | 1:1 or 3:4 | house heuristic |
| Photo resolution | at least 2× its display size | house heuristic |

## Charts

| Item | Value | Source |
|---|---|---|
| Points before a series is worth drawing | at least 5 real points | house heuristic |
| Bar width | about 0.6 of its slot, at most 32 pt | house heuristic |
| Line stroke | 2 pt | house heuristic |

## Screens to check

| Item | Value | Source |
|---|---|---|
| Phone, iOS class | 393 × 852 pt; safe area about 59 top, 34 bottom | house heuristic (test size) |
| Phone, Android class | 412 × 915 dp; about 24 top, 48 bottom with 3-button navigation | house heuristic (test size) |
| Screenshot scale for review | 2× | house heuristic |
| Readable column on tablets | at most about 672 pt | house heuristic |
