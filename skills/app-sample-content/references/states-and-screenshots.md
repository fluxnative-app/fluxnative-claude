# States, sample labels, store screenshots and visual fallback

## States a mockup leaves out

A mockup, a screenshot or a first build shows one happy moment: a full list, loaded, nothing wrong. For each screen that shows sample data, decide what each state below shows, in data and in copy, and write the decision as a comment beside that screen's records.

| State | Data | Copy |
|---|---|---|
| Zero items | an empty record set (fresh install, cleared filter, search with no match) | the situation in one sentence plus one real action that fills it |
| One item | a set with one record: totals equal that row, no grid or chart drawn for one point | the plural rule gives "1 order", never "1 orders" |
| Loading | none yet | no copy; the layout's skeleton carries it |
| Error | the fetch fails | what happened, what to do, and a retry; no status codes, stack text or provider names |
| Not found | the record is gone | say it is gone and give a way out |
| Failed submit | the save fails | the input stays in the form |
| Saved record reopened | what the person saved, read back after a reload: their words and figures, not sample ones | the title is their wording; dates relative to today |

Keep each state's records in the record source next to the main set (for example `emptySet`, `oneItem`). If the sample module has no way to reach a state, add at most a `__DEV__`-guarded switch inside that module, or list the state under Not verified.

## Sample-mode labels

- One quiet label on the hero ("Sample data. Add your first expense to replace it."), never a tag on every row.
- No "coming soon" rows: a feature that is not built is left off.
- No notes to App Review or to the owner on a working screen; those go in the review notes.

## Store screenshots

- One market and one locale per screenshot set.
- List the screens to capture: the 3 to 5 that show the app's main task.
- Each one populated, with no empty state, no sample label and no past bookable slot.
- Capture at the store's required size, or say in the report that it was not captured.

## Visual fallback when mobile-deslop is not installed

Visual tells (colour, type, layout, icons, states) belong to `mobile-deslop`, if installed. Fallback when it is not:

- one accent colour, one filled primary button per viewport;
- no emoji as icons; one icon family;
- rows on the ground, a card only for one self-contained object;
- loading as a skeleton at the final layout, a spinner only inside a control.
