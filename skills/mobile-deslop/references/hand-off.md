# Hand-off

Load this when the work needs sample content, a second opinion or a new screen the user explicitly asked for (this skill adds none on its own). Other skills are used only if installed.

- Sample people, figures and copy:
  - If `app-sample-content` is installed, use it.
  - Otherwise apply the D and W rows of `tells.md`, derive every total from the same records, and use names from the app's market.
- A second opinion, or a cold "does this look AI-made?" panel:
  - If `mobile-screen-review` is installed, use it. It uses the same seven tiers as Step 5.
  - Otherwise re-read your findings against the Not tells list in `SKILL.md` and drop any that rest on taste alone.
- New screens, only ones the user explicitly asked for:
  - If `mobile-design-brief` is installed, use it.
  - Otherwise write each new screen's job and lead element in one line before building it.
- Theme tokens: If `expo-design-system` is installed, its token module is the theme: extend it, never add a second one.
