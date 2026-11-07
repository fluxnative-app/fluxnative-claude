---
name: app-workflow
description: "Use when the user wants to build, change, fix or explain a FluxNative app or landing page through the FluxNative connector: find or create the project, then read, edit, check and preview its source by its id. Not for an Expo project on this machine."
---

# Work on a FluxNative project

## One connection, every project

The plugin's `fluxnative` server, `https://mcp.fluxnative.app/mcp`, reaches every app and landing page the account can edit, in every workspace. There is nothing else to add or connect.

- `list_apps` and `create_app` find and make projects.
- Every project tool takes `project`, the project's id from `list_apps`, as its first argument. Pass the same id on every call about that project.
- The design catalog searches (`search_flows`, `search_screens`, `search_services`), FluxNative's skills (`load_skill`) and the landing-template library (`search_landing_templates`, `get_landing_template`) take no `project`: they are the same for every project.

The source lives in FluxNative, not on this machine. Local file, shell and git tools cannot see it, and a copy on disk is not the app. Every source write becomes a revision the user can see and undo in the FluxNative builder. Backend tools and `preview_act` change live data, which no revision undoes.

## 1. Find or create the project

1. Call `list_apps`, with `query` when the user named the project. Each row has an `id`, a name, a `kind` (`app` or `landing`) and a workspace, newest activity first; it marks an imported app and names the app a landing page presents.
2. When several projects match, or none, ask which one. Never guess between two.
3. A workspace the result says is left out has a plan without remote MCP. Tell the user it needs a plan that includes it; do not look for another way in.
4. Call `create_app` only when the user asks for a new project. Give it a `name` (up to 30 characters), the `kind`, a `description` in the user's own words (the brief every build of it starts from) and, when they have several workspaces, a `workspace` id from `list_apps`. A landing page may name the app it presents as `parent_project`. It builds nothing and spends no credits; the plan's project limit still applies.

Say which project you are working on (name and id) before you change it, and keep using that id.

## 2. Read the project's contract

Each project carries its own contract at the root of its tree: `read_file` with `project` and `path` `AGENTS.md`. Follow it.

