# Tells catalog

A tell is a choice made by default, not for this product. Each row: `ID Name | looks like | spot it (shot / code) | fix`.

- IDs are stable. A retired ID is never reused, so an issue or a report can cite `L6` and mean the same thing next year.
- "shot" means a screenshot at 2x; "code" means a search you can run (commands in `code-counts.md`).
- A row is a reason to look, not a verdict. Check the Not tells list in `SKILL.md` before filing one.
- Tier in the finding comes from what the tell breaks, not from its group: a C tell that makes text unreadable is tier 1. Default tiers per ID: the table below.
- Area shares and hue angles are estimates by eye on the shot; write `estimated` in the evidence field. Do not file a tell on a percentage alone.

## Default tier per tell

| Tier | Tells |
|---|---|
| 1 | N4, N6, T8, D4, S3, W5, C12, L14; C7 when text is unreadable |
| 2 | T1, T5, T9, N5, L3, L11 |
| 3 | L1, L2, L4, L5, L6, L7, L13, N1, N2, N3, N7, N8, S1, S2, S4, S6 |
| 4 | C1, C2, C3, C5, C6, C7 (readable), C8, C9, C10, C11 |
| 5 | L9, C4, S5, T2, T3, T4 |
| 6 | L8, L10, L12, D7, D11 |
| 7 | I*, D1–D3, D5, D6, D8–D10, D12, D13, W1–W4, T6, T7, M* |

## C: colour

C12 is in the last table.

| ID Name | Looks like | Spot it | Fix |
|---|---|---|---|
| C1 Purple gradient | violet-to-pink wash on the hero, button or background | shot: two hues about 60° apart, each over a large area; code: gradient stops not taken from a brand token | remove it, or keep 1–2 where the brand is built on one (a button, a ring), never behind text or lists |
| C2 Default AI palette | cream paper with a terracotta serif; near-black with neon cyan data; a UI kit's default blue | shot: ground and accent match one of these three | neutral ground, near-black ink, one accent from the brand with one meaning |
| C3 Saturated slab | hero or card as one solid saturated block under oversized bold text | shot: one saturated field over about a quarter of the screen | neutral raised surface; show the brand in the figure, the meter or one action |
| C4 Coloured shadows or glows | tinted halos under cards or buttons; a shadow sampled from a photo | shot at 2x | one neutral shadow under 13% opacity, never with a hard border as well |
| C5 Rainbow accents | each tile or chip in a different hue for variety; 3+ non-neutral hues outside photos | shot: count hues with photos masked; code: per-item colour arrays | one accent for action; a category hue only when it is labelled and the same everywhere |
| C6 Accent flood | brand colour on the header, every card and several buttons | shot: count accent uses per viewport | accent 1–3 times per viewport |
| C7 Gray-on-gray text | pale meta text; an input outlined by a faint hairline | shot plus a contrast check; code: hand-lightened greys like `#999` | one muted-ink role at ≥ 4.5:1; input boundary ≥ 3:1 (WCAG) |
| C8 State by colour only | selected chip, current step, gain or loss differ by hue alone | grayscale the shot | add a second cue: check mark, weight, outline, sign or word |
| C9 Inverted dark mode | surfaces melt into the ground, saturated fills vibrate, pure black ground or pure white text, muddy accent, glaring pale photo, switch track gone | dark shot, control by control | each surface level a step lighter, off-white ink, lifted accent, dim or frame pale art, every control boundary ≥ 3:1 |
| C10 Pastel button with white label | white text on a light pink or mint fill | measure the pair | a tonal fill with dark ink, or a darker fill |
| C11 Kept starter colour | the starter template's grey-blue or kit blue still the brand colour | code: the theme's primary equals the starter's | the product's own brand colour |

## T: type

