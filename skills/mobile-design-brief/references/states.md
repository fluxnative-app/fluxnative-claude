# States

A mockup or a first build shows one moment: the account full of good data. Every screen that shows data also has the states below, designed in light and dark. Decide them in the brief before building, then check each one by forcing it.

## Decide per screen

For each data screen, fill one row of the brief's States table. Five columns are required; write "n/a" with a reason when one cannot happen (a menu that always has items has no zero).

| Column | Decide | Force it by |
|---|---|---|
| Zero | what the screen shows with no records (see Empty below) | an empty account, or a seed with no rows for this screen |
| One | how the layout holds a single record: a list of one row, a grid with one tile, a chart with one point | a seed with exactly one record |
| Loading | the skeleton, at the loaded layout's geometry | a slow network, or a delay added in the service call |
| Error | the inline message and retry where the data would be | airplane mode, or a service call that throws |
| Reopen saved | opening a record that was saved earlier: the detail or form shows the saved values, not blanks or defaults; a draft says it is a draft | save a record, close the app (reload the page on web), open the same record |

Layouts built for many items often break at one: a chart with one point draws no line (show the value as a number until there are 5 points, see numbers.md), a grid leaves one tile stranded at half width, a "See all" link leads to the same single row. Decide what one looks like rather than letting the many-item layout stretch.

The table is a done item: the screen is not done until its row is filled and each state has been forced and seen once, or is listed under Not verified with the reason.

## The states

| State | What the screen shows | Check |
|---|---|---|
| Live | the real data | the normal render |
| Loading | a skeleton at the loaded layout's geometry: same rows, same heights, same image ratios | the skeleton and the loaded screen line up when swapped; no full-screen spinner |
| Empty (zero) | the screen's own structure at zero (headers, a zero total, the add control where the used screen keeps it), one sentence about the situation, one real action | not a lone icon, a line and a button in the middle of the page |
| Error | what happened, what to do, and a retry, inline where the data would be | no codes, no provider or library names, no "Something went wrong" alone |
| One | the single record in the many-item layout's place, without stretching it; no "See all" for one item | nothing looks missing or stranded |
| Reopen saved | the saved values in place, the record's last-saved time if edits are possible | no field falls back to a default or a blank |
| Partial | the data that loaded, with an inline note and retry on the part that failed | the whole screen is not replaced by an error |
| Offline | the last data with a quiet "Offline" note and its time; actions that need the network say so when tapped | the screen is not blank |
| Not found | a plain line ("This order no longer exists") and a way out (back to the list) | no "Try again" on something that will never load |

For screens with AI output, two more:

| State | What the screen shows |
|---|---|
| Streaming | text appears as it arrives; a stop control; the input stays usable or clearly paused |
| Quota or limit reached | what the limit is, when it resets, and the one action that lifts it |

## Rules

- A spinner only inside a control (a button that is sending). Loading a screen is a skeleton.
- One failure gives one signal, at its location. Not a banner plus a toast plus an inline message.
- A failed submit keeps everything the person typed and marks the field at fault.
- A resting button stays at full strength; a disabled look only when the action truly cannot run, with the reason visible.
- Empty-state copy says the situation and the next step in one sentence: "No orders yet. Your first one shows here once you pay." plus the action.
- Error copy: "Couldn't load your orders. Check your connection and try again." plus Retry.

## Toasts

- 4 s when the toast has an action (Undo), 2.4 s without (house heuristic).
- One action at most.
- Held on screen while a screen reader is on.
- Never covers the figures or row the action changed; place it above the bottom bar.
- Says only what the screen does not already show.

## Light and dark

- Each state is checked in both schemes.
- Skeleton blocks keep at least a visible step against the ground in dark mode.
- Error and warning colours have their own dark values; they are not the light hex on a dark ground.
