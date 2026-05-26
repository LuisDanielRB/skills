# ralph (legacy)

The original agentic-execution experiment: loop an agent over open issues, pick the next one, build it, commit, repeat until done. **Superseded — don't run this as-is.** It's kept only as a salvage seed for when we build the real runner.

See the root [README → Agentic execution](../README.md#agentic-execution-later) for the forward plan and the tool options (`/do-work`, `/loop`, headless `claude -p`, `/schedule`).

## Why it's stale

- **Docker is no longer needed.** `afk.sh` (now deleted) wrapped the loop in `docker sandbox run` only to skip permission prompts safely during unattended runs. Claude Code's built-in sandboxing / permission modes cover that now.
- **GitHub-issue-based.** `once.sh` and `prompt.md` read from `gh issue list` and close/comment GitHub issues. We've since moved issues to local markdown under `docs/issues/`.

## What's still worth keeping

- **`prompt.md`** — the task-selection priority is sound: critical bugfix → dev infrastructure → tracer bullet → polish → refactor. And the `<promise>NO MORE TASKS</promise>` sentinel is a clean loop-termination signal.
- **`once.sh`** — a Docker-free single-shot invocation (`claude --permission-mode acceptEdits`). Useful shape for the future driver.

## What to change when we revisit

1. Retarget from `gh issue list` to reading `docs/issues/*.md` (pick the next **AFK** issue with no unmet `Blocked by`).
2. Drop the GitHub close/comment steps; mark the issue done in its markdown file instead.
3. Have the loop invoke `/do-work` rather than re-implementing the build/validate/commit steps inline.
4. Pick a driver from the root README table — start with `/loop`, reach for a headless `claude -p` loop only for walk-away runs.
