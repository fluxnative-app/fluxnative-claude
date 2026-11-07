# Finding shape

## Fields

| Field | Content |
|---|---|
| `screen` | Route and scheme: `/orders · dark`. Add the page for long screens: `/orders · light · p2`. |
| `tier` | `1` to `7`: broken, hierarchy, structure, colour, surfaces, density, details. Same tiers as `mobile-deslop`. |
| `pattern` | A tell ID from `mobile-deslop` (`D7`, `N8`…), or from the fallback list in `review-method.md` when it is not installed, or `""` when none fits. Never invent an ID. |
| `region` | Where on the screen: "header", "first row", "bottom 120 pt", "tab bar". |
| `observation` | What the pixels show. No judgment words, no fix. |
| `effect` | What it does to the person: what they miss, misread or cannot do. |
| `fix` | One change: a token, a role, a component, a spacing step or a removal. |
| `evidence` | `seen` (in the shot), `measured` (a count or a tool measurement backs it) or `from code`. |

## Format and numbering

- `pattern` is a tell ID from `mobile-deslop` (for example `D7`). If it is not installed, use
  the fallback list in `review-method.md`, or leave the field empty.
- `evidence` is `seen`, `measured` (a count or a tool measurement backs it) or `from code`.
  `from code` only confirms an absence (`capture.md`) or explains a cause; the observation
  still comes from a shot.
- Output the JSON object below when the caller wants JSON, otherwise the markdown row. Use one
  format per report. Number findings O1, O2… in order; continue the sequence on later rounds.

## JSON

```json
{
  "ran_on": "Expo Web export in a browser tool, 393×852 at 2x, light and dark",
  "verdict": "findings",
  "findings": [
    {
      "id": "O1",
      "screen": "/orders · light",
      "tier": "1",
      "pattern": "N6",
      "region": "last row",
      "observation": "At the end of the scroll the last order sits under the floating New order button.",
      "effect": "The oldest order cannot be read or opened.",
      "fix": "Pad the list end by the button's height plus the bottom inset.",
      "evidence": "seen"
    }
  ],
  "rulings": []
}
```

`verdict` is `clean` when there are no new findings and no earlier finding is ruled `open`,
otherwise `findings`. Every field is required. The other report parts (concept paragraph,
flows walked, panel result, Blocking, Not verified) follow the JSON as markdown, in report
order.

For a ready-to-ship question: not ready while a tier 1 finding is open (name it under
Blocking); otherwise say which tiers are left.

`ran_on` names what actually ran the app: `Expo Web export`, `iOS simulator`,
`Android emulator`, `release build on <device>`, or `user screenshots, origin unknown`, plus
the size and schemes. Claim only what was exercised there: a web export says nothing about
device fonts, native sheets or haptics (`verify-without-device.md`).

## Markdown row

A markdown report starts with the same line, before the table:

```
Ran on: Expo Web export in a browser tool, 393×852 at 2x, light and dark
```

```
| id | screen | tier | pattern | region | observation | effect | fix | evidence |
|---|---|---|---|---|---|---|---|---|
| O1 | /orders · light | 1 | N6 | last row | Last order sits under the floating New order button at scroll end. | Oldest order cannot be read or opened. | Pad the list end by the button height plus the bottom inset. | seen |
```

## Good findings

1. `/home · light | 2 | | hero | The balance is 18 pt, the same size as the row titles below it. | The person scans the list before finding the balance. | Set the balance in the display role with a smaller raised currency unit. | seen`
2. `/stats · dark | 4 | C9 | cards | Card surfaces and the ground are the same tone; card edges disappear. | Groups read as one block; the person cannot tell where a week ends. | Lift card surfaces one step lighter than the ground in the dark theme. | seen`
3. `/inbox · light | 6 | D7 | rows | Each row carries three grey lines: sender · time · label, a preview, and a tag line. | The subject, which people decide by, is lost among meta lines. | Keep one muted line (the preview); move time to the trailing slot and the label to a badge. | measured` (backed by a count of "·" lines)

## Bad findings, rewritten

**Vague.**
- Bad: `/home | 3 | | everywhere | The layout feels busy. | Hard to use. | Simplify.`
- Rewritten: `/home · light | 3 | L1 | whole screen | Seven bordered cards, one per row of data, fill the first viewport. | No group stands out; the person reads every box. | Put the rows on the ground in one grouped list split by hairlines; keep one card for the active order.`

**An adjective as the fix.**
- Bad: `/plan · light | 2 | N5 | footer | Two filled buttons. | Confusing. | Make the main button pop more.`
- Rewritten: `/plan · light | 2 | N5 | footer | "Start trial" and "See plans" are both filled in the brand colour. | The person cannot tell which one moves them forward. | Keep "Start trial" filled; make "See plans" a text button.`

**Repeats a measurement.**
- Bad: `/home | 4 | | all | The count found 14 hex colours outside the theme. | Too many colours. | Use fewer colours.`
- Rewritten: a count is a fact that tells you where to look, not a finding. Look at the screen
  and write what the extra colours do: `/home · light | 4 | C5 | category tiles | Six tiles use six hues; none is labelled. | Hue suggests a meaning the app does not have. | Neutral tiles with the accent only on the selected tile; hues come from theme tokens.` Set `evidence` to `measured` because the count backs it.

## Rulings

On the next round, rule each open finding:

```json
{"id": "O1", "ruling": "fixed", "evidence": "/orders light p2: the last order ends 16 pt above the button"}
{"id": "O2", "ruling": "fixed", "evidence": "/orders light: the total is now the only 36 pt line; the promo strip is gone"}
{"id": "O3", "ruling": "open", "evidence": "/inbox light: rows still carry two grey lines"}
```

- `fixed` names what the new shot shows.
- `open` says what is still there.
- No new shot of that screen means `open`, with evidence "not re-shot" ("not re-shot at the
  same size" when only a different size was shot, below).
- Write rulings before new findings. New findings continue the id sequence.
- Rule `fixed` only when the new shot shows the change, and name what it shows.
- A successful write, a passing build or a diff is not evidence. Without a new shot the ruling
  is `open`.
- New findings get new ids. Never reuse an id.
- Rulings sit outside the cap of 8; findings from the flow walk count toward it. Each round
  that writes rulings counts as one of the 3 reviews.
- Earlier findings given without ids: number them O1… in the order given, and say so.
- Re-shoot the same route, scheme and device size as the finding; a different size means
  ruling `open` with evidence "not re-shot at the same size".
- A finding on a screen that no longer exists: `fixed`, evidence "screen removed", only if the
  request did not need it.

## Who applies the fix

Fixing the findings goes to `mobile-deslop`, if installed. If neither it nor
`app-sample-content` is installed, whoever fixes (the user or another agent) follows each finding's
`fix` field: in tier order, in the shared theme or component when the cause is shared, then a
new render and this review again for rulings. This skill does not make the change.