| ID Name | Looks like | Spot it | Fix |
|---|---|---|---|
| T1 Everything equal weight | under 4 sizes, all 14–18 pt, medium weight everywhere | squint; code: ≤ 3 text styles on the screen | one display moment; meta to a smaller muted role |
| T2 Noisy type | more than 6 sizes or 3 weights; one-off sizes per screen | code: distinct `fontSize` and `fontWeight` | type roles only: display, title, body, label, caption |
| T3 Semibold everywhere | every row title and amount at 600 | shot: row weights | regular for list titles and values; semibold for titles; bold only on the lead figure |
| T4 Tracked-capital labels | "SPENDING THIS WEEK" above every figure or section | shot: lines in capitals; code: `textTransform: 'uppercase'` | sentence-case muted label, or none: let the figure carry it |
| T5 Small hero figure | the key number under about 32 pt, its currency the same size | shot | lead figure at display size; unit about half size and raised |
| T6 One sans, one weight | one family at one weight everywhere; two near-identical sans; a display serif under 16 pt; Title Case and exclamation marks | code: font families list | at most 2 faces; display size at least 1.6× the label under it; sentence case |
| T7 Jumping numbers | prices, timers or counters change width as they update | code: no `tabular-nums` | `fontVariant: ['tabular-nums']` on changing or aligned digits |
| T8 Sliced glyphs | text shrunk to fit, cut by a fixed height, scaling disabled, stacked diacritics sliced | shot with longest strings at 200%; code: fixed `height` on text, `allowFontScaling={false}` | wrap or truncate; `minHeight` and padding; line height ≥ 1.2× for stacked diacritics |
| T9 Setup sentence as hero | a headline-size line like "Set up your budget" or "No expenses yet" where the figure goes | shot: a line ≥ 28 px with no digit and an imperative or empty phrase | the screen's own structure at zero (the figure reads 0), the action where a used screen keeps it |

## L: layout and identity

L14 is in the last table.

| ID Name | Looks like | Spot it | Fix |
|---|---|---|---|
| L1 Card soup | cards wrapping single rows, cards in cards, border plus shadow plus tint | shot: card backgrounds over about 60% of the area | rows on the ground; one way to separate: space between groups, a hairline between rows inside a group; never also a card surface or a shadow; a card only for one self-contained object |
| L2 Dashboard of identical tiles | a grid of same-size cards, each a tinted glyph, a title and a number | shot: equal tiles; code: one card mapped over a list | one hero, then a quieter list or half-width tiles |
| L3 Decorative header eats the fold | greeting, avatar, bell and banner fill over 40% of the first viewport | shot | one line or none; the headline is the day's fact |
| L4 Stamped hero | one hero composition or one signature shape repeated on several screens | shot: cover the lower halves and compare leads | each screen's lead from its own job; a signature only where it means something |
| L5 Default look | a title bar, one big number, a plain list, a full-width bottom button; or a lone icon, a line and a button | shot: only colour and words differ from a starter | the product's signature element in the hero's place, with its own data |
| L6 Flat as a spreadsheet | entry screen of text rows on one ground: almost no colour, no photo, no drawn data | shot: under about 4% colour, nothing drawn | refine: a surface step, the accent on the figure or action, a drawn real series, a photo where things have a real-world look |
| L7 Drawn in PowerPoint | clip-art from primitive shapes, one illustration per item, an empty ring or corner circle, outlines ≥ 1.5 pt, a chart of ≤ 3 or equal values | shot; code: `borderWidth` ≥ 1.5 | remove or refine, never add another shape |
| L8 Even spacing everywhere | every gap 16; left edges at 16, 20 and 24 on one screen | shot: compare left edges | tight inside groups, loose between sections; one gutter; full-bleed only for media |
| L9 Mixed radii | 8, 12, 16 and 24 together | code: distinct `borderRadius` | one 3–4 step scale; nested radius = outer − padding |
| L10 Too sparse | one small card in a large empty viewport; empty cards for a new user | shot | larger lead or the next section brought up; first open shows the screen's structure at zero |
| L11 Fold cuts | a heading alone at the fold; text fading under a bar | shot: bottom 120 pt | end the first screen on a block boundary above the bars |
| L12 Ragged rows | row heights vary; avatar, title and value off one centre line; 44 pt rows reading as a settings table; the same tile on every row | shot: cover the titles; code: rows built from stacked text | one idea per row (title, ≤ 1 meta line, ≤ 1 trailing); content rows 64–76 pt (60 only when one line) (house heuristic) |
| L13 Web chrome in an app | Cancel and Save mid-page; two back controls; an amount in a plain box; a desktop table of small text | shot | modal: Cancel leading, Save trailing in the header; pushed screen: one back; a list row that drills in |

## D: data and content

