# Brief template

Copy the template into `DESIGN.md` (or `.fluxnative/memory/design-dna.md` in a FluxNative app) and fill every line. If a line does not apply, write "n/a" and why. Exception: when SKILL.md's Scope limits the request, fill only the sections it names. Keep the filled brief under about 75 lines; add rows only for screens in scope.

## Template

```markdown
# Design brief: <app name>
Updated: <YYYY-MM-DD>

## Product
- One line: <what it does, in the product's own words>
- Person: <who> opens it <where, when> to <get what done>
- Category: <row from category-defaults.md>

## Screens
| Screen | Job | Claim | Proof on screen | Lead |
|---|---|---|---|---|
| <route> | <what gets done> | <one true statement> | <what shows it> | <element seen first> |

## Style direction
<3-5 sentences: mood; colour logic (what the ground, ink and accent each do); how the type feels; component style (flat, bordered or raised; radius); at most one signature element, on <screen> where a generic hero would go, because <why it belongs to this product>.>

## Colour (light / dark)
- ground #...... / #......   surface #...... / #......  (dark: each level a step lighter)
- ink #...... / #......  (never #000)   muted ink #...... / #......  (>= 4.5:1)
- accent #...... / #......  means: <one meaning>
- error / warning / success: #...... / #......  status only

## Type, shape, space
- Faces (max 2): <face> ; roles display / title / body / label / caption = <size/weight each> (max 6 sizes, 3 weights; display >= 1.6x body)
- Tabular figures on: <which numbers>
- Radius scale: <3-4 steps> ; nested radius = outer radius - padding
- Spacing 4 / 8 / 12 / 16 / 24 / 32 ; margin <n> ; separation: space between groups, a hairline between rows inside a group; never also a card surface or a shadow
- Icons: <one family>, <one stroke>

## Routes
| Route | tab / push / modal / sheet | Lands after its main action | One-way door |
|---|---|---|---|

- Tab bar and headers set in: <layout file(s)> ; inferred routes: <route, from which cue>

## States (data screens)
| Screen | Zero | One | Loading | Error | Reopen saved |
|---|---|---|---|---|---|

## Motion
- <element>: <purpose word>, <duration or "platform default">

## Sample world
- Market <city/country> ; currency <code> ; dates <format> ; time <12h/24h>

## Moved from the category default
- <default> -> <this app>, because <reason>

## Notes
- <YYYY-MM-DD> <change and reason>
```

## What each section holds

- **Product:** one line. **Person:** who opens it, where, and what they want in that moment.
- **Screens:** per screen in scope, four fields:
  - job: what the person gets done here;
  - claim: the one thing the screen says is true;
  - proof: what on screen shows the claim;
  - lead: the element seen first. Leads differ between screens.
  A screen the request does not need has no claim and is not built; never remove a screen the user asked for. Utility screens (settings, sign-in, forms) may state their claim as their job.
- **Style direction:** one paragraph of 3–5 sentences: the mood, the colour logic, how the type feels, the component style, and at most one signature element (with why it belongs to this product; it sits where a generic hero would go). Personality goes in colour, type and imagery; layout and navigation stay standard.
- **Colour:**
  - a neutral ground; ink that is never `#000`;
  - one accent hex and its one meaning (for example "the next step");
  - status colours (error, warning, success) used only for status;
  - a dark pair for every role: each raised level a step lighter, off-white ink, accents lifted so they keep contrast.
  - Compute each text/ground pair's contrast ratio (WCAG relative luminance) with a short node or python snippet, and record the ratios in the brief next to the hexes.
- **Type:** at most 2 faces, at most 6 sizes, at most 3 weights; display at least 1.6× body. Roles: display, title, body, label, caption.
- **Shape and space:** a radius scale of 3–4 steps; spacing steps 4/8/12/16/24/32; one icon family with one stroke. One way to separate: space between groups, a hairline between rows inside a group; never also a card surface or a shadow.
- **Route map:** every route marked tab, push, modal or sheet, plus its one-way doors, and the layout file that draws the tab bar and headers (`navigation.md`).
- **States:** per data screen, what it shows at zero items, one item, loading, error, and when a saved record is reopened (`states.md`).
- **Sample world:** market, currency and date format. Hand off to `app-sample-content`. Fallback if it is not installed: write 6 names from the market and 3 uneven records, with each total written as an equation next to the data.

Numbers to start from are in `numbers.md`. The project's tokens win over them.

## No theme file

No theme file: create `src/theme/index.ts` exporting colours (light and dark), type roles, radius and spacing, and have screens import it. A Tamagui or NativeWind config found by Sniff is the theme: put the tokens there instead, never in a second system.

## Starting from existing screens or mockups

Existing screens. Derive the brief from the code first: run the Pre-finish counts (`build-and-prefinish.md`), take the most-used colour, radius and font sizes as the draft tokens, and write the brief from what is there, noting each place it will change. Then move the literals into the theme (see "No theme file" below if there is none).

Screens or mockups as input. Read the route map from them before writing it: a shared bottom bar, a back arrow, a Cancel/Save header, a grabber or a menu icon each say what kind of route a screen is (`navigation.md`, "Reading routes from screens"). Mark each such route "inferred" in the brief.

## What "differs from the category default" means

Read the category's row in `category-defaults.md`. The brief differs when at least one of these holds, and "Moved from the category default" names it:

