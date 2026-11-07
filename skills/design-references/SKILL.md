---
name: design-references
description: "Use when designing, restyling or recreating screens in a FluxNative app, or when the user names an app or flow to emulate: search FluxNative's catalog of real app journeys and screens, then adapt structure and density to the app's own theme, never copying another product."
---

# Design from real app references

## When to use

- A new screen or flow in a FluxNative app, where real products show how the journey usually goes (onboarding, checkout, settings, a feed).
- The user names an app to look like, or attaches a screenshot to match.
- A screen looks generic and you need evidence of how good apps solve it.

The catalog is the same for every project, so its searches take no `project`. It holds mobile apps: use it for an app, not a landing page, which has its own template library (`search_landing_templates`, in the `app-workflow` skill). Reading and changing the app's own source takes its id as `project` (the `app-workflow` skill).

## The catalog

FluxNative keeps a catalog of real mobile apps, broken into journeys (flows) and screens. Find references by the screen pattern or the journey you are building, not by the app: the catalog is thin per app and rich per pattern. Two searches reach it, each by a prose `query` or by the `ids` a result returned:

| Tool | Returns | Cost |
|---|---|---|
| `search_flows` | Journeys, each with its full ordered list of screen ids | Text only |
| `search_screens` | Screens as inline images | A few hundred vision tokens per screen: about 260 on Claude for 299×648, about 760 for a 512-wide screen |

- Every flow and screen names its app: `app_id`, `app_name` and a one-line `app_summary`. Pass `app` (an `app_id`) to either search to keep to that one product, for example to pull more of one app's screens so a journey stays consistent; `app` alone lists that app's journeys or screens.
- Write a query as one sentence about one subject: what happens in the journey, or what appears on the screen and how it is composed. Keyword lists, negations and style words rank badly.
- Compose each screen from exemplars of its kind, across apps. An empty result is ordinary: describe the screen another way, or ask the other search.
- A screen with an `exemplar_id` is FluxNative's own coded screen: start the feature file from it with `copy_template` (`exemplar:<slug>`), never by retyping it, then change its data, copy and brand.
- A screen marked `identity: "open"` may also inform the design DNA's palette family, illustration and density, never a mark or logo.
- `load_skill` holds the design procedures (`design-direction`, `anti-slop-invariants`, `accessibility-pass`, …).
- Fetch one journey's screens (`search_screens` with the step `ids`), build that journey, then come back for the next. Never fetch a whole app's screens at once. `search_screens` with `exclude_ids` skips screens you have already seen.
- Hosts that render MCP Apps show flows and screens in an interactive view, where the user can pick a screen or flow and hand its id back to you.

## Procedure

1. **Read the app first.** `src/theme` and `@/components` define what you can use, and the app's design DNA says who it is: `.fluxnative/memory/design-dna.md`, with `design-screens.md`, `design-components.md` and `product-dna.md` beside it when the app has them. Note the palette roles, type scale, radii and icon family. Identity comes only from these files, never from a reference.
2. **Find the pattern.** Search the journey with `search_flows`, and each screen kind with `search_screens`. Pick two or three references that fit the app's audience and density; they may come from different apps.
3. **Write down what you will take** from each screen:
   - the zones from top to bottom, each as a share of the height;
   - the anchor and the reading order;
   - the repeated units (row, card, chip) and their anatomy;
   - what stays fixed and what scrolls;
   - the states shown (selected, empty, error) and where each control leads.
4. **Map it onto the app's tokens.** Colors land on theme roles, type sizes on the nearest type roles, spacing and radii on their scales. Change a role once in `src/theme` when the whole app should shift; never scatter literals.
5. **Build structure first,** with the app's real copy and its kernel components, then compare in the live preview (the `preview-verification` skill) on the closest device.
6. **Fix differences in order:** structure and proportions, then type, then color and surfaces, then detail (icons, crops, borders). Re-check after each tier, not after each fix.
7. **Stop** when someone holding both images would match every zone and the anchor without hesitating. Note what differs on purpose.

## What a reference gives you

| Reference | Take | Leave |
|---|---|---|
| A catalog journey or screen | Structure, hierarchy, density, navigation, the order of steps | Its palette, brand, copy, imagery and exact geometry |
| A screenshot of the user's own product | Everything visible: it is the spec | Nothing; ask about what it does not show |
| A screenshot of someone else's app | Structure and density, as with the catalog | Brand, copy, imagery, logos |

## Never

- Never copy another product's brand, copy, imagery, icons or logo into the app.
- Never add a second icon family, or hard-code colors and sizes outside `src/theme`.
- Never fetch screens you are not about to build: each one costs vision tokens.
- Never draw standard platform chrome by hand when the reference shows a standard tab bar or header; use the platform's.

## Report

- The references you used (app and journey names) and what you took from each.
- Token changes made in `src/theme`, if any.
- Deliberate differences from the references, and anything not yet checked in the preview.
