# Review method

Tiers with examples, how to check each craft line, the Not tells list, and a short tell list
for when `mobile-deslop` is not installed. All examples are invented.

## Order of a pass

1. Baseline: look at each shot once at full size; read the page text.
2. Squint (or blur the shot): what leads?
3. Grayscale: does any state depend on hue alone?
4. Craft lines, screen by screen.
5. Tells (the list at the end, or `mobile-deslop`'s catalog).
6. Light, dark, longest strings, large text, Android size.
7. Walk the flows (`flow-walk.md`).
8. Write at most 8 findings.

You cannot squint or grayscale an image by looking. If a shell is available, make the copies:
`magick shot.png -blur 0x12 blur.png` and `magick shot.png -colorspace Gray gray.png`, then
view them. If not, write "squint/grayscale not run" and set `evidence` to `seen` (never
`measured`) on findings that rely on them. Count hues only from the code (theme tokens) when you cannot mask photos.

## Tiers with examples

Each tier: two findings worth writing, one thing that is not a finding.

### Tier 1: broken

- `/cart · light`: the last line item sits under the floating checkout bar at the end of the
  scroll; the person cannot read or remove it. Fix: pad the list end by the bar's height.
- `/summary · dark`: the header says "3 orders" and the list shows 4 rows. Fix: derive the
  count from the rows.
- Not a finding: an empty calendar day next week. A future day with nothing on it is true.

### Tier 1, too close to a reference

- `/home`: the headline repeats five words of the reference's headline. Fix: write the
  headline from this product's own fact.
- `/home`: the request names a group of people and no person is on screen. Fix: put the
  group's members in the lead.
- Not a finding: the same three-zone order (summary, list, action) as the reference, when the
  user asked for that structure.

### Tier 2: hierarchy

- `/wallet`: balance is 20 pt, the same size as the section titles; nothing leads. Fix: make
  the balance the display role, with a smaller raised currency unit.
- `/plan`: two filled buttons ("Start", "Upgrade") in one viewport. Fix: keep "Start" filled;
  make "Upgrade" a text button.
- Not a finding: a settings screen where no element leads. A list of equal settings has no
  hero by design.

### Tier 3: structure

- `/home`, `/stats`, `/goals`: the same big-number-over-ring hero on all three. Fix: give each
  screen the lead its job needs (stats: a drawn series; goals: the next goal's progress).
- `/profile`: each settings row is a separate card with shadow. Fix: rows on the ground in one
  grouped list, split by hairlines.
- Not a finding: one card holding one self-contained object, such as a ticket or a booking.

### Tier 4: colour

- `/categories`: six tiles in six hues with no labels tying hue to meaning. Fix: neutral
  tiles; the accent only on the selected one.
- `/feed · dark`: cards and ground are the same tone; groups melt. Fix: lift card surfaces one
  step lighter than the ground.
- Not a finding: a full-bleed orange header when orange is the brand and the brief says so.

### Tier 5: surfaces

- `/checkout`: radii 8, 12, 16 and 24 on one screen. Fix: one radius scale; inner radius =
  outer radius minus padding.
- `/home`: cards with a border, a shadow and a tinted fill at once. Fix: one separation
  device: keep the fill, drop border and shadow.
- Not a finding: the system sheet's own corner radius.

### Tier 6: density

- `/inbox`: each row has three grey lines joined with "·". Fix: one muted line; date to the
  trailing slot; keep every fact the request names.
- `/today`: a 90 pt empty band between the header and the first section. Fix: close it to the
  section gap (24–32 pt).
- Not a finding: rows of 48–56 pt on a settings screen.

### Tier 7: details

- `/timer`: digits shift width as they change. Fix: tabular figures on the counter.
- `/tabs`: three outline icons and one filled icon in the tab bar. Fix: one icon family, one
  stroke.
- Not a finding: the platform's default back chevron.

## Craft lines

Walk every screen through these. Each line has a check in the table below.

- Weight marks what leads.
- One hero, with room around it.
- No filler cards.
- Figures plausible for the market.
- Content rows 64–76 pt, 60 only when one line (house heuristic).
- Few colours, each with one meaning.
- Real charts (5 or more real points), no icon scaled up as a picture, no stock-photo people
  or generic 3D blobs as the only image.
- Not a spreadsheet: the entry screen is not text rows on one ground only; it has a lead and a
  drawn series or a photo where the thing has one.
- Not PowerPoint: no slide-style title over three bullet cards, no saturated slab under
  oversized text, no clip-art of primitive shapes.

## Craft line checks

| Line | Check |
|---|---|
| Weight marks what leads | Squint. The first thing you see is the thing the screen is for. Only one element is heavy. |
| One hero, with room | One display-size element in the first viewport, with more space around it than around other blocks. |
| No filler cards | Every card holds one self-contained object. Cover the card: would anything be lost? If not, it is filler. |
| Figures plausible for the market | Write the equation for each total (lines + tax = total; count = rows) and check it. Read prices against the market from the inputs; if none was given, write "figures not checked against a market". |
| Content rows 64–76 pt | Read it from the style (paddingVertical + line height), or measure at 2x: content rows 128–152 px, 120 only for one line. Settings rows 48–56 pt. (house heuristic) |
| Few colours, each with one meaning | Count non-neutral hues outside photos: one action hue, plus status and labelled category hues. If you cannot mask photos, count hues from the theme tokens in code. |
| Real charts, no blown-up icons | Charts show 5+ points; no icon is scaled up as a picture. |
| Not a spreadsheet | By eye, in grayscale and colour: the entry screen is not text rows on one ground with almost no colour and nothing drawn (`L6`). Do not report a percentage you did not measure. |
| Not PowerPoint | No saturated slab under oversized text, no clip-art of primitive shapes, no thick outlines (1.5 pt or more, 3 px at 2x), no chart of 3 or fewer equal values. |

## Not tells

Do not flag these. When unsure, ask the user or check the brief.

- **Brand gradient or brand colour.** If the brand is built on a gradient or a saturated hue,
  using it is right. Still flag it behind body text or lists.
- **Required sample labels.** A "Sample" label the brief asks for, on the hero, once.
- **Platform defaults.** System tab bar, navigation bar, sheets, switches, pickers, alerts,
  fonts and back controls look the way the platform draws them.
- **Requested reference structure.** If the user asked to follow a reference's layout, the
  same zone order is the request, not a copy. Copied words, palette or brand still count.

## Fallback tell list

Use these when `mobile-deslop` is not installed. IDs match its catalog.

| ID | Tell | Spot it |
|---|---|---|
| C1 | Purple gradient | violet-to-pink wash on a hero, button or background, with no brand behind it |
| C5 | Rainbow accents | 3+ non-neutral hues outside photos, no labels |
| C9 | Inverted dark mode | surfaces melt, pure black, muddy accents, vanishing switch tracks |
| T1 | Everything equal weight | squint: nothing leads |
| T4 | Tracked-capital labels | all-caps label over every section |
| T9 | Setup sentence as hero | "Set up your…" or "No … yet" at headline size where the figure goes |
| L1 | Card soup | most of the screen is card backgrounds |
| L2 | Dashboard of identical tiles | a grid of same-size tiles, glyph + title + number |
| L4 | Stamped hero | the same lead on several screens |
| L6 | Flat as a spreadsheet | text rows only, almost no colour, no drawn data |
| L7 | Drawn in PowerPoint | clip-art shapes, thick outlines, empty rings |
| D1 | Generated cast | the same few model-default first names, studio headshots |
| D4 | Disagreeing figures | totals that do not add up, counts that differ across screens |
| D7 | Meta stack | several grey lines or "·"-joined facts under each title |
| D9 | Sample tag per row | "Sample" on every row, notes to the owner on a working screen |
| I1 | Emoji as icons | emoji in tabs, buttons, rows, empty states |
| I3 | Letter, dot or number discs | initials, dots or "01, 02" as item icons |
| N4 | Dead control | an enabled control that does nothing |
| N5 | Competing CTAs | 2+ filled buttons in one viewport |
| N6 | Content under chrome | last row or text hidden by a bar |
| N8 | Wrong landing | an action ends somewhere other than the record it changed |
| S1 | Spinner as loading | full-screen spinner instead of a skeleton at the final layout |