- An app is Expo + React Native source: Expo Router routes in `app/` re-export one Screen each from `src/features/<feature>/`, data goes through `src/services/<domain>/index.ts`, `src/theme` is the only token source, and `src/platform/**` is never edited. In an unfamiliar app, also read `README.md` and `.fluxnative/app-spec.json`; the spec lists screens, navigation and flows with their status.
- An app's identity is its design DNA: `.fluxnative/memory/design-dna.md`, with `design-screens.md`, `design-components.md` and `product-dna.md` beside it when the app has them. Read them before designing a screen.
- An app's `AGENTS.md` is written for FluxNative's built-in assistant. Its `<app_map>` and `<kernel_api>` are `kernel_api`: every kernel component's props and token's name, used without opening their files. What it calls finishing is your final report, after `run_checks`. Read the skills it names with `load_skill`. Nothing here holds back a UI kit or NativeWind, so follow its Libraries rule yourself.
- A new app (the starter screen is still there, no `.fluxnative/app-spec.json`): `kernel_api`, then its component folders. Before the theme, `search_flows` for the journey and `search_screens` for each kind of screen: their images are the bar for looks (type scale, spacing, surfaces, colour use), not layouts to copy, since this app's features decide its screens. Write the lead reference's look into `design-dna.md` with their ids, build each screen in it from kernel components, and judge every screenshot beside those images before your report.
- An imported app (marked in `list_apps`) is the customer's own Expo app; its `AGENTS.md`, if any, is theirs. Locate with `grep` and `code_graph` before reading, follow its conventions, change the minimum, and never restructure it toward the template.
- A FluxStore or FluxNews migration keeps the owner's settings in `src/config/` and the Home layout in `src/features/home/home.sections.ts`; data comes through `src/services/commerce`. Never print, copy or ask for the WooCommerce keys in `.env`.
- Notes in `.fluxnative/memory/` tagged `decision` are the owner's standing rules, right after the user's request; save a lasting rule the user states as one.
- A landing page is a static Vite, React and Tailwind site: facts and shared copy in `src/content.ts`, sections in `src/sections/` (one laid out from a template reads its words, links and images from its `<Name>.copy.ts`), tokens in `src/index.css`, only the packages in `package.json`.
- Before designing a landing page or section: `search_landing_templates` (an app's page: ask for `app-showcase`), `get_landing_template`, then `copy_template` from the template's id to `src/App.tsx`, and write each `<Name>.copy.ts` for this product, keys kept. Change markup only with `edit_file` (drop, repeat or reorder items); never retype a template section. No template image comes over this connection.
- A landing page never invents contact details or URLs and never says "sample", "coming soon" or "to be confirmed": content the brief lacks is believable sample content, listed in your report. A page that presents an app takes its name, brand, icon and real features from the app's source (its id as `project`). The owner deploys it from the page's Publish.

## 3. Read before you write

- `list_files` with a `glob` such as `app/**/*.tsx` maps the tree; `include_metadata` adds each path's SHA-256 and size without reading it.
- `grep` finds the exact text a person sees or the symbol named (`path` or `include` to narrow, `output` `files` or `count` for an overview). `code_graph` (apps) follows code across files: definitions, references, callers up to the screens.
- `read_file` returns the content, the committed revision and the full-file SHA-256; a long file continues from `start_line`.

## 4. Change the source

- `edit_file` replaces an exact `old_string` with `new_string`. Pass `expected_sha256` from your last read of that path.
- `write_file` creates a file (`expected_sha256` null) or rewrites a whole file.
- `apply_changes` commits up to 50 create, update, delete, move and copy operations as one revision. It is the only way to delete, move or copy a path, and the safe way to rename a component together with its imports.
- `copy_template` starts from FluxNative's own code: a template id for a page or section, `exemplar:<slug>` for an app screen.
- Studio theme files (`src/theme/brand.ts`, shape, typography, elevation, motion, fonts) change one literal at a time with `edit_file`.
- `upload` puts a file from this machine into the project. `step: "begin"` returns a single-use command that streams the file straight to FluxNative (it carries a short-lived upload credential); run it, then `step: "commit"` with its `upload_id` writes it at a `path`. When the command cannot run, `step: "inline"` sends up to 2 MB as base64 in one call. Both take the path's `expected_sha256` (null for a new file).
- Hashes are checked per path, so edits to different files do not conflict and can run in parallel.

When a write is refused:

- **Changed since you read it.** Someone else wrote that file. Read it again, re-apply your change to the new content, and retry once.
- **`agent_editing`.** FluxNative's built-in assistant is writing those paths right now. Work on other files or wait; never overwrite its work.
- **Protected paths.** Anything under `.github/`, `.fluxnative/` other than `memory/`, deleting or moving `.fluxnative/app-spec.json`, and a new top-level folder are refused. `.env` is writable.
- **App-spec diagnostics** in a write result: fix each one.

## 5. Check your work

- `run_checks` runs TypeScript, ESLint, Jest and the iOS and Android JavaScript exports on the committed source (on a landing page, the typecheck and the static build). Run it after meaningful changes and fix what it reports. It does not prove native linkage or device behavior.
- For anything visible or interactive, use the `preview-verification` skill: `build_preview`, then `preview_view`, `preview_act` and `preview_logs`. Check a landing page at desktop, tablet and phone widths.

## App-only tools

A landing page refuses these; call them only with an app's id.

- **Backend:** `connection_info` names the connected backend, the public `.env` values to write and the secret names. The backend tools run with credentials that stay on FluxNative. Each backend has one read tool (`supabase_read`, `cloudflare_read`, `api_read`, `firebase_read`, `appwrite_read`, `revenuecat_read`) and change tools (for Supabase, `supabase_sql` and its siblings). Read with the read tool; a change tool acts on live data no revision undoes, so confirm deletes and other destructive changes with the user first. A backend tool on an app without that backend is refused by name: ask the user to connect it with the project's Server button in the FluxNative builder. When a credential is missing, ask the user to add it there; never ask for its value. Before a Supabase app ships, use the `supabase-rls-audit` skill.
- **Android signing:** `signing_key read` returns the SHA-1 and SHA-256 fingerprints to register with Firebase, Google Sign-In or Maps. `signing_key generate` creates the key when the app has none. A keystore the user already has is uploaded in the builder, never here, and you never create a local keystore.
- **Design references and recipes:** `search_flows` and `search_screens`, by journey and by screen, each result naming its app (see the `design-references` skill), and `search_services` for maintained implementation recipes for a capability such as auth, payments, push or storage. Read the recipe before wiring a provider. Start a screen from an `exemplar_id` with `copy_template`.
- **Kernel:** `kernel_api` lists what the app's kernel exports, props included (`module` narrows it, e.g. `theme`).
- **Skills:** `load_skill` reads FluxNative's design and release skills; read their tool words as this connection's instructions map them.

Not available over MCP: native builds, store uploads, review, release and ZIP export. The user starts those in the FluxNative builder (Build and Publish). Never claim one happened. No image renderer either: make an icon's PNG on this machine and `upload` it.

## Never

- Never edit a FluxNative project with local file, shell or git tools, or edit a local copy and paste it back.
- Never mix up projects: pass the id of the project the user means on every call.
- Never put a secret in source or `.env`: every `EXPO_PUBLIC_*` value ships inside the app.
- Never ask for a key or a password in chat.
- Never retry a conflict without reading the file again, or overwrite paths held by `agent_editing`.

## Report

- The project (name and id) and the files you changed, one line each.
- The checks and preview steps you ran with their results, and what you could not verify.
- Anything the user must do in FluxNative: connect a backend or add a credential with the Server button, build, or publish.
