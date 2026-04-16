# Instructions

This folder contains reusable instruction sets for repository-wide agent behavior, review behavior, authoring guidance, and maintenance expectations.

Each instruction set lives in its own folder:

- `/instructions/<instruction-name>/INSTRUCTIONS.md`

## How to use this catalog

Use this README to quickly identify which instruction sets are relevant for a task or repository setup.

For each instruction set, the summary should help answer:

- what level the instruction applies at
- what kind of agent behavior it shapes
- when it should be included or adapted
- whether it complements or overlaps with another instruction set

When adding, removing, renaming, merging, or significantly changing an instruction set, update this file in the same change.

## Instruction sets

### `<instruction-set-name>`
**Purpose:** Short explanation of what this instruction set controls.

**Use when:**
- condition or repository need
- condition or repository need

**Applies to:**
- repository-wide behavior
- PR review behavior
- authoring workflow
- maintenance workflow

**Key expectations:**
- important behavior or rule
- important behavior or rule

**Supporting files:**
- `INSTRUCTIONS.md`

**Notes:** Optional precedence, composition, or overlap guidance.

---

### `<another-instruction-set-name>`
**Purpose:** Short explanation of what this instruction set controls.

**Use when:**
- condition or repository need
- condition or repository need

**Applies to:**
- repository-wide behavior

**Key expectations:**
- important behavior or rule
- important behavior or rule

**Supporting files:**
- `INSTRUCTIONS.md`

**Notes:** Optional extra context.

---

## Authoring guidance for entries

Each entry should be:

- short enough to scan quickly
- specific enough to help an agent choose correctly
- clear about scope and precedence
- updated whenever the instruction set meaningfully changes

Prefer summaries that help an agent decide whether the instruction belongs in the current task.

For example, prefer:

- “Review behavior for PRs: emphasize signal over noise, validate instruction drift, and avoid trivial comments”

over:

- "General PR instructions"