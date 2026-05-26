---
name: map-context
description: Bootstrap a project's domain documentation (CONTEXT.md glossaries, CONTEXT-MAP.md, seed ADRs) from an existing codebase. Survey mode proposes the context structure; deepen mode extracts a curated glossary for one section at a time. Use when adopting domain docs on an existing repo, or when the user wants to map a codebase's domain language or bounded contexts.
argument-hint: "[section] — omit to survey and propose structure; pass a section/context name to deepen just that one"
---

# Map Context

Establish the domain documentation for an existing codebase: the `CONTEXT.md` glossaries, the `CONTEXT-MAP.md` if there are several, and ADRs for decisions already baked into the code.

This is the *establish* step. `/grill-with-docs` **maintains** these docs during design; `/improve-codebase-architecture` **consumes** them. Run this on day 0 of adopting domain docs on a repo that doesn't have them yet, or when onboarding a new context.

Work in **two modes — never one whole-repo pass.** A single sweep extracts sharp terms early and tired, sloppy ones late. Survey once; deepen one section at a time.

## Two principles that govern everything here

- **Curate, don't dump.** A glossary's value is that it's opinionated: one canonical term, aliases listed under *Avoid*, and **general programming concepts excluded** (timeouts, error types, utility patterns don't belong — see [CONTEXT-FORMAT.md](../grill-with-docs/CONTEXT-FORMAT.md)). A scraped list of every noun in the repo is worse than no glossary. Propose candidates; let the user ratify.
- **Structure is a human call.** How many contexts a codebase has is a bounded-context judgement. Propose the split and let the user correct it — never decide unilaterally.

## Mode 1 — Survey (no argument)

Structural only. Do **not** extract terms yet.

1. Explore the repo with the Explore agent — top-level layout, module/package names, how the code is grouped, any existing docs.
2. Decide single vs multi context:
   - **Single** (most repos): one `CONTEXT.md` at the root.
   - **Multiple**: a `CONTEXT-MAP.md` at the root listing each context, where it lives, and how they relate.
3. Present the proposed structure. For a multi-context repo, show the list of contexts, their paths, and their relationships. Ask: are these the right boundaries? too split? too coarse? Iterate until the user ratifies.
4. Persist the ratified structure:
   - **Multi-context**: write `CONTEXT-MAP.md` (format in [CONTEXT-FORMAT.md](../grill-with-docs/CONTEXT-FORMAT.md)). It doubles as the checklist of sections still to deepen.
   - **Single-context**: write nothing yet — the `CONTEXT.md` is created lazily when the first term lands in deepen mode.

End by listing the sections to deepen, and note that each should be its own `/map-context <section>` run.

## Mode 2 — Deepen one section (`/map-context <section>`)

Take a single section/context and go deep. Keep the pass small — ideally a **fresh session per section** so quality doesn't decay.

1. Read `CONTEXT-MAP.md` (if it exists) to locate the section and its relationships. Explore **only** that section's code.
2. Draft a **curated** candidate glossary for this context — canonical terms, aliases to avoid, generic concepts dropped, fuzzy or overloaded terms flagged for resolution.
3. Ratify with the user using the one-at-a-time interview loop from [`/grill-me`](../grill-me/SKILL.md): present a term, recommend the canonical choice, resolve it, move on. Cross-check against the code — if a term's actual usage contradicts the proposed definition, surface it.
4. Write the section's `CONTEXT.md` as terms resolve (lazily, not batched), per [CONTEXT-FORMAT.md](../grill-with-docs/CONTEXT-FORMAT.md).
5. While reading the code you'll spot decisions already baked in (event-sourcing, a DB choice, a monorepo, a deliberate deviation). Offer an ADR **only** when it's hard to reverse, surprising without context, and the result of a real trade-off — see [ADR-FORMAT.md](../grill-with-docs/ADR-FORMAT.md). Don't manufacture ADRs for the obvious.

## Next

- More sections to map? → `/map-context <next-section>` (fresh session).
- Structure mapped, ready to plan a feature? → `/grill-with-docs`.
- Want to act on what you learned? → `/improve-codebase-architecture`.