- the accent hue and ground are not the category's default pair;
- the main screen's lead is not the default lead (for example, the next pick-up time where the default is a promo photo);
- the main screen's structure is not the default stack of equal tiles or cards.

Shifting a hex by a few steps, or renaming a section, is not a difference.

## Writing the style direction

The paragraph is the brief's one piece of prose. Each sentence must name something a screenshot can show.

- Mood: two or three words about how it should feel to use (calm, quick, exact), then the setting it is used in.
- Colour logic: what each role does, not a list of hexes ("the ground stays quiet; the accent appears only on the next step").
- Type feel: which face, and where the large size goes.
- Component style: flat on the ground, bordered, or raised; how round; where hairlines go.
- Signature element: at most one, the detail a person would describe to a friend. Leave it out rather than invent one.

Personality lives in colour, type and imagery. Do not express it through an unusual layout, hidden navigation or custom gestures: those cost the person learning time on every screen.

A sentence that no screenshot could confirm ("modern and clean", "delightful") is cut.

## Filled example (invented app)

```markdown
# Design brief: Pell Street Coffee (invented)
Updated: 2026-11-04

## Product
- One line: order ahead from one neighbourhood café and pick up without queueing.
- Person: a regular, 5-10 minutes away on the walk to work, orders their usual and wants to know when it is ready.
- Category: food ordering

## Screens
| Screen | Job | Claim | Proof on screen | Lead |
|---|---|---|---|---|
| Menu | order fast | your usual is one tap away | the usual as the first row, with price and ready time | pick-up slot strip |
| Drink | set size and milk | the price shown is the price paid | price updates with each option | drink name and price |
| Basket (modal) | pay | nothing hidden | lines, then the total | total |
| Order status | know when to walk in | ready at 8:14 | ready time largest, queue position under it | ready time |
| Orders | repeat a past order | past orders are here | dated rows with totals | latest order |

## Style direction
Quick and exact, read one-handed on a walk to work. The ground is a quiet off-white, ink near-black, and green appears only on the next step, so the eye goes to order, pay or picked up. The system face throughout; the largest size is kept for the ready time. Rows sit flat on the ground, split by hairlines; cards only for the slot strip, radius 12. Signature element: the pick-up slot strip (next five 3-minute slots with the orders ahead in each) on Menu, where a promo banner would go, because the product is about timing.

## Colour (light / dark)
- ground #F6F6F4 / #151617   surface #FFFFFF / #1E2022
- ink #1C1D1F / #ECECEA   muted ink #5E6166 / #A3A6AA
- accent #2F6B4F / #5FA884  means: the next step (order, pay, picked up)
- error #B3261E / #F2B8B5  payment and slot errors only ; warning #8A5A00 / #F0C46B  slot nearly full ; success #2E7D32 / #81C784  order ready
- contrast on ground (light / dark): ink 15.6 / 15.3 ; muted 5.7 / 7.4 ; accent 5.8 / 6.4 ; error 6.0 / 10.6 ; warning 5.5 / 11.1 ; success 4.7 / 9.0 ; label on accent 6.3 / 6.4

## Type, shape, space
- System face only; display 30/700 (ready time), title 20/600, body 17/400, label 15/600, caption 13/400
- Tabular figures on prices, totals, times, queue counts
- Radius 8 controls, 12 slots and cards, 20 sheets
- Spacing 4-32 ; margin 16 ; separation: space between groups, a hairline between rows inside a group
- Icons: one outline family, 1.5 stroke

## Routes
| Route | Presentation | Lands after its main action | One-way door |
|---|---|---|---|
| Menu | tab | Drink | no |
| Drink | push | Menu, basket count updated | no |
| Basket | modal | Order status | no |
| Order status | replaces Basket on payment | stays until picked up | yes: back goes to Menu, never Basket |
| Orders | tab | order detail (push) | no |
| Store picker | sheet | Menu with the new store | no |

- Tab bar and headers set in: app/(tabs)/_layout.tsx, app/_layout.tsx ; inferred routes: none (written from the idea)

## States (data screens)
| Screen | Zero | One | Loading | Error | Reopen saved |
|---|---|---|---|---|---|
| Menu | n/a: the store always has a menu | n/a | skeleton rows at row height | "Couldn't load the menu." + Retry in the list | n/a |
| Orders | "No orders yet. Your first one shows here once you pay." + Order | one dated row, no "see all" | skeleton rows | inline note + Retry | n/a |
| Order status | n/a | n/a | ready time placeholder at its size | "Can't reach the café. Your order is still placed." + Retry | the same order, same ready time, after the app is closed and opened |
| Drink | n/a | n/a | n/a | price error inline | editing a basket line opens with its size and milk set |

## Motion
- Slot selection: feedback, 120 ms. "Ready": reward, one check animation, once.

## Sample world
- Melbourne ; AUD ; DD/MM ; 12h. Flat white 5.20, long black 4.60, oat +0.80.
- Basket: 2 x 5.20 + 4.60 + 0.80 = 15.80 (equation kept next to the data)

## Moved from the category default
- Food photo hero, promo carousel, orange accent, category tiles -> slot strip lead, list menu with the usual first, green accent for the next step, because the regular already knows the menu.

## Notes
- 2026-11-04 First version.
```
