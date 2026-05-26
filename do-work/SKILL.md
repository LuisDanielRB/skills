---
name: do-work
description: "Execute a unit of work end-to-end: plan, implement, validate with the project's typecheck and tests, then commit. Use when user wants to do work, build a feature, fix a bug, or implement a phase from a plan."
---

# Do Work

Execute a complete unit of work: plan it, build it, validate it, commit it.

## Workflow

### 1. Understand the task

Read any referenced plan, PRD, or issue file. Explore the codebase to understand the relevant files, patterns, and conventions. If the task is ambiguous, ask the user to clarify scope before proceeding.

While exploring, identify the project's **feedback loops** — the actual commands this repo uses to typecheck and test. Look at `package.json` scripts, `Makefile`, `justfile`, `pyproject.toml`, `Cargo.toml`, `go.mod`, CI config, or the README. Don't assume a package manager; use what the project actually uses (`pnpm`, `npm`, `yarn`, `bun`, `cargo`, `go test`, `pytest`, etc.). Note these commands for step 4.

### 2. Plan the implementation (optional)

If the task hasn't been planned yet and isn't trivial, stop and plan it — `/grill-me` to stress-test the approach, or `/grill-with-docs` if the project keeps a `CONTEXT.md` / ADRs. Skip this for small, well-understood changes.

### 3. Implement

Default to **red/green/refactor**, one test at a time, in a tracer-bullet style:

1. Write a single failing test for the smallest vertical slice of behavior
2. Run the test — confirm it fails (red)
3. Write the minimum code to make it pass (green)
4. Repeat from step 1 for the next slice of behavior
5. Refactor if needed while keeping tests green

Each test should target one thin vertical slice through the system. Do not write all tests upfront — write one, make it pass, then move to the next.

**Where TDD doesn't fit** — purely presentational or visual UI, throwaway glue, or exploratory work where the behavior isn't yet pinned down — implement directly and verify by running it. The test of "should this be TDD'd?" is whether there's logic worth locking in, not which layer it lives in.

### 4. Validate

Run the feedback loops you identified in step 1 and fix any issues. Repeat until they pass cleanly. For example, in a pnpm project:

```
pnpm run typecheck
pnpm run test
```

Substitute the commands this project actually uses.

### 5. Commit

Once typecheck and tests pass, commit the work.

## Next

→ The next unblocked issue, or `/handoff` if context is filling up.
