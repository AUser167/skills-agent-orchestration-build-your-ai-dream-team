# Project Pulse dashboard implementation plan

## Summary

Build a small, dependency-free static dashboard for Mona's Project Pulse. It should show project name, owner, status, recent activity, priority, and a short contributor-friendly summary in responsive, accessible project cards.

The repository has no app implementation, package manifest, frontend framework, or app-specific test suite. Follow the existing brief and agent instructions: use the three `app/` files, create `.vscode/launch.json`, and avoid introducing build tools or dependencies.

## Ordered implementation steps

### 1. Agree on the data and markup contract

The Orchestrator confirms the brief's data shape and shared UI hooks before implementation: a top-level `projects` array with `name`, `owner`, `status`, `recentActivity`, and `priority`; a `.dashboard` container; and `.project-card` for each card. Agree on shared status and priority hooks so the HTML and CSS work can proceed without conflicting assumptions.

**Assignments:** Orchestrator coordinates; Designer advises on hierarchy, readability, responsive behavior, and accessibility. No files need to change in this step.

**Dependency:** Complete this contract before parallel implementation so the stylesheet and markup agree.

### 2. Implement independent files in parallel

Once the contract is fixed, these tasks can proceed in parallel because their file ownership is separate:

- **Designer — `app/styles.css`:** Style the agreed hooks, including `.dashboard` and `.project-card`. Provide responsive layout, readable spacing and contrast, visible status and priority treatments, and clear keyboard focus. Include the expected rounded-card and shadow styling without relying on external assets or libraries.
- **Coder — `app/project-data.json`:** Add a valid top-level `projects` array with representative, contributor-friendly records. Every record must include `name`, `owner`, `status`, `recentActivity`, and `priority`.
- **Coder — `.vscode/launch.json`:** Add strict JSON for a launch configuration named `Run Project Pulse Dashboard`. Serve from `${workspaceFolder}/app` with `python3 -m http.server 5500` and open `http://localhost:%s/index.html` through `serverReadyAction`. Keep the configuration deterministic and do not add extension or tooling dependencies.

### 3. Build the page and integrate the files

**Coder — `app/index.html`:** Create the semantic page titled **Project Pulse**, link `styles.css`, and load `project-data.json`. Render project cards from the `projects` data using `.project-card`; show each project's name, owner, status, recent activity, and priority. Keep any necessary rendering logic in the page rather than adding an unassigned JavaScript file or dependency. Include understandable loading, empty-data, and data-load-error states.

**Dependency:** The page uses the agreed data and CSS contracts from step 1 and integrates the files from step 2. It can be authored in parallel with step 2 only if those contracts remain stable; otherwise, do this step after step 2.

### 4. Review the integrated dashboard

The Orchestrator reviews the four assigned files together. Designer checks the visual hierarchy, responsiveness, contrast, and focus states; Coder checks data rendering, error states, and launch behavior. Resolve any markup/CSS contract or launch-path mismatch before handoff.

**Dependency:** This review must follow integration.

## Roles and file ownership

| Agent | Responsibility | Files |
| --- | --- | --- |
| Designer | Define and apply the responsive, accessible visual system and component styling. Coordinate CSS hooks with the page contract. | `app/styles.css` |
| Coder | Implement semantic markup and data rendering; provide representative data; configure the runnable preview; validate the integrated behavior. | `app/index.html`, `app/project-data.json`, `.vscode/launch.json` |
| Orchestrator | Set the shared contract, delegate non-overlapping scopes, resolve integration issues, and review the handoff. | Coordinates; no implementation-file ownership |

## Dependencies and parallel work

- The markup/data/CSS contract must be agreed first; otherwise independently written files can disagree on selectors or field names.
- After that contract, Designer can work on `app/styles.css` while Coder works on `app/project-data.json` and `.vscode/launch.json`. These assignments do not overlap.
- `app/index.html` can be implemented alongside those tasks only if its agreed field names and CSS hooks are stable. Otherwise, implement it after the stylesheet and data shape are settled.
- Integrated review and launch verification must happen after all four files are present.
- Keep the work limited to the requested dashboard. The existing repository does not provide an app build system or frontend test framework.

## Edge cases and risks

- Handle a missing, invalid, or empty project list visibly; do not leave a blank dashboard or silently hide data-load failures.
- Use semantic headings and labels, maintain keyboard focus visibility, and do not communicate status or priority by color alone.
- Keep the data file valid JSON and ensure its property names match the page's rendering logic exactly.
- Ensure the server runs from `app/` and opens `index.html`, rather than displaying a directory listing.
- Port 5500 may already be occupied; surface that launch failure and stop any preview server after manual checks.
- Avoid assuming a Python or browser-debug extension is installed. Use the repository's existing VS Code setup and do not add tooling without need.

## Validation expectations

Run the repository's existing checks:

```bash
python3 -m json.tool app/project-data.json >/dev/null
python3 -m json.tool .vscode/launch.json >/dev/null
bash scripts/validate-exercise.sh
```

Then verify behavior:

1. Start a manual preview with `python3 -m http.server 5500 --directory app` and check `http://localhost:5500/index.html`; confirm the dashboard, not a directory listing, is served. Stop the server when finished.
2. Open **Run Project Pulse Dashboard** in VS Code. Confirm it starts the server from `app/` and opens `index.html` at the expected URL; stop the preview afterward.
3. In the browser, confirm cards render from the JSON and expose every required field. Check the empty/error states, responsive layout, keyboard focus, and that status/priority remain understandable without color.

## Open questions

None for the initial implementation: the brief specifies the required fields, files, launch name, server command, and URL. The team should settle any additional CSS hooks during step 1 and keep them consistent across Designer and Coder work.
