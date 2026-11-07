# Copy

Before-and-after pairs for the words on a screen. The "before" column is what generators write by default; the "after" column says what the product does or what is true today. Every pair can be checked in a screenshot.

## Slogans to product words

| Before | After |
|---|---|
| Elevate your fitness journey | 3 workouts this week |
| Unlock seamless ordering | Order ahead |
| Effortlessly manage your money | Spending |
| Discover amazing places near you | Near you |
| Supercharge your productivity | Today's tasks |

Words to look for: elevate, seamless, effortless, unlock your, supercharge, journey, amazing, magic, next-level (count 5 in `counts.md`). Also read any headline with "discover" by eye. A product word names the screen or the task.

## Greeting to the day's fact

| Before | After |
|---|---|
| Good morning, Sam! 👋 | Rent due in 4 days |
| Welcome back! | 2 orders on the way |
| Hi there, ready to learn? | 12 words to review |
| Let's crush today's goals! | 6,240 of 8,000 steps |

If the person's name helps, put it in small text, not the headline.

## Instructions removed

| Before | After |
|---|---|
| Tap any item to edit it | (removed; the row has a chevron or edit control) |
| Swipe left to delete | (removed only when a visible delete control exists; otherwise keep it and report it) |
| Use the button below to add a new expense | Add expense (the button itself) |
| A word counts as learned once you answer it right 3 times | (removed, or in a help screen) |

## Each fact once

| Before | After |
|---|---|
| Title "Orders" with a heading "Your orders" below it | Title "Orders" |
| "Tue 12 Mar" chip beside "Tuesday, 12 March" | One of them |
| Row "Book class" with a button "Book class" | Row "Yoga, 18:30" with a button "Book" |

## Buttons

| Before | After |
|---|---|
| Submit | Send request |
| Let's go! | Start workout |
| Learn more | See plan details |
| OK (after a delete) | Delete booking (on the confirm itself) |
| Continue / Next / Proceed in one flow | One word for the same step everywhere |

A button says what happens when it is pressed. Sentence case, no exclamation marks.

## Empty states

| Before | After |
|---|---|
| No data | No expenses this month. **Add expense** |
| Nothing here yet! | Your saved recipes show here. **Browse recipes** |
| Oops, it's empty 🙈 | No bookings. **Find a class** |
| No results | No classes match "yogaa". **Clear search** |

One sentence for the situation, one action that fills it. No illustration filling the screen around a single line.

## Errors

| Before | After |
|---|---|
| Something went wrong | Couldn't load your bookings. Check your connection. **Try again** |
| Error 503: Service Unavailable | The menu didn't load. **Try again** |
| AuthApiError: invalid_grant | Your session ended. **Sign in again** |
| 404 Not Found | This booking was cancelled or moved. **See bookings** |
| Failed to submit (form cleared) | Couldn't send. Your note is still here. **Try again** |

What happened, what to do, one action. No codes, no provider or library names, and a failed submit keeps what the person typed. One failure shows one message, where it happened.

## AI labels

| Before | After |
|---|---|
| ✨ AI-powered insights | Suggested (once, on the section) |
| Analysed your last 30 days and matched 12 transactions with high confidence | You spend less on weekdays than at weekends |
| Answered by AI from your notes. Confidence: 82% | (the answer, with the section labelled once) |
| Low confidence: the category may be wrong | State the result plainly; if the app has no choice control, drop the confidence text (do not add a control) |
| AI label on every card in the list | One label on the list's heading |

Label AI once where it starts. Show the result in the app's words. Never show a confidence score; change copy only, not controls.

## Sample-mode and review notes

| Before | After |
|---|---|
| "Sample" tag on every row | One line on the hero: "Sample data. Add your first expense to replace it." |
| Premium reports: coming soon | (row removed) |
| Note for App Review: payments are in test mode | (removed from the screen; written in the review notes) |
| Demo user / Test account in the profile header | The person's own name if given; otherwise no name (a plain "Profile" with a sign-in action) |

## Sample writing

Text the person would write themselves: notes, messages, reviews, journal entries, prompts.

| Before | After |
|---|---|
| Today I practised gratitude and felt my mindfulness grow. | Bus late again. Got the good coffee anyway. |
| Absolutely loved the vibrant, fresh flavours! 5 stars! | Portions were big. Soup a bit salty. Would go back. |
| Remind me to hydrate, stretch and meditate every morning | remind me to call the dentist tmrw |
| Three sample AI prompts, one per feature case | Three ordinary prompts; the feature handles them however it does |

Rules for text a person in the app would write (notes, messages, reviews, journal entries, prompts to an AI feature):

- ordinary and a little clumsy, as people type on a phone;
- not punchlines, not built around the feature's keyword;
- sample AI inputs do not walk through each case the feature handles, one per row.

## Check before you finish

- [ ] The slogan-word count is 0.
- [ ] Each headline states a fact about today, not a greeting.
- [ ] No screen explains how to use itself.
- [ ] No fact repeats on one screen.
- [ ] Every empty state has one sentence and one action; every error has an action and no codes.
- [ ] Every state in the states table (`states-and-screenshots.md`) has its copy decided, not only the full list.
- [ ] AI is labelled once per section, with no process narration.
- [ ] One sample label at most per screen; no "coming soon".
