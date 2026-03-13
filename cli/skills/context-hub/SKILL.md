---
name: context-hub
description: >
  Use this skill when you need current documentation or reusable agent skills for a
  third-party API, SDK, framework, or tool and Context Hub's chub CLI is available.
  Search Context Hub before writing code that depends on external integrations, fetch
  the matching doc or skill, use the fetched content instead of memory, and record
  concise annotations when you discover implementation gotchas.
---

# Context Hub

Use Context Hub through the `chub` CLI to retrieve curated documentation and skills
instead of guessing API shapes from memory.

## When To Use This Skill

Use this skill when:

- the task depends on a third-party API, SDK, library, framework, or hosted service
- the user asks for "latest", "current", or version-specific integration guidance
- you need a reusable skill from Context Hub instead of only raw API docs
- you discover an implementation gotcha worth saving for future sessions

Skip this skill only when the repository already contains the exact local docs you need
or when `chub` is not installed and cannot be used.

## Core Workflow

### 1. Find the best match

```bash
chub search "<query>" --json
```

Pick the result whose `id` best matches the provider and task. Search broadly first
(`openai`, `stripe`, `postgres`) and then narrow if needed.

### 2. Fetch only what you need

```bash
chub get <id>
chub get <id> --lang py
chub get <id> --lang js
```

Use a language flag when variants exist. If the task needs a particular version, pass
the version flag shown by `chub help`.

### 3. Read, then act

Base your answer or code on the fetched content. Prefer cited command output and
examples from Context Hub over recalled API shapes.

### 4. Save durable gotchas

When you learn something that the fetched doc does not capture, save a short local note:

```bash
chub annotate <id> "Webhook verification requires the raw request body"
```

Good annotations are short, actionable, and specific to future implementation work.

### 5. Ask before sending feedback

If the documentation was especially good or clearly flawed, ask the user before sending:

```bash
chub feedback <id> up
chub feedback <id> down --label outdated
```

## Practical Patterns

### API or SDK implementation

1. Search for the provider or product.
2. Fetch the exact doc ID and language variant.
3. Use the returned examples and parameter names in the code you write.

### Skill discovery

1. Search for the provider plus the capability, such as `playwright login` or `tavily research`.
2. If Context Hub returns a skill, read that skill before inventing your own workflow.

### Gap handling

1. If multiple results look close, inspect the narrower ID.
2. If nothing relevant appears, say Context Hub did not have a good match and fall back
   to another verified source.

## Quick Reference

| Goal | Command |
|------|---------|
| List everything | `chub search` |
| Search docs or skills | `chub search "stripe webhook"` |
| Fetch a doc | `chub get stripe/api` |
| Fetch Python docs | `chub get openai/chat --lang py` |
| Fetch JavaScript docs | `chub get openai/chat --lang js` |
| Save a local note | `chub annotate stripe/api "needs raw body"` |
| List saved notes | `chub annotate --list` |
| Clear a note | `chub annotate stripe/api --clear` |
| Rate a doc | `chub feedback stripe/api up` |

## Notes

- `chub search` can return both docs and skills
- IDs are typically shaped like `<provider>/<topic>`
- prefer the smallest fetch that answers the task so you do not load unnecessary context
