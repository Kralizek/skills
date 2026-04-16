# Skills

This folder contains reusable skills for common engineering practices, repository conventions, and agent guidance patterns.

Each skill lives in its own folder:

- `/skills/<skill-name>/SKILL.md`

A skill folder may also contain supporting files such as examples, templates, sample code, or reference material.

## How to use this catalog

Use this README to quickly identify which skills are relevant for a task.

For each skill, the summary should help answer:

- what problem the skill addresses
- when the skill should be applied
- whether the skill is broadly reusable or specialized
- whether there are supporting templates or examples worth using

When adding, removing, renaming, merging, or significantly changing a skill, update this file in the same change.

## Skills

### `<skill-name>`

**Purpose:** Short explanation of what this skill covers.

**Use when:**
- condition or type of task
- condition or type of task

**Key conventions:**
- important convention
- important convention

**Supporting files:**
- `SKILL.md`
- `examples.md`
- `template.SomeFile.cs`

**Notes:** Optional extra context, scope boundaries, or overlap guidance.

---

### `<another-skill-name>`
**Purpose:** Short explanation of what this skill covers.

**Use when:**
- condition or type of task
- condition or type of task

**Key conventions:**
- important convention
- important convention

**Supporting files:**
- `SKILL.md`

**Notes:** Optional extra context.

---

## Authoring guidance for entries

Each entry should be:

- short enough to scan quickly
- specific enough to help an agent choose correctly
- written to distinguish the skill from nearby or overlapping skills
- updated whenever the skill meaningfully changes

Prefer summaries with strong discriminators over vague descriptions.

For example, prefer:

- “NUnit + AutoFixture + FakeItEasy unit test conventions with custom AutoData patterns”

over:

- “Testing best practices”