# Motion gate

Decide whether a motion should exist before deciding how it moves. Deleting an animation is a valid result. For springs, worklets and gesture code, use the Expo animation skill if it is installed.

## 1. Frequency

Frequency comes from the route map: tabs, rows and toggles are many times a day; checkout and onboarding are rare.

| How often the person sees it | Rule |
|---|---|
| Many times a day (tab switch, list scroll, opening a row) | The platform default, or nothing |
| Tens of times a day (toggle, add to basket, check a habit) | Under 150 ms, no overshoot |
| Rare (first order, goal met, upgrade done) | Delight is allowed, once, and it never blocks input |

## 2. Purpose

Name the purpose in one word. If no word fits, delete the animation.

- **Feedback:** the control heard the touch.
- **Orientation:** where the new screen came from or went.
- **Continuity:** the same object in a new place (a thumbnail growing into a detail).
- **Reveal:** data arriving (a figure counting to its value once).
- **Reward:** a rare moment worth marking.

## 3. Durations (house heuristic)

| Kind | Duration |
|---|---|
| Press feedback | 100–150 ms |
| Control state (switch, chip, checkbox) | 150–200 ms |
| Surface (sheet, menu, dialog) | 250–300 ms |
| Screen or block entrance | 300–400 ms |
| Exit | about 2/3 of its entrance |
| Stagger between items | 30–50 ms, over at most 6 items |
| One celebration | up to about 800 ms, input stays live |

Anything over 400 ms that is not a single celebration reads slow.

## 4. How it moves

- Animate transform (translate, scale, rotate) and opacity only. Never animate width, height, margin or top per frame.
- Springs only for gestures (drag, swipe, a sheet following a finger) and rewards. Controls and navigation use an ease-out curve with no overshoot.
- Press feedback starts on press-in, not on release.
- State never waits for an animation to finish: the data is saved first, then the motion plays.

## 5. Haptics

- Use them for success, a selection that changed, and a real impact (a drag snapping into place).
- One haptic per action.
- Never on scroll, typing, or every tap.

## 6. Reduce Motion

- When the system setting is on, decorative motion becomes a cross-fade or nothing.
- Skeleton shimmer stops; skeletons stay still.
- Feedback that carries meaning (a field shaking on error) is replaced by a visible message, which should be there anyway.

## Never ship

- A pulsing or glowing call-to-action.
- An idle loop that runs with no input (a breathing illustration, a rotating badge).
- An auto-advancing carousel.
- Bounce on chrome: tab bar, headers, navigation, error messages.
- An entrance over 400 ms that blocks input.
- Parallax on a list.
- Haptics on scroll or typing.
- A looping animation with no stop condition (check the code for repeat or loop calls).
