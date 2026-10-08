# Task Board

A [Herdr](https://herdr.dev) plugin that tracks GitHub issues, Jira tickets and untitled work through
`created → research → code → testing → pr → merged`, grouped by project.

```
           created    research      code       testing    released
              ┆           ┆           ┆           ┆           ┆
herdr         ┆           ┆           ┆           ┆           ┆
              ┆           ┆           ┆           ┆           ┆
    #2        ●───────────●───────────●───────────●───────────○  pi shows momentarily as codex · testing 10m
```

## Install

Needs Herdr 0.7+, Python 3.9+, an authenticated `gh`, and the `jira` CLI for Jira tickets.

```bash
git clone git@github.com:simpikkle/herdr-custom.git && cd herdr-custom
herdr plugin link "$PWD"
ln -s## Use

```bash
tasks set code PROJ-123              # move a task to a stage; links the calling pane to it
tasks set testing                    # update the task linked to this pane
tasks set code --title "Fix login"   # task without a ticket
tasks set pr --pr <url> --project Auth
tasks set merged owner/repo#42 --detached --note "deployed"   # from another pane, without linking it
tasks start 42                       # split a pane, start an agent on issue #42 of the current repo
tasks add PROJ-123                   # track a ticket without starting an agent
tasks list [--json]                  # e.g. for an agent that follows PRs
tasks rename task-7 "Fix login"
tasks rm task-7
herdr plugin action invoke board-popup  # board as a popup (esc closes it; jumping to a task closes it too)
herdr plugin action invoke open-board   # board in its own tab (q closes it)
herdr plugin action invoke enroll       # popup: link the focused pane to a ticket
```

Refs can be `42`, `#42`, `owner/repo#42`, an issue URL, a Jira key or URL, or `task-7`
(tasks without a ticket). Linking a pane's untitled task to a ticket folds it into the ticket.
Tasks are grouped by `--project`; tasks without one go under "No project".
State lives in `~/.local/state/herdr-tasks/tasks.json` (override with `TASKS_FILE`).

## Automatic tracking with Claude Code

Add to `~/.claude/settings.json`:

```json
"hooks": {
  "SessionStart": [{"matcher": "*", "hooks": [{"type": "command", "command": "python3 ~/.local/bin/tasks hook session"}]}],
  "UserPromptSubmit": [{"hooks": [{"type": "command", "command": "python3 ~/.local/bin/tasks hook prompt"}]}]
}
```

Inside Herdr, the first prompt of each Claude session creates an untitled task for its pane
(shown on the board under Claude's session title until it gets a ticket or a title),
and Claude is told to link it to the ticket, infer a project and report its stage, up to `pr`.
It never sets `merged`.

To bind keys, add to Herdr's `config.toml`:

```toml
[[keys.command]]
key = "prefix+d"
type = "plugin_action"
command = "simpikkle.tasks.board-popup"
description = "task board"

[[keys.command]]
key = "prefix+t"
type = "plugin_action"
command = "simpikkle.tasks.enroll"
description = "enroll pane in task board"
```

On the board, `↑`/`↓` or `j`/`k` select a task, `Enter` or a click jumps to its tab, `o` or a click on
`PR #…` opens the PR.
