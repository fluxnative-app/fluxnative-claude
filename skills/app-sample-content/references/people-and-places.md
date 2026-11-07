# People and places

Sample people make a screen look used or staged. A model left alone reaches for the same few names, one person from each part of the world, and studio headshots. Each rule below can be checked by reading the record source or a screenshot.

## Choose names by market

1. **Name the market.** One country or city the app is for, from the brief, the app's language and currency, or the user. A multi-market app picks one market per screen set (for example, one set of store screenshots per locale).
2. **Name the age band.** Who uses the app today (for example 25–40). Their given names are the ones common in the decades they were born, not this year's baby names.
3. **Draw from that market.** Given names and family names a local would find ordinary, in the market's script when the app shows that script. If you can, check against the market's public name statistics; otherwise use what you know and keep to common names, not rare ones.
4. **Write them as the market writes them.**
   - Name order: family name first where that is the local order (for example Vietnamese, Japanese, Korean, Chinese, Hungarian).
   - What the app shows: many markets address people by given name; some by family name with a title. Follow the app's own pattern.
   - Diacritics kept (Nguyễn Thị Lan, not Nguyen Thi Lan, in a Vietnamese UI).
5. **Vary the set.**
   - In a list of up to 6 people, no two share a first letter, and lengths differ (short, long, with and without a family name shown).
   - The mix of genders and ages follows the app's users, not an even split chosen for looks.
   - A single-market app does not hold one person from each continent. A city with mixed origins can show that mix in the proportions a local would expect.
6. **Keep a person consistent.** The same person has the same name, photo, initials and role on every screen.

## The default cast: check, not ban

These names appear in generated apps far more often than chance: **Maya, Priya, Theo, Marcus, Leah, Ines, Hana, Jules.** Count 4 in `counts.md` also flags the usual English defaults: **Sarah, Alex, Emma, Sam, Mia.**

- When the count finds one, ask: did the user, a real customer or the market put it there? If yes, keep it and say why in the report.
- If the model put it there, replace it with a name chosen by the method above.
- Never swap one default for another default (Maya → Mia is the same problem).

## Photos of people

- About a third of people have no photo. They get the app's no-photo avatar (initials or a neutral glyph, as the platform's contacts app does). In a list of fewer than 3 people use no photo for at least one.
- Personal content (friends, family, a group) looks like phone snapshots: candid, mixed light, ordinary backgrounds. Studio headshots belong only where the real product would show them (a clinic's staff page, a marketplace's verified sellers).
- Use a photo only if it is yours or licensed; list each photo, its source and licence in `ASSETS.md` beside the image folder. If you cannot show that, use the app's no-photo avatar. No placeholder hosts, no photos pulled from search results.
- The same face never appears twice as two different people.

## Never real people

- No public figures, athletes, creators or politicians, even as a joke.
- No names of the team, the user's contacts or customers unless the user supplies them for this purpose.
- No photo of an identifiable person unless its licence allows this use.

## Places, businesses and contact details

- **Cities and districts:** real ones from the market are fine; they make the data local.
- **Businesses:** invent names that sound local for the category (a bakery, a gym, a clinic). Never a real brand or chain.
- **Street addresses:** a real street in the right city with an invented number, or an invented street. Never a real private address.
- **Phone numbers:** use a range the market reserves for fiction where one exists (for example 555-01xx in North America, 07700 900xxx in the UK). Where none exists, leave the number off or show it masked.
- **Email and web addresses:** `example.com`, `example.org` or `example.net`, which are reserved for examples. Never a domain someone owns.
- **Distances, travel times and opening hours** fit the city: walking minutes in a dense centre, driving minutes in a spread-out suburb, hours as local shops keep them.

## Check before you finish

- [ ] The market and age band are written at the top of the record source.
- [ ] The default-cast count is 0, or each hit is explained.
- [ ] About a third of people have no photo (at least one in a list under 3); every photo is listed in `ASSETS.md` with its source and licence.
- [ ] No real person, brand or private address; contact details use reserved ranges or are left off.
- [ ] Each person is the same on every screen.
