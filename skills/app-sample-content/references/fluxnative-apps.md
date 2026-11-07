# FluxNative apps

Read this file only when the project matches the marker below. For any other Expo or React Native app, ignore it.

## The marker

The files decide, and both parts must hold:

1. The root `AGENTS.md` starts with the line `# Generated app source contract`.
2. `src/platform/`, `src/theme/` and `src/features/` exist.

`.fluxnative/` alone is not enough: fresh apps do not have it yet. Over the FluxNative MCP, read `AGENTS.md` with `read_file` and check the folders the same way.

Every project, marked or not, follows its own `AGENTS.md`. It is the contract and wins over this file and over every skill in this pack.

- **No marker** (another first line, a missing folder, or no `AGENTS.md`): not a FluxNative-template app. Ignore the rest of this file. Fix inside the app's own styling system, change the minimum and never restructure it toward this layout.
- **Marker and `src/config/look.ts`**: a FluxNative Commerce app. The rules below hold where its `AGENTS.md` agrees; its look is data in `src/config/look.ts`, described in its `DESIGN_GUIDE.md`, and that file wins on look and components.

`list_apps` prints a hint at the end of some rows. These suffixes are quoted from its output:

- `(imported: the customer's own Expo source)`: usually no marker.
- `(FluxNative Commerce)` and `(FluxNative Commerce, migrated from FluxStore or FluxNews)`: a Commerce app.

A suffix is a hint, not the rule. When it and the files disagree, the files win.

## Tokens

- Colours, sizes, spacing, radii and fonts come only from `src/theme/`. No colour, size or font literal in a screen or component.
- Read the token names before using them: open `src/theme/` locally, or call `kernel_api` with `module: "theme"` over the FluxNative MCP. Never guess a token name.
- A fix to a palette, radius or type problem goes into the theme once, not into each screen.
- The accent and ground derive from `brandSeed` in `src/theme/brand.ts`. Change that value rather than patching colours. When it is not the template seed `#5E6B78`, or `.fluxnative/memory/design-dna.md` already holds a direction, keep it unless the brief contradicts it.

## Components

Every component comes from `@/components`. Names you can expect:

- layout: `Screen` (with `topBar` and `bottomBar`), `List`, `ListSection`, `ListItem`, `Sheet`, `TopAppBar`, `TabBar`;
- tabs: `AppTabs` lives in `src/app-shell/` and is used by the tabs layout, not by screens;
- controls and content: `Icon`, `IconButton`, `IconTile`, `Button`, `Card`, `AppText`, `PlainText`, `TextField`, `Chip`, `PhotoCard`, `MediaHeader`, `HeroFigure`;
- charts: `BarChart`, `LineChart`, `DonutChart`, `ProgressRing`.

Confirm each name in `src/components` or with `kernel_api` before you use it. Never invent a component or a prop; if one you need does not exist, build it from these in the feature folder. Text goes through `AppText` or `PlainText`, never React Native's `Text`.

Common needs and the component that meets them:

- a screen's key number with a smaller unit: `HeroFigure`;
- rows on the ground with hairlines: `ListSection` and `ListItem`;
- a tinted leading tile: `IconTile`;
- a place, dish or product: `PhotoCard`;
- bars and safe areas: `Screen` with `topBar` and `bottomBar`, which pads content by the bars' height.

## Rules

- Dialogs and sharing: `useDialog()` and `useShare()`. `Alert.alert` and `Share.share` fail on web; each call is a finding by itself.
- Back controls: `useGoBack(<parent route>)` from `src/hooks/`.
- Dates, numbers and prices: through `@/i18n/format`, never formatted by hand.
- Direction: start and end, never left and right.
- Structure:
  - never edit `src/platform/**`;
  - routes in `app/` only re-export `src/features/<feature>/<Name>Screen.tsx`; change the screen, not the route file;
  - data, sample data included, goes through `src/services/<domain>`, never inline in a screen.
- Never add a UI kit or NativeWind.
- Purchases go through `src/services/iap` once a purchases recipe is added (`search_services` names it), never from a screen. If it exports `PURCHASES_SOURCE = "demo"` or `"off"`, no purchase is real: report it as not verified.

## One look on every platform

A FluxNative app has one look on iOS, Android and web. A `Platform.OS` branch that changes the look is a finding. Insets, edge-to-edge and the keyboard still follow each platform. For these apps this overrides any per-platform look advice, including per-platform numbers, in this pack or in other skill packs; the kernel's components already set targets and chrome.

## Sample data

- Never invent the signed-in person: no name they did not give (greet without one). Sample names are for other people only, such as staff, sellers, reviewers or a demo business's contacts.
- Keep the one quiet line saying the data is a demo, and an order or booking confirmation saying the business does not receive it yet. They are part of the contract, not review notes.
- What the person adds persists through `@/services/storage`.

## Design memory

- The brief lives in `.fluxnative/memory/design-dna.md`. Read it before any design change; write brief changes there, with the date, instead of `DESIGN.md`.
- A note in `.fluxnative/memory/` whose `tags:` line contains `decision` is the owner's choice. Never flag it as a problem and never undo it.
