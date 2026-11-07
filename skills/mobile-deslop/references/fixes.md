# Fixes

What to change for each tell group, and what to put where an ornament was removed. Fix the shared cause first: a theme value, a row component or a card wrapper used on many screens.

Before each fix, check that it keeps the screen's features, data and routes. A fix that removes a fact the request names is wrong, even if it looks cleaner.

## Where the inputs come from

- **Brand colour:** take it from the brief, the app icon or splash, or the most used non-neutral hue in the theme. If none exists, ask the user once; do not invent one.
- **Market:** take it from the app's locale list (`i18n/`, `app.json` `locales`) or the store listing; if unknown, ask.
- **The day's fact:** it must come from data the app already has. If there is none, use the screen's own zero state (T9).

## Hard rules

Rules the result must meet: check them on every new render (Step 6), and every fix must leave them true. Each can be checked in a screenshot or the code. Contrast ratios (4.5:1, 3:1) come from WCAG. All other numbers are house heuristics: a reason to look, not a pass or fail.

- One action accent, used 1–3 times per viewport; status hues and labelled category hues are separate.
- At most 1 filled primary button per viewport.
- Gradients only when the brand is built on one: at most 1–2 per screen, never behind text or lists.
- Radii from one scale. A nested radius equals the outer radius minus the padding.
- No emoji in tabs, buttons, rows or empty states. One icon family, one stroke width.
- Rows sit on the ground. One way to separate: space between groups, a hairline between rows inside a group; never also a card surface or a shadow. A card only for one self-contained object.
- Sentence-case labels, except acronyms and codes (PIN, OTP, booking refs).
- At most 1 muted meta line per row. Other facts go in trailing slots or the detail screen, and every fact the request names is kept.
- Body text ≥ 4.5:1 and control boundaries ≥ 3:1, in both schemes (WCAG). Compute it from the two token hex values in the theme (WCAG formula), or sample two pixels of a 2x shot; report the pair and the ratio.
- Every enabled control does what it says, or is removed.
- Loading is a skeleton at the final layout. A spinner only inside a control or in pull-to-refresh.

## Colour (C)

- **Gradients (C1):** replace the gradient fill with the neutral surface. If the brand is built on a gradient, keep it on 1–2 elements (the primary action, a progress ring) and take it off backgrounds, lists and anything behind text.
- **Palette (C2, C11):** set one accent from the brand in the theme. Ground near-white (light) and near-black but not `#000` (dark). Ink near-black, never `#000`, on light.
- **Slabs (C3):** turn the saturated block into a raised neutral surface. Move the brand colour to the figure, a meter or the one action on it.
- **Glows (C4):** one neutral shadow under 13% opacity, or a hairline. Never both a shadow and a hard border.
- **Many hues (C5, C6):** keep the accent for action. Turn per-item colours into neutrals, or into category colours that are labelled and the same on every screen. Status colours only for status.
- **Contrast (C7, C10):** darken the muted-ink token until body text is ≥ 4.5:1; input and control outlines ≥ 3:1. For a pastel fill, put dark ink on it, or darken the fill.
- **Colour-only states (C8):** add a check mark, weight change, outline, sign (+/−) or word.
- **Dark mode (C9):** define each role's dark value instead of inverting: surfaces a step lighter per level, off-white ink, the accent lifted in lightness, large fills in a muted container tone, pale images dimmed or framed.

## Type (T)

- **Hierarchy (T1, T3, T5):** one display moment per screen. Lead figure at display size with its unit about half size and raised. List titles and values regular weight; semibold only for titles; bold only on the lead figure.
- **Too many styles (T2, T6):** map every literal size and weight to a role (display, title, body, label, caption) in the theme, then replace the literals. At most 2 faces; display size at least 1.6× the label under it.
- **Capitals (T4):** sentence case, muted label role, or delete the label when the figure says it.
- **Numbers (T7):** tabular figures on prices, timers, counters and aligned columns.
- **Clipping (T8):** remove fixed heights on text; use `minHeight` and padding; allow wrapping; re-enable font scaling on content.
- **Setup sentence (T9):** keep the used screen's layout at zero (the balance reads 0, the list is empty with one line), and put "Add" where the used screen keeps its action.