| ID Name | Looks like | Spot it | Fix |
|---|---|---|---|
| D1 Generated cast | Maya, Priya, Theo, Marcus, Leah, Ines, Hana, Jules; one person per background; every face a model headshot | read the names | names from the app's market; about a third without a photo; candid faces |
| D2 Face does not match name | a man's photo over a woman's name | shot | order the portraits to match, or a neutral glyph |
| D3 Round, shaped data | round sums, even steps, a staircase series, a date every third day, a list that exactly fills the grid | read the figures | uneven, lived-in values |
| D4 Disagreeing figures | lines that do not sum to the total; a count that does not match its rows; one figure different across screens; a price far off the market | write the equation and check it | derive every figure from the same records |
| D5 Chart from nothing | a chart of constant, equal or ≤ 3 values; a claim the chart does not show | shot | a real series of ≥ 5 points, or no chart |
| D6 Said twice | a heading repeating the title; a date chip beside the same date in words | read for duplicates | once, where it is read |
| D7 Meta stack | "·"-joined meta lines, or 3+ grey lines under a title | shot; code: "·" count | one muted line the person decides by; other facts in trailing slots or the detail screen |
| D8 Placeholder copy | Lorem, "Item 1", John Doe, "Title", "Description here", 1,234, $99.99 | code: placeholder search | real or believable content |
| D9 Sample tag per row | a "Sample" tag on every row; "coming soon"; a note to the owner or App Review on a working screen | read the page text | one quiet label on the hero; features not built are left off |
| D10 Model narration | "Suggested, high confidence", "Looked at the next 7 days", "Answered by AI from…" | read the page text | the result in the app's words; AI labelled once |
| D11 Paragraph in a card | an explanation or "a − b = c" inside a card | shot | one title, one figure, ≤ 2 short lines |
| D12 Numbers said wrong | "about 29.8 days", this year shown in a date, "9 AM" beside "9:00 AM", an amount with no currency, "1 items" | read the page text | as a person says it; one format per screen; plural rules |
| D13 Staged sample writing | sample notes built around the feature: one target word per sentence, punchlines | read three samples | ordinary, slightly clumsy writing |

## W: words

| ID Name | Looks like | Spot it | Fix |
|---|---|---|---|
| W1 Slogan voice | "Elevate your journey", Discover, Seamless, Effortless, Unlock (examples; the full word list is in `app-sample-content` `references/copy.md` when installed) | read | product words: "Track order" |
| W2 Greeting as headline | "Good morning, Sam" as the largest line | shot | the day's fact as the headline |
| W3 Instructions as copy | "Tap an item to change it" on a working screen | read | say what the person gets, or nothing |
| W4 Empty and error words | "No data", "Something went wrong", "Error 503", a provider's name | read | what happened, what to do, one action |
| W5 Paywall dark patterns | tiny or late close button, countdown with no deadline, invented "was" price, per-week price larger than the billed price | shot | billed price largest, close ≥ 44 pt from the first frame, Restore, Terms and Privacy present |

## I: imagery and icons

| ID Name | Looks like | Spot it | Fix |
|---|---|---|---|
| I1 Emoji as icons | emoji in tabs, buttons, rows or empty states | code: emoji search | one icon family |
| I2 Icon soup | an icon on every row and label; mixed stroke weights; one glyph for three meanings | shot | icons only where they speed recognition; one family, one stroke, one meaning each |
| I3 Letter, dot or number discs | initials, bare dots or "01, 02, 03" as item icons | shot; page text has single-letter lines | a drawn mark of the item, or no icon |
| I4 Icon blown up as photo | a UI icon at hero size, or a grey box where a photo belongs | shot | a real photo, or a labelled neutral placeholder at the final ratio |
| I5 Staged stock photos | glossy studio shots for the person's own content; placeholder image hosts | shot; code: placeholder host search | candid phone-style photos for personal content; owned or licensed images, licence recorded |
| I6 Soft or broken photo | upscaled, under 2× its display size, the same photo twice, a dead URL | shot at 2x | images ≥ 2× display size; each used once |
| I7 Text over photo | text on a busy photo with no scrim; text drawn twice for a shadow | shot | a lower-third scrim; one text node per line |
| I8 Sparkles for AI | a sparkle glyph as the AI marker | shot | the brand's own mark, or a plain label |

## N: navigation and chrome

| ID Name | Looks like | Spot it | Fix |
|---|---|---|---|
| N1 Wrong navigation | a hamburger for 3–5 destinations; more than 5 tabs; icon-only tabs; a centre "+" tab | shot | 3–5 tabs with icon and label; tasks in sheets or modals |
| N2 Drawn system chrome | a hand-drawn status bar, notch spacer or home indicator; a second tab bar | shot | system chrome only |
| N3 Non-native smells | header as a coloured block; X on a pushed screen; back arrow on a modal; tab label on two lines; sheet with no grabber | shot | pushed screen gets back; modal gets Cancel or Close; sheet gets a grabber and swipe to dismiss |
| N4 Dead control | `onPress={() => {}}`; a form with only an X; a "Success" alert with no write | tap each control; code: empty handler search | wire it, or remove it |
| N5 Competing CTAs | 2+ filled primary buttons in one viewport; button pairs in two orders | shot | one primary; others neutral or text |
| N6 Content under chrome | last row behind a floating bar; text under a header; an empty band over 56 pt under the status bar | scroll to the end | pad by the bar's measured height; safe-area insets |
| N7 Glass on content | a translucent fill faking glass on cards or over a plain ground | shot at 2x | glass only on the floating functional layer (bars, sheets) |
| N8 Wrong landing | Send returns to the overview; a toast covers the figures it changed | walk the flow by taps | land on the changed record; the toast says only what the screen does not |

