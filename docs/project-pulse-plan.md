# Project Pulse implementation plan

## Goal

Build Mona's lightweight, polished static dashboard so contributors can quickly see active projects, owners, status, recent activity, priority or risk, and short summaries. The page must open as a dashboard in the browser, not as a directory listing.

## Responsibilities and file ownership

| Owner | Assignment |
| --- | --- |
| **Orchestrator** | Keep scope to the files below, agree the data/markup contract before implementation, sequence dependent work, integrate the results, and run the validation checklist. |
| **Planner** | This plan: define phases, assignments, dependencies, edge cases, and validation expectations. |
| **Designer** | Own `app/styles.css`: define and implement the responsive visual system, hierarchy, readable spacing, accessible colors/focus treatments, project cards, status badges, and priority treatment. Coordinate the class/attribute hooks needed in the HTML with Coder before parallel implementation. |
| **Coder** | Own `app/index.html`, `app/project-data.json`, and `.vscode/launch.json`. Implement accessible semantic markup and deterministic rendering from the data, provide representative project records, and configure a runnable browser preview. |

## Ordered implementation phases

1. **Agree on the contract (Orchestrator, Designer, Coder).** Confirm that JSON has a top-level `projects` array and each record has `name`, `owner`, `status`, `recentActivity`, and `priority`; agree the HTML hooks and status/priority values that CSS will style. Keep the requested feature set static and small; avoid adding a framework or build system.
2. **Create the foundation (Coder; sequential prerequisite).** Create `app/project-data.json` with several realistic, distinct projects and create `app/index.html` with the exact page title “Project Pulse”, a clear dashboard heading, accessible project-card container, references to `styles.css` and `project-data.json`, and a small script that loads and renders the records. Include status, priority, owner, activity, and contributor-friendly summary in each card.
3. **Build the experience (Designer and Coder; parallel after phase 1).** Designer creates `app/styles.css` using stable `.dashboard` and `.project-card` hooks, polished cards with rounded corners and subtle shadow, clear status/priority distinctions, and responsive layouts. Coder completes the HTML rendering and data in the agreed structure. Check in with each other if implementation reveals a contract mismatch; do not silently invent competing selectors or field names.
4. **Make it runnable (Coder; can proceed alongside phase 3 once the preview method is chosen).** Create `.vscode/launch.json` with a configuration named **Run Project Pulse Dashboard**, strict JSON, a deterministic local preview command/URL, `cwd` set to `${workspaceFolder}/app`, and a URL/path that opens `index.html` directly. Use an available static-server/debugger approach consistent with the repository environment; do not target the workspace root and expose a directory listing.
5. **Integrate and validate (Orchestrator, with Designer and Coder fixing their owned files).** Run the checks below, preview the dashboard through the named launch configuration, resolve integration defects, and report any remaining environment limitation.

## File assignments and dependencies

- `app/project-data.json` — **Coder**. Source of the dashboard records. It must be valid JSON with a top-level `projects` array and the five required fields per record. HTML rendering depends on this agreed schema and the data being available.
- `app/index.html` — **Coder**. Page shell, semantic/accessibility structure, stylesheet and JSON references, and rendering behavior. It depends on the agreed CSS hooks and JSON schema; use DOM APIs/text content rather than interpreting project values as markup.
- `app/styles.css` — **Designer**. Visual and responsive treatment, including `.dashboard`, `.project-card`, rounded corners, and box shadows. It depends on the agreed markup hooks; the page can be built against that contract in parallel.
- `.vscode/launch.json` — **Coder**. Runnable browser preview named **Run Project Pulse Dashboard**. It depends on choosing a preview server/debugger command and URL, but not on the finished visual styling. Set its working directory to `${workspaceFolder}/app` and open `index.html`.

## Parallel work vs. sequencing

- The **schema and markup/CSS contract must be agreed first**. This is the handoff boundary that prevents Designer and Coder from producing incompatible class names or data fields.
- After that contract, Designer can implement `app/styles.css` in parallel with Coder implementing `app/index.html` and `app/project-data.json`; the Coder should use agreed hooks and representative fields while the Designer styles those hooks.
- Coder can prepare `.vscode/launch.json` in parallel with styling once the preview method and URL are selected. Final launch verification is sequential after the page and static server are available.
- Integration and end-to-end validation must follow completion of all four assigned files. Defect fixes can then run in parallel only when they concern disjoint owned files; changes to shared contracts require coordination and a subsequent re-test.

## Edge cases and risks

- Empty or missing `projects` array: show a useful empty state instead of a blank region or runtime error.
- Invalid/unavailable JSON or a failed fetch: show a readable error/recovery message; do not leave a misleading empty dashboard. Serve the app over HTTP if browser `file://` fetch restrictions apply.
- Missing, blank, or unexpected field values: render safe fallbacks; handle unknown status/priority values with neutral styling rather than broken labels or inaccessible color-only meaning.
- User/data text containing HTML characters: render as text, never inject it as HTML.
- Long project names, summaries, owner names, and activity text; narrow viewports; zoom; keyboard-only navigation; and reduced motion should not cause clipping or lost content.
- Ensure status and priority remain understandable without color alone, with sufficient contrast and visible focus. Do not rely on color as the only signal.
- A launch configuration that serves the workspace root or omits the explicit `index.html` can open a directory listing rather than Project Pulse. Keep `cwd` and URL/path aligned.
- A launch command may depend on a browser/debugger extension or server tool not installed in every Codespace. Choose and document a deterministic available option, and report if the local environment cannot run it.

## Validation expectations

1. Confirm the expected files exist: `app/index.html`, `app/styles.css`, `app/project-data.json`, and `.vscode/launch.json`.
2. Check `app/project-data.json` parses as JSON; verify `projects` is an array and representative entries provide `name`, `owner`, `status`, `recentActivity`, and `priority`.
3. Inspect the HTML for the exact “Project Pulse” title, stylesheet and JSON references, accessible page/card structure, and safe data rendering. Verify a clear empty/error state is present.
4. Inspect the CSS for `.dashboard`, `.project-card`, `border-radius`, and `box-shadow`; verify responsive behavior, readable contrast, status/priority treatments, and keyboard focus styling.
5. Parse `.vscode/launch.json` as strict JSON and verify it contains **Run Project Pulse Dashboard**, uses `${workspaceFolder}/app` as `cwd`, and opens `index.html` rather than a directory.
6. Run the repository's relevant exercise checks where available, including the Step 3 checks for page/data/style/launch references and required field names.
7. Launch **Run Project Pulse Dashboard** and confirm the browser opens the actual page, cards show all required information, JSON loading succeeds, and the layout remains usable at narrow and desktop widths. Exercise empty/error and unknown-value behavior where practical.

## Open question

Which browser-preview server/debugger command is available in the target Codespace? Check the installed environment when implementing `.vscode/launch.json`; keep the configuration deterministic and preserve the required `cwd` and direct `index.html` URL regardless of the selected supported method.
