# Counts

The Step 1 count commands, their known false positives and the fallback without `rg`. Run each count before you change anything, and again at the end.

## Scope

The source folders the Sniff step found (default: every folder holding .ts, .tsx or .json except node_modules, build output and `**/theme/**`). Write that scope at the top of the report.

## Commands

Every command ends with `<scope folders> -g '*.ts' -g '*.tsx' -g '*.json' -g '!**/theme/**'`, shortened to `…` below:

```sh
rg -n -i 'picsum\.photos|placehold\.co|placeholder\.com|pravatar\.cc|loremflickr\.com' …  # 1 placeholder hosts
rg -n 'John Doe|Jane Doe|John Smith|Jane Smith|Test User|Demo User|Lorem|Item [0-9]|Description here|Acme|123 Main St|\$99\.99|\b1,234\b' …  # 2 placeholder text
rg -n "'Title'|\"Title\"" …                                                   # 3 bare "Title" strings
rg -n -w 'Maya|Priya|Theo|Marcus|Leah|Ines|Hana|Jules|Sarah|Alex|Emma|Sam|Mia' …  # 4 default cast
rg -n -i 'elevate|seamless|effortless|unlock your|supercharge|journey|amazing|magic|next-level' …  # 5 slogan words; copy.md adds "discover" by eye
rg -n -i 'coming soon' …                                                      # 6 "coming soon"
```

## Known false positives

Usual count for each: 0. Known false positives:

1. a test that checks the placeholders are gone (the hosts are spelled out so a text field's `placeholder` prop does not match);
2. `Item 1` as a type or test name; a real price of 99.99; a real figure of 1,234;
3. a prop or i18n key named `Title`;
4. a name in a code comment or doc string; a name the user or a real customer gave you (see `people-and-places.md`);
5. code identifiers, comments, licence text;
6. a feature flag name that never reaches the screen.

## Fallback without rg

When `rg` is missing: use `grep -rnE` with the same pattern, add `-i` for counts 1, 5 and 6 and `-w` for count 4, add `--exclude-dir=theme`, and keep the same folders with `--include='*.ts' --include='*.tsx' --include='*.json'`. Drop any scope folder that does not exist.
