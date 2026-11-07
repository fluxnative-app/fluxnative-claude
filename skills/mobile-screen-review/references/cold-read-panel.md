# Cold-read panel

This is a method you run yourself in fresh subagents, not a tool: no MCP tool or command runs it.

A cold read answers one question: would a person who has seen many apps think this one was
made by an AI with default choices? You cannot answer it yourself after working on the app.
Two fresh readers can.

## When

- The user asks whether the app looks AI-made, generic or templated.
- Before store screenshots or a launch.
- After a polish round, to check it moved the first impression.

## Freeze the shots

Take the shots once and give both runs the same files.

| Set | Count | What |
|---|---|---|
| Main | 6 | The screens a new user sees first and most: entry, the main list, a detail, the main action, a form, settings or profile |
| More | 4 | Other screens: an empty state, an error or offline state, a secondary flow, a second detail |
| Dark | 2 | The entry screen and one busy screen in dark mode |

At 2x, labelled by route and scheme. Use the same device size for all twelve. These shots are
not limited by the review's 4-route cap. Remove anything
that is not the app (browser chrome, simulator frame, debug overlays).

## Required, not tells

Fill this from the brief before running. Each run gets it.

```
Required by the brief, not tells:
- Brand: <colour or gradient the brand is built on, or "none">
- Labels: <sample labels the brief requires, e.g. one "Sample" label on the home hero>
- Structure: <reference structure the user asked for, or "none">
- Platform: native tab bar, navigation bar, sheets and controls are the platform's own
```

## Prompt

Start each run in a fresh subagent with no access to the code, the brief or this
conversation. Give it only the frozen shots, the list above and this prompt:

```
You see screenshots of one mobile app. You know how apps made by AI tools with default
choices tend to look, and how apps made by a design team for one product look.

Answer two things.

1. Verdict, one of: AI-made / borderline leaning AI-made / borderline leaning designed /
   designed.
2. The tells that drove your verdict, most damaging first. For each: the screen label, what
   you see, and why it reads as made by default rather than for this product.

Rules:
- Under 400 words.
- No praise. No redesign. No score.
- Items in "Required by the brief" are not tells.
- If a screen shows nothing that reads as default, do not mention it.
```

## Reply shape

```
Verdict: borderline leaning AI-made
1. /home · light: every section starts with an all-caps grey label over a large number;
   reads like a dashboard kit, not this café's day.
2. /menu · light: each item has a pastel disc with its initial letter; no item is drawn.
3. /orders · dark: cards and ground are one tone; looks like the light theme inverted.
```

## Acting on it

- Map each tell to a tell ID where one fits (`T4`, `I3`, `C9` in the example).
- Act on tells **both** runs name. A tell one run names goes in the report as "named once",
  with no fix, unless it is a tier 1 problem: then write it as a normal finding anyway.
- Turn each shared tell into a finding in the normal shape; it keeps its tier.
- After fixes, freeze new shots and run two new fresh subagents. Never reuse a run that has
  seen earlier shots.
- Stop after 3 rounds. After that, runs start to contradict each other on taste.

## When it cannot run

If your environment cannot start fresh subagents, or cannot give them images, write:

```
Cold-read panel: not run (<reason>).
```

Still answer the user: give your own read as normal findings labelled "reviewer read, not a
cold read". Never answer the prompt yourself and report it as a cold read.
