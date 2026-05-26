# skills

A personal set of [Claude Code](https://claude.com/claude-code) skills for an agentic development workflow: stress-test a plan, turn it into a PRD and issues, then have an agent (or you) build each slice end-to-end.

## The pipeline

```
day 0 (existing repo):  map-context   →   improve-codebase-architecture
                        (seed CONTEXT.md / ADRs)   (find deepening opportunities)

per feature:  grill-me / grill-with-docs  →  to-prd   →  to-issues   →  do-work
                 (stress-test the plan)       (PRD .md)   (issue .md's)  (build a slice)
```

Each skill is one focused step with a human review gate between them — there is no "do it all" button, on purpose. Skills end with a **Next** pointer suggesting the likely follow-up.

Supporting skills:

- **map-context** — bootstrap a repo's domain docs from existing code. Survey mode proposes the context structure; deepen mode extracts a curated glossary one section at a time (run it per-section, not all at once).
- **prototype** — throwaway code to answer a design question before committing (logic via a terminal app, or several UI variations on one route). Usually pulled in *during* planning.
- **improve-codebase-architecture** — find "deepening" opportunities (shallow → deep modules) and report them, informed by the project's domain language and ADRs.
- **handoff** — compact the current conversation into a doc so a fresh agent can pick up. An interrupt you fire whenever context fills up, not a pipeline phase.
- **write-a-skill** — meta-skill for authoring new skills in the house style.

`ralph/` is the legacy agentic-execution experiment, now superseded — see [Agentic execution](#agentic-execution-later) and [`ralph/README.md`](ralph/README.md).

## Conventions

- **No external issue tracker.** PRDs are written to `docs/prd/<NNNN>-<slug>.md` and issues to `docs/issues/<NNNN>-<slug>.md`, sequentially numbered, in the target repo. Created lazily.
- **AFK vs HITL.** Every issue is tagged. **AFK** issues can be implemented, validated, and merged by an agent unattended. **HITL** issues need a human — an architectural call, a design review, or anything risky enough to make or heavily review yourself.
- **Domain docs are optional but encouraged.** If a repo keeps a `CONTEXT.md` glossary and ADRs under `docs/adr/`, the skills use that vocabulary and respect those decisions. `grill-with-docs` and `improve-codebase-architecture` build these up over time; nothing breaks if they don't exist yet.
- **No hardcoded package manager.** Skills detect the project's actual task runner (pnpm/npm/yarn/bun/cargo/go/pytest/…) rather than assuming one.

## Install

These are user-level skills. Symlink each skill folder into `~/.claude/skills/` (or copy them) so Claude Code loads them in every session:

```sh
for d in map-context grill-me grill-with-docs to-prd to-issues do-work prototype improve-codebase-architecture handoff write-a-skill; do
  ln -sfn "$PWD/$d" "$HOME/.claude/skills/$d"
done
```

Then invoke a skill in any session, e.g. `/do-work` or `/to-prd`.

## Agentic execution (later)

Once `to-issues` has produced a queue of **AFK** issues, you can have an agent drain them instead of running `/do-work` by hand. This is **deferred on purpose** — the AFK label is the human gate (applied at issue-creation time), so hand-run `/do-work` for the first weeks to build trust, then graduate.

Key reframe: **the runner is a driver, not a skill** — it just calls `/do-work` — so it costs nothing against the 10-skill cap, and **Docker is no longer needed** (Claude Code's built-in sandboxing / permission modes replace the old container).

| When you want it | Tool | Oversight |
|---|---|---|
| First few weeks | hand-run `/do-work` per issue | full — this *is* the HITL default |
| Supervised auto | `/loop /do-work` (built-in `/loop` skill, self-paced) | you can interrupt anytime |
| Walk-away / overnight | headless `claude -p` shell loop, targeting `docs/issues/` | low |
| Scheduled / cadence | `/schedule` a routine (built-in) | none |

When you build the headless loop, the salvageable kernel lives in `ralph/` — the task-selection priority and the loop-until-done sentinel. See [`ralph/README.md`](ralph/README.md) for what to keep and what to change.
