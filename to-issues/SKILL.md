---
name: to-issues
description: Break a plan, spec, or PRD into independently-grabbable issues saved as local markdown files under docs/issues/, using tracer-bullet vertical slices. Use when user wants to convert a plan into issues, create implementation tickets, or break down work into issues.
---

# To Issues

Break a plan into independently-grabbable issues using vertical slices (tracer bullets). Each issue is a self-contained markdown file an agent (or you) can pick up and work in isolation.

## Process

### 1. Gather context

Work from whatever is already in the conversation context. If the user passes a reference as an argument (a PRD path like `docs/prd/0003-foo.md`, an existing issue file, or a plan), read its full body first.

### 2. Explore the codebase (optional)

If you have not already explored the codebase, do so to understand the current state of the code. If a `CONTEXT.md` glossary exists, use its vocabulary in issue titles and descriptions, and respect ADRs under `docs/adr/` in the area you're touching. If neither exists yet, don't block — use plain, precise language.

### 3. Draft vertical slices

Break the plan into **tracer bullet** issues. Each issue is a thin vertical slice that cuts through ALL integration layers end-to-end, NOT a horizontal slice of one layer.

Slices are either **AFK** or **HITL**:

- **AFK** — can be implemented, validated, and merged by an agent without human interaction.
- **HITL** — requires a human: an architectural decision, a design review, a judgement call, or a change risky enough that you'd want to make or heavily review it yourself.

Prefer AFK where possible, but mark a slice HITL whenever a human genuinely needs to be in the loop — don't hand an agent a decision it shouldn't be making alone.

<vertical-slice-rules>
- Each slice delivers a narrow but COMPLETE path through every layer (schema, API, UI, tests)
- A completed slice is demoable or verifiable on its own
- Prefer many thin slices over few thick ones
</vertical-slice-rules>

### 4. Quiz the user

Present the proposed breakdown as a numbered list. For each slice, show:

- **Title**: short descriptive name
- **Type**: HITL / AFK
- **Blocked by**: which other slices (if any) must complete first
- **User stories covered**: which user stories this addresses (if the source material has them)

Ask the user:

- Does the granularity feel right? (too coarse / too fine)
- Are the dependency relationships correct?
- Should any slices be merged or split further?
- Are the correct slices marked as HITL and AFK?

Iterate until the user approves the breakdown.

### 5. Write the issue files

For each approved slice, write a markdown file to `docs/issues/<NNNN>-<slug>.md` (zero-padded sequential number; scan the directory for the highest existing one and increment; create `docs/issues/` lazily if it doesn't exist). Use the issue body template below.

Write files in dependency order (blockers first) so you can reference real filenames in the "Blocked by" field. Record the **Type** (AFK/HITL) in the file so whoever picks it up knows whether an agent can run it unattended.

<issue-template>
## Type

AFK or HITL.

## Parent

A relative path to the source PRD or parent issue file (e.g. `../prd/0003-foo.md`). Omit this section if there's no parent.

## What to build

A concise description of this vertical slice. Describe the end-to-end behavior, not layer-by-layer implementation.

Avoid specific file paths or code snippets — they go stale fast. Exception: if a prototype produced a snippet that encodes a decision more precisely than prose can (state machine, reducer, schema, type shape), inline it here and note briefly that it came from a prototype. Trim to the decision-rich parts — not a working demo, just the important bits.

## Acceptance criteria

- [ ] Criterion 1
- [ ] Criterion 2
- [ ] Criterion 3

## Blocked by

- A relative path to the blocking issue file (if any)

Or "None - can start immediately" if no blockers.

</issue-template>

Do NOT modify the source PRD or any parent issue file.

## Next

→ `/do-work` on the first AFK slice with no blockers.
