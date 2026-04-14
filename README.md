# Sprint Planning Assistant — Sport-Soccer R&D

An interactive dashboard for the WSC Sports **Sport-Soccer** R&D squad that pulls together past sprint performance, the current sprint's health, and a view into upcoming work (Q2 goals + backlog).

**Live dashboard:** https://<your-github-username>.github.io/sprint-planning-assistant/

*(Update the link above after enabling GitHub Pages.)*

---

## What's in it

Three tabs, all filtered to `Squad[Dropdown] = "Sports- Soccer"` in Jira:

- **Past Sprints** — velocity, completion rate, bug count, effort distribution, and per-person contribution across the last 4 closed sprints (S4–S7).
- **Current Sprint** — real-time progress on S8, projected completion, open bugs sorted by priority, and the full issue list grouped by R&D Lead.
- **Future Sprints** — Q2 goals split by P0 / P1+, status breakdown, insights, and a sortable backlog with an "add to sprint" checkbox so you can plan the next sprint directly from the dashboard.

Global filters at the top (R&D Lead, issue type) apply to tables and charts across all tabs.

## How the data stays fresh

The dashboard is a single self-contained HTML file (`index.html`) with the Jira data embedded as a JSON blob. The data is refreshed **every hour** by a Cowork scheduled task running on Yuval's machine — it queries Jira, rebuilds the JSON, commits the updated file, and pushes to this repo. GitHub Pages then serves the new version automatically.

If you open the dashboard and the **Generated** timestamp in the header is more than a couple of hours old, the refresh job probably failed — ping Yuval.

## How to use it

Just open the [live dashboard](https://<your-github-username>.github.io/sprint-planning-assistant/) in any browser. No login, no install. Bookmark it.

If you want to run it locally instead (e.g. offline), clone the repo and open `index.html` directly:

```bash
git clone https://github.com/<your-github-username>/sprint-planning-assistant.git
cd sprint-planning-assistant
open index.html        # macOS
# or just double-click the file
```

## Requesting changes

Open a GitHub issue or message Yuval on Slack. Common requests:

- Add a new chart or metric
- Change the squad / filter
- Add a teammate to the R&D Lead list (this usually auto-picks up once they're assigned work in Jira)
- Fix a bug in the dashboard itself

## For contributors — how the refresh works

The refresh pipeline is documented for transparency, but you don't need to run it yourself:

1. A Cowork scheduled task (`sprint-dashboard-hourly-refresh`) fires on the hour.
2. It queries Jira via the Atlassian MCP for past sprints (2582–2585), current sprint (2586), Q2 goals, and the backlog.
3. Results are parsed, the `const DATA = {...}` block inside `index.html` is replaced, and the `Generated` timestamp is updated.
4. `git add`, `git commit`, `git push` to `main`.
5. GitHub Pages redeploys within ~1 minute.

Credentials (Jira API token) live only on Yuval's machine in a gitignored `.env` and are never committed.

## What's NOT tracked in this repo

See `.gitignore`. In short: any `.env` file, Jira token, scratch JSON results from the refresh pipeline, local Python virtualenvs, and editor/OS noise.

---

Maintainer: Yuval Ben Nissan · yuval.bennissan@wsc-sports.com
