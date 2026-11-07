# FluxNative for Claude

Build and edit the native iOS and Android apps, and the landing pages, you make in [FluxNative](https://fluxnative.app), from Claude Code, claude.ai or Cowork.

## What it does

FluxNative builds Expo + React Native apps and static landing pages. This plugin connects Claude to your FluxNative account and teaches it how to work on those projects:

- **The FluxNative connector**, a remote MCP server at `https://mcp.fluxnative.app/mcp`. One connection reaches every app and landing page you can edit, in every workspace: Claude lists them, creates a blank one, and works on any of them by its id. It reads and changes the source, runs the checks, looks at and taps through a live preview, searches FluxNative's catalog of real app designs and its landing-page template library, and administers an app's connected backend.
- **Skills** that tell Claude how to do that work well:
  - `app-workflow`: find or create the project, read its contract, then read before writing, make safe edits, keep FluxNative's source structure and check the result.
  - `preview-verification`: build the preview, walk the screen, read console and network logs, and report only what was seen.
  - `design-references`: use FluxNative's catalog of real app journeys and screens, adapted to your app's own theme.
  - `supabase-rls-audit`: audit a connected Supabase project's row-level security, grants and storage buckets, and fix them with your consent.
- **Design skills** for any React Native or Expo app, from the open pack [fluxnative-app/fluxnative-skills](https://github.com/fluxnative-app/fluxnative-skills) (MIT):
  - `mobile-design-brief`: pick the app's look before the first screen and write it into the theme.
  - `mobile-app-design`: design and build screens from a brief, a mockup, a screenshot or a Figma frame.
  - `app-sample-content`: write believable sample data and copy, including the empty, loading and error states.
  - `mobile-deslop`: remove the generic AI-made look from screens that already exist.
  - `mobile-screen-review`: critique screens and rank what to fix, without editing them.
  - `paywall-onboarding`: build and check paywall, onboarding and permission screens against store rules.
- **Commands:**
  - `/fluxnative:apps [name]` lists your apps and landing pages.
  - `/fluxnative:connect <app>` picks one of them for Claude to work on.

Claude cannot build, upload or submit an app through this plugin. Native builds, store uploads, review and release stay in FluxNative, where you start them.

## Requirements

- A FluxNative account whose workspace plan includes remote MCP (Creator, Pro or Business; the Free plan does not).
- Claude Code, or a claude.ai plan that supports plugins and connectors.

## Install

**Claude Code**

```bash
claude plugin marketplace add fluxnative-app/fluxnative-claude
claude plugin install fluxnative@fluxnative
```

Start a session, run `/mcp`, select the FluxNative server and choose **Authenticate**. Your browser opens FluxNative's sign-in and consent page. Then try `/fluxnative:apps`.

**claude.ai and Cowork**

Add the plugin under **Customize › Plugins**, from the directory or with **Add marketplace** and this repository. Open the plugin's **Connectors** tab and click **Connect** to sign in to FluxNative.

## Authentication

The plugin's connector signs in with OAuth: your editor opens FluxNative's consent page once, and refreshes its access by itself. The plugin holds no key, token or password, and asks for none. Revoke its access at any time under **Settings › Editors**.

To connect Claude Code without a browser, create a connection key in the FluxNative builder under **Settings › Editors** and add the server the way that page shows, instead of installing this plugin. Use one or the other, never both, and never paste a key into a chat.

## How it works

The connector is one connection to your whole account. `list_apps` and `create_app` find and make projects; every other project tool takes the project's id, so Claude can move between your apps and landing pages without adding another server or signing in again. The design catalog searches and the landing-page template library take no project: they are the same for all of them.

On an app Claude has the source, check, preview, Android signing, backend and design catalog tools; on a landing page, the source, check, preview and template library tools. A backend tool on an app without that backend is refused by name, and you connect the backend with the project's Server button in the FluxNative builder. Every source change is saved as a revision you can see and undo in the FluxNative builder. Backend tools and taps in the live preview change your backend's live data (database, users, storage, functions), which no revision undoes: Claude Code asks before each of those calls.

## Usage

Ask in plain words and name the project:

- "Show my FluxNative apps."
- "Create a blank FluxNative app called Pocket Budget for tracking shared expenses."
- "In my Recipe Box app, add a favorites tab and check it in the preview."
- "Why is the login screen in Habit Tracker blank?"
- "Audit the Supabase rules of my Bookshelf app before I ship."
- "Tighten the hero copy on the Recipe Box landing page and check it on a phone."

## Data and privacy

- The plugin contains instructions only: no scripts, no hooks, no executables, and nothing that runs on your computer.
- Claude reaches FluxNative only through the connector at `https://mcp.fluxnative.app/mcp`, after you sign in with OAuth.
- What Claude sends to FluxNative: the tool calls it makes, which carry the source it writes, files you ask it to upload, search queries for the design catalog and the template library, and actions in the live preview. A tap in the preview runs the real app and can reach the app's connected backend.
- What comes back: your project's source, check results, preview screenshots, page text and logs, design references, and the public settings of a connected backend. Backend credentials, signing passwords and keystores stay on FluxNative's servers and are never returned.
- FluxNative records each tool call on a project in the workspace's activity log (who, which app, which tool, the outcome and how long it took), never its arguments or your conversation.
- Access follows your FluxNative permissions and is checked on every request. Revoke it at any time under **Settings › Editors**.
- Privacy policy: https://fluxnative.app/privacy. Terms: https://fluxnative.app/terms.

## Support

Documentation: https://fluxnative.app/docs/remote-mcp. Help: support@fluxnative.app, or https://fluxnative.app/legal/support.

## License

MIT. See [LICENSE](LICENSE).
