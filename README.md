# Task Board

A [Herdr](https://herdr.dev) plugin that tracks GitHub issues through
`created → research → code → testing → released`, grouped by project.

```
           created    research      code       testing    released
              ┆           ┆           ┆           ┆           ┆
herdr         ┆           ┆           ┆           ┆           ┆
              ┆           ┆           ┆           ┆           ┆
    #2        ●───────────●───────────●───────────●───────────○  pi shows momentarily as codex · testing 10m
```

## Install

Needs Herdr 0.7+, Python 3.9+ and an authenticated `gh`.

```bash
git clone git@github.com:simpikkle/herdr-custom.git && cd herdr-custom
herdr plugin link "$PWD"
ln -s "$PWD/tasks" ~/.local/bin/tasks   # any directory on your PATH
```

## Use

```bash
tasks start 42                 # split a pane, start an agent on issue #42 of the current repo
tasks set 42 released          # move an issue to a stage yourself
tasks add owner/repo#42        # track an issue without starting an agent
herdr plugin action invoke open-board   # open the board in its own tab (q closes it)
```

`tasks start` tells the agent to report its stage with `tasks set`; each change is
timestamped and shown in Herdr's sidebar. Agents never set `released`.

Issues can be written as `42`, `#42` or `owner/repo#42`. `--kind` picks the agent
(default `claude`). State lives in `~/.local/state/herdr-tasks/tasks.json`
(override with `TASKS_FILE`).
