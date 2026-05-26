---
name: to-prd
description: Turn the current conversation context into a PRD saved as a local markdown file under docs/prd/. Use when user wants to create a PRD from the current context.
---

# To PRD

Take the current conversation context and codebase understanding and produce a PRD. Do NOT run a fresh interview — synthesize what you already know. (If the plan still has open questions, that's a sign to run `/grill-me` or `/grill-with-docs` first, not to interview here.)

## Process

1. Explore the repo to understand the current state of the codebase, if you haven't already.

   If a `CONTEXT.md` glossary exists, use its vocabulary throughout the PRD, and respect any ADRs under `docs/adr/` in the area you're touching. If neither exists yet, don't block — write in plain language, and note any term that's begging to be canonicalised so it can be captured later.

2. Sketch the major modules you'll need to build or modify. Actively look for opportunities to extract **deep modules** that can be tested in isolation — a lot of behaviour behind a small, stable interface (as opposed to a shallow module whose interface is nearly as complex as its implementation).

   Record the modules and which ones warrant tests directly in the PRD's Implementation/Testing sections. If the user is present, confirm the module breakdown and test targets with them before writing the file; if running unattended, state your assumptions explicitly in the PRD so they can be reviewed.

3. Write the PRD to a local markdown file at `docs/prd/<NNNN>-<slug>.md` (zero-padded sequential number, scan the directory for the highest existing one and increment; create `docs/prd/` lazily if it doesn't exist). Use the template below. Tell the user the path when done.

<prd-template>

## Problem Statement

The problem that the user is facing, from the user's perspective.

## Solution

The solution to the problem, from the user's perspective.

## User Stories

A thorough numbered list of user stories, each in the format:

1. As an <actor>, I want a <feature>, so that <benefit>

<user-story-example>
1. As a mobile bank customer, I want to see balance on my accounts, so that I can make better informed decisions about my spending
</user-story-example>

Cover the feature's real surface area — but every story must earn its place. Prefer a tight set of sharp stories over an exhaustive padded one.

## Implementation Decisions

A list of implementation decisions that were made. This can include:

- The modules that will be built/modified, and their interfaces
- Technical clarifications from the developer
- Architectural decisions
- Schema changes
- API contracts
- Specific interactions

Do NOT include specific file paths or code snippets. They may end up being outdated very quickly.

Exception: if a prototype produced a snippet that encodes a decision more precisely than prose can (state machine, reducer, schema, type shape), inline it within the relevant decision and note briefly that it came from a prototype. Trim to the decision-rich parts — not a working demo, just the important bits.

## Testing Decisions

A list of testing decisions that were made. Include:

- A description of what makes a good test (only test external behavior, not implementation details)
- Which modules will be tested
- Prior art for the tests (i.e. similar types of tests in the codebase)

## Out of Scope

A description of the things that are out of scope for this PRD.

## Further Notes

Any further notes about the feature.

</prd-template>

## Next

→ `/to-issues` to break the PRD into tracer-bullet slices.