## Layout (L)

- **Cards (L1):** take single rows out of cards and put them on the ground. One way to separate: space between groups, a hairline between rows inside a group; never also a card surface or a shadow. Keep a card only for one self-contained object (a product, an account, a booking).
- **Tiles (L2):** pick the tile that answers the screen's job, make it the hero, and turn the rest into a list or half-width tiles under it.
- **Header (L3):** one line or none above the content; the headline is the day's fact.
- **Stamped hero (L4, L5):** give each screen the lead its job needs: a figure on a summary, a photo on a place, a list on an inbox.
- **Flat (L6):** see "Thin screens" below. Refine; do not decorate.
- **PowerPoint shapes (L7):** delete shapes that carry nothing; outlines to a hairline; a chart only with real data.
- **Spacing (L8):** one screen gutter; tight spacing inside a group, looser between sections; full-bleed only for media.
- **Radii (L9):** pick a 3–4 step scale in the theme (small, control, card, sheet); map every literal to it; nested radius = outer − padding.
- **Sparse and fold (L10, L11):** raise the lead's scale or bring the next section up so the first screen ends on a block boundary above the bars.
- **Rows (L12):** one row component: title, at most one meta line, at most one trailing slot; content rows 64–76 pt (60 only when one line); leading tile only where it tells rows apart.
- **Web chrome (L13):** move Cancel and Save into the modal header (Cancel leading, Save trailing); keep one back control; an amount gets a real amount field.

## Data and words (D, W)

- Derive every figure from one set of records and check each total's equation (D3, D4).
- Replace placeholders and the default cast with names from the app's market (D1, D8).
- One muted meta line per row; move other facts to a trailing slot or the detail screen, keeping every fact the request names (D7).
- One quiet sample label on the hero instead of a tag per row; delete "coming soon" (D9).
- The result in the app's words; label AI once (D10).
- Product words instead of slogans; the day's fact instead of a greeting; no instructions as copy (W1–W3).
- Empty and error copy: what happened, what to do, one action (W4).

## Imagery (I)

- Emoji and icon soup (I1, I2): one icon family with one stroke width; icons only where they speed recognition; one glyph per meaning.
- Discs (I3): a drawn mark of the item, or no leading icon.
- Photos (I4–I7): real or licensed photos at ≥ 2× their display size, each used once; candid style for personal content; a lower-third scrim under text on a photo.
- AI marker (I8): the brand's mark or a plain word.

## Navigation and states (N, S)

- Dead controls (N4): wire it to the real action, or remove it. Never a "Success" alert with no write.
- Competing actions (N5): one filled primary per viewport; the rest neutral or text.
- Insets (N6): pad scroll content by the bar's measured height and the safe-area insets.
- Landings (N8): after an action, land on the record it changed.
- Loading (S1): a skeleton with the loaded layout's geometry.
- Empty and error (S2–S4): the situation, one real action, one signal at its location; keep input after a failed submit.

## Motion (M)

- Delete decorative loops (M1); keep springs with overshoot for rewards only (M2).
- 100–400 ms, never blocking input (M3); press feedback on press-in (M4).
- Haptics only on success, selection and real impacts (M5).
- Transform and opacity only; under Reduce Motion, a cross-fade or nothing (M6).

## Thin screens

Removing ornament leaves space. Give it to real content:

- **The day's fact as the headline.** "$42 left until Friday", "3 orders to pack", "Next class 18:30". Not a greeting.
- **A larger lead figure** with a smaller raised unit, and the context in one muted line under it.
- **A drawn series of ≥ 5 real points** from the app's own data: spending by day, sessions by week. No chart when the data has fewer points.
- **A candid photo** where the thing has a real-world look: a place, a dish, a product, a person. Not on ledger rows.

Never put a new decoration in the freed space: no new gradient, blob, illustration, glow or badge.
