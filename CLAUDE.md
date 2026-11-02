# CLAUDE.md — Bar Tab

Before doing any work in this repository, read these files in order:

1. `HANDOFF.md`: current status, unresolved issues and the next step.
2. `PROJECT_MEMORY.md`: what the app is, saved-data keys, key decisions, limitations.
3. `AGENTS.md`: the working rules every coding agent follows here.

Then confirm with the owner what they want done before changing code. If `HANDOFF.md` says a stage
is waiting for approval, do not start the next stage.

## Quick facts
- App: a single file, `index.html` (inline CSS, JS and SVG). No build step, no dependencies.
- Master copy: this repo, branch `main`. The Claude artifact is rebuilt from `index.html`.
- Data: browser `localStorage` only. Keys are listed in `PROJECT_MEMORY.md`; never rename them.
- Test over a local HTTP server (for example `python3 -m http.server 8765`), not `file://`.

## When you finish a piece of work
- Update `HANDOFF.md` (status, tests run, open issues, next step).
- Add any new lasting decision or limitation to `PROJECT_MEMORY.md`.
- Never put passwords, tokens or private customer information in these notes.
