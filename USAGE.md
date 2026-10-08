# Usage

## Board

Open it as a popup (`simpikkle.dashboard.board-popup`) or in its own tab (`simpikkle.dashboard.open-board`).

- `↑`/`↓` or `j`/`k` select, `Enter` or a click jumps to the task's tab
- click a ticket ID to open the ticket, `PR #…` (or `o`) to open the PR
- click `▶ start` on a ticket that hasn't started to run `dashboard start --workspace` for it
- `s` syncs issues and PR status (also runs every 5 minutes while the board is open)
- `q` closes it (`Esc` too, in the popup)

Tasks are ordered by stage, furthest along first. Tabs working on a ticket are listed under it:
◐ working, ◉ needs you, ✓ done, ○ idle. Tasks merged in the last 2 weeks are listed under "Done".

Bind a key in Herdr's `config.toml`:

```toml
[[keys.command]]
key = "prefix+d"
type = "plugin_action"
command = "simpikkle.dashboard.board-popup"
description = "dashboard"
```

## Claude Code

`dashboard setup` adds two hooks to `~/.claude/settings.json`:

- `SessionStart` tells Claude how to report its stage and lists the open tickets it can link to
- `UserPromptSubmit` creates an untitled task for each new session's pane

Claude links the task to its ticket once it knows it, and moves it forward up to `pr`.
It never sets `merged`, and stages only move back with `--force`, which Claude must ask you about.

## CLI

Refs: `42`, `#42`, `owner/repo#42`, an issue URL, a Jira key or URL, or `task-7` (no ticket).

```bash
dashboard set code PROJ-123              # move a task to a stage; links this pane to it
dashboard set testing                    # update the task linked to this pane
dashboard set pr --pr <url>              # record the PR
dashboard set code --title "Fix login"   # task without a ticket
dashboard set code PROJ-123 --force      # move back to an earlier stage
dashboard set merged PROJ-123 --detached # update without linking this pane
dashboard start PROJ-123 --workspace     # new workspace with a "research" tab and a Claude prompt
dashboard start 42                       # split this pane and start an agent on it
dashboard add PROJ-123                   # track without starting
dashboard sync [--query Q]               # add issues from your tracker, refresh PR status, mark merged PRs
dashboard setup                          # change tracker, query or start folder
dashboard list [--json]
dashboard rename task-7 "Fix login"
dashboard rm task-7
```

`simpikkle.dashboard.enroll` opens a popup to link the focused pane to a ticket.

## Settings

`dashboard setup` writes these to `~/.config/herdr-dashboard/config`; environment variables override them:

| Variable | Default |
| --- | --- |
| `DASHBOARD_START_DIR` — where `start --workspace` opens and Claude looks for the repo | `~` |
| `DASHBOARD_ISSUES` — tracker `dashboard sync` adds issues from: `github`, `jira` or `none` | `none` |
| `DASHBOARD_GITHUB_QUERY` — GitHub issue search | `assignee:@me state:open` |
| `DASHBOARD_JIRA_JQL` — Jira issues | `assignee = currentUser() AND statusCategory != Done AND issuetype != Epic` |
| `DASHBOARD_FILE` — state file | `~/.local/state/herdr-dashboard/tasks.json` |
