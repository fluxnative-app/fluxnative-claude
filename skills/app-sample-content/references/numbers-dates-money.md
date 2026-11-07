# Numbers, dates and money

Generated figures give themselves away by shape: round, evenly stepped, fitted to the layout, and different on each screen. Every rule here can be checked by reading the record source or the rendered text.

## Uneven, lived-in series

Signs of a made-up series (each one a reason to look):

- every value ends in 0 or 5;
- equal steps (120, 140, 160, 180);
- a staircase that only rises or only falls;
- a bar that sits exactly on a gridline, or a peak that is exactly the axis maximum;
- dates every second or third day, or all on the same weekday;
- a list whose length exactly fills the grid or the first screen.

How to make one that reads as real:

1. Start from a value plausible for this person and market.
2. Give it the category's rhythm: spending rises at the weekend and on payday; workouts skip a day or two; sleep is shorter before work days; a shop's orders peak at lunch.
3. Vary each point by an irregular amount, not a fixed percentage.
4. Leave one gap (a missed day) and one outlier where the category has them.
5. Keep at most about a third of values round. Round figures are right where people choose them: a budget, a goal, a rent.

A chart gets at least 5 real points. If the records do not have 5, show the figure without a chart.

## Equations

Every total, count, average or percentage on screen has an equation written in a comment next to the selector or record that produces it. Check each one before you finish.

| Shown | Equation |
|---|---|
| order total | sum(quantity × unit price) + delivery + tax − discount = total |
| budget left | budget − sum(spent this period) = left |
| badge or tab count | count(rows that match) = badge |
| streak | consecutive days with a record, ending today or yesterday |
| average | sum(values) ÷ count(values), over the period the label names |
| percent | part ÷ whole, rounded the same way everywhere it appears |
| progress | done ÷ goal, and the ring or bar shows that fraction |

Rules:

- Store only base facts (lines, quantities, unit prices, flags). Derive totals, counts and badges in one selector or helper every screen uses; never type a total by hand.
- Where a figure must be stored (a fixed past balance), write its equation in a comment and check it once with a throwaway script or an existing test file (`npm test -- --run <file>` or `npx tsx <script>`). Never add an assertion to app code.
- Round once, at display. Sum unrounded values. If the shown lines then do not add to the shown total, round the lines so they do, or show a rounding line.
- Tax sits between the lines and the total, as the market's receipts show it (included or added).
- The same fact (a balance, a count, a next booking) has the same value on every screen. Search the record source for each figure that appears twice.

## Money

- Every amount carries its currency, by symbol or code. An amount with no currency is a finding.
- Store the ISO currency code with the amount; format through `Intl.NumberFormat` (or the app's format module) with the market's locale. That sets the separators, symbol position and minor units.
- Minor units follow the currency: some have none (for example JPY, VND, KRW), some have three. Never show `.00` on a currency that has no cents.
- Prices are plausible for the market. Compare with what a local pays for the same thing; the same coffee or class can cost several times more in one city than another. Do not carry one market's figures into another by changing the symbol.
- Prices that people set by hand (a menu, a class pack) can be round or end in 9 as that market's shops do. Prices that come from arithmetic (a split bill, a total with tax) are not round.
- No "was" price or discount unless the sample story needs one, and then the arithmetic holds.

## Dates and times

- Compute sample dates from the current date at runtime (now minus 2 days), never fixed strings. A screenshot taken next month still says "Yesterday" for yesterday's record.
- Relative words for the near past: "Today", "Yesterday", then the weekday within the last week, then the date.
- Show the year only when it is not the current year.
- Bookings and slots: nothing bookable in the past. An empty future day is fine; a full past day is a finding.
- Times follow the locale's clock (12 or 24 hour), and one format per screen. Never "9 AM" beside "9:00 AM".
- Times fit the activity: a dinner booking at 19:30, not 03:00; a morning run before work.
- Ages, birthdays, "member since" and record dates agree with each other.
- Durations are said as a person says them: "about 4 weeks", "2 h 15 min", never "29.8 days" or "135 minutes" on a summary.

## Counts and plurals

- Use the platform's plural rules (`Intl.PluralRules`) or the app's i18n plural helper. Never `count + " items"` or `item(s)`.
- Write the zero case as a sentence when it is a state ("No orders yet"), not "0 orders".
- Languages differ: some have no plural form, some have several. Let the rules pick; never assume English.

## One format per screen

- One date format, one time format and one number format per screen, and the same format for the same fact on every screen.
- Digits that change or line up (prices in a column, timers, balances) use tabular figures where the font has them.

## Check before you finish

- [ ] No series has equal steps or all-round values without a reason.
- [ ] Every total, count and percentage has a written equation, and each holds.
- [ ] Every amount has a currency; minor units fit the currency.
- [ ] Dates are computed from today; no bookable past slot; the year shows only when needed.
- [ ] Plurals come from plural rules.
- [ ] Each screen uses one format per kind of value.
