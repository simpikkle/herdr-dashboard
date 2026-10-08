# herdr-dashboard

A task board for [Herdr](https://herdr.dev): your GitHub issues, Jira tickets and Claude Code sessions,
from `created` to `merged`, with the tabs working on each one.

```
              created  research    code     testing     pr      merged
    PROJ-142     ●━━━━━━━━━●━━━━━━━━━●━━━━━━━━━●━━━━━━━━━●─────────○  Retry failed uploads · 2h · PR #318 · CI failing
                 ┆         ┆         ┆         ┆         ┆         ┆  ↳ ◐ upload retry backoff
    api#57       ●━━━━━━━━━●━━━━━━━━━●─────────○─────────○─────────○  Rate limit headers · 40m
    PROJ-150     ●─────────○─────────○─────────○─────────○─────────○  Dark mode toggle · Oct 7 (Wed) · ▶ start
```

## Install

Needs Herdr 0.7+ and Python 3.9+. [`gh`](https://cli.github.com) for PR status and GitHub issues, [`jira`](https://github.com/ankitpokhrel/jira-cli) for Jira issues.

```bash
herdr plugin install simpikkle/herdr-dashboard
herdr plugin action invoke simpikkle.dashboard.setup   # asks a few questions, puts `dashboard` on PATH, adds the Claude Code hooks
```

## Connect

Setup asks which tracker to sync issues from (GitHub, Jira or none), which issues, and the folder where
`▶ start` opens new work. Run it again to change your answers.

- **GitHub**: `brew install gh && gh auth login`. Also needed to show PR status.
- **Jira**: `brew install jira-cli && jira init` (Cloud, your Jira URL, your email).
  It needs an [API token](https://id.atlassian.com/manage-profile/security/api-tokens) in `JIRA_API_TOKEN`.

## Use

Open the board with `herdr plugin action invoke simpikkle.dashboard.board-popup` (or bind it to a key; setup shows how).
Click a task to jump to its tab, a ticket ID or `PR #…` to open it, `▶ start` to start work on it.

Claude Code sessions in Herdr show up on their own and keep their stage up to date.

See [USAGE.md](USAGE.md) for the CLI, settings and details.
