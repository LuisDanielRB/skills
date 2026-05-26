---
name: write-a-skill
description: Create new agent skills with proper structure, progressive disclosure, and bundled resources, matching the conventions used by the skills in this repo. Use when user wants to create, write, or build a new skill.
---

# Writing Skills

A skill is a folder with a `SKILL.md` at its root. The frontmatter `description` is the only thing the agent sees when deciding whether to load the skill, so it does the heavy lifting. Keep `SKILL.md` focused and push depth into sibling reference files.

## Process

1. **Gather requirements** — ask the user:
   - What task/domain does the skill cover?
   - What specific triggers should load it (keywords, contexts, file types)?
   - Does it need executable scripts, or just instructions?
   - Any reference material to bundle?

2. **Draft the skill** — create the folder and `SKILL.md`. Split long or rarely-needed detail into sibling reference files (see "When to split" below).

3. **Review with the user** — present the draft and ask: Does this cover your use cases? Anything missing or unclear? Is any section too thin or too heavy?

## House conventions (match the rest of this repo)

- **Frontmatter**: `name` (kebab-case, matching the folder) and `description`. Optionally `argument-hint` if the skill takes an argument.
- **Description shape**: first sentence says what it does; second sentence starts with "Use when …" and lists concrete triggers. Third person, max 1024 chars.
- **Body**: an H1 title, a one-line statement of intent, then a numbered `## Process` (or `<what-to-do>` / `<supporting-info>` blocks for grilling-style skills). Prose over bullets where it reads better.
- **Progressive disclosure**: detail lives in sibling `UPPERCASE.md` files (e.g. `LANGUAGE.md`, `LOGIC.md`, `UI.md`), linked with relative markdown links. Reference one level deep — don't make the agent chase a chain.
- **No time-sensitive info** and **consistent terminology** — if the skill defines vocabulary, use it exactly throughout.
- **Compose, don't duplicate**: if another skill already states a protocol, link to it (e.g. `../grill-me/SKILL.md`) rather than restating it.

## SKILL.md template

```md
---
name: skill-name
description: What it does in one sentence. Use when [specific triggers].
---

# Skill Name

One line on what this skill is for.

## Process

1. Step one.
2. Step two.

[Link deeper detail one level down: See REFERENCE.md]
```

### Good vs bad description

**Good** — `Extract text and tables from PDF files, fill forms, merge documents. Use when working with PDF files or when user mentions PDFs, forms, or document extraction.`

**Bad** — `Helps with documents.` (no way to distinguish it from any other document skill.)

## When to add scripts

Add a utility script when the operation is deterministic (validation, formatting), the same code would be generated repeatedly, or errors need explicit handling. Scripts save tokens and improve reliability versus regenerating code each run.

## When to split files

Split detail into a sibling reference file when `SKILL.md` is getting long enough that the core process is hard to scan (roughly 120+ lines is a smell), when content covers distinct domains, or when a section is advanced/rarely needed. Keep `SKILL.md` as the always-loaded core; everything else is loaded on demand.

## Review checklist

- [ ] Description has a what-it-does sentence and a "Use when …" trigger sentence
- [ ] `SKILL.md` stays focused; depth is split into sibling files
- [ ] No time-sensitive info
- [ ] Consistent terminology
- [ ] Concrete examples included
- [ ] References go one level deep
- [ ] Shared protocols are linked, not duplicated