## S: states

| ID Name | Looks like | Spot it | Fix |
|---|---|---|---|
| S1 Spinner as loading | a full-screen spinner; a skeleton unlike the loaded layout | code: `ActivityIndicator` as the only loading state | a skeleton at the final geometry; spinner only inside a control |
| S2 Lone-icon empty | an icon, a line and a button filling the page; "No data" | empty account shot | the situation in one sentence, one real action |
| S3 Unwired errors | raw HTTP codes; "Try again" on not-found; a `catch` that only logs; sample data hiding a failure | code: `catch` blocks, `?? sample` fallbacks | inline error with retry; not-found with a way out; input kept |
| S4 One failure, three signals | banner plus toast plus inline text for one error | trigger the failure | one signal, at its location |
| S5 Faded resting control | a resting button faded to mean "nothing to send" | shot | resting fill keeps ≥ 3:1; disable only when it truly cannot act |
| S6 Missing states | only live data designed, only one scheme | try an empty account and a nonsense search | live, loading, empty and error, in light and dark |

## M: motion

| ID Name | Looks like | Spot it | Fix |
|---|---|---|---|
| M1 Decorative motion | pulsing button, idle looping illustration, parallax on a list, auto-advancing carousel | code: `withRepeat`, `Animated.loop` with no stop | motion only to confirm an action or reveal data |
| M2 Bounce everywhere | spring overshoot on chrome, presses, navigation and errors | code: spring configs | overshoot only for rewards |
| M3 Slow or blocking motion | over 400 ms; input blocked during an entrance | device | 100–400 ms; input never waits on an animation |
| M4 No pressed state | a custom pressable shows no change on press | press it | feedback on press-in: a light overlay, slight scale or opacity |
| M5 Haptics noise | haptics on scroll, typing or every tap | code: haptics calls | success, selection and real impacts only |
| M6 Reduce Motion ignored | bounces under Reduce Motion; layout properties animated per frame | code: no reduce-motion check; animated `width`, `height`, `top` | transform and opacity only; decorative motion becomes a cross-fade or nothing |

## Only when working from a reference

File these only when the user gave a reference app or screen.

| ID Name | Looks like | Spot it | Fix |
|---|---|---|---|
| C12 Tracked palette | 3 of the 5 dominant colours nearly match the reference's | compare dominant colours side by side | re-derive the palette from this product's brand colour |
| L14 Brand carry-over | a copied phrase of 3+ words, the reference's mascot, wordmark or signature shape; or a request part missing because the reference lacked it | compare beside the reference | own words and assets; keep the reference's section order and density, never its content |

## The tells worth the most attention

Purple gradients and emoji icons are known to every reviewer. These eight are not, and they survive a first polish:

- **T9 Setup sentence as hero.** A new account opens on "Add your first expense" in headline size. The used screen has a figure there. Keep the figure's place and show 0; put the action where the used screen keeps its action.
- **L6 Flat as a spreadsheet.** The overcorrection after removing gradients: text rows on one ground, nothing drawn. The answer is refinement (a surface step, the accent on the figure, a real series), not a new ornament.
- **L7 Drawn in PowerPoint.** Shapes that carry nothing: an empty ring, a corner circle, clip-art made of primitives, thick outlines. Delete them; do not restyle them.
- **D7 Meta stack.** Second-wave generators replace cards with rows carrying three "·"-joined facts. Keep one line the person decides by. Move the rest, but never drop a fact the request names.
- **D9 Sample tag per row.** "Sample" on every row, or a note to App Review on a working screen. One quiet label on the hero is enough.
- **D4 Disagreeing figures.** The home total and the list total differ; three line items do not add up. Write the equation next to the data and check it.
- **N8 Wrong landing.** Every screenshot passes, but Save returns to the overview and a toast covers the figure it changed. Only walking the flow by taps finds it.
- **D1 Generated cast.** The same names appear in app after app. Check the names against the app's market; a real user's name may match, so it is a check, not a ban.
