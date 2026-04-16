# AI Guidance Library

A curated repository of reusable **skills*- and **instructions*- for AI coding agents.

This repository is not tied to a single product or codebase. Its purpose is to capture strong engineering conventions, review guidance, testing patterns, and authoring rules in a form that can be reused across many repositories.

The main goal is to turn repository-specific guidance into **portable, well-structured assets*- that can be copied, adapted, or distilled into new repositories with minimal effort.

## Why this repository exists

Many repositories gradually accumulate useful AI guidance:

- coding conventions
- testing patterns
- review expectations
- changelog rules
- serialization practices
- API design conventions
- domain-specific engineering heuristics

That guidance is often valuable beyond the repository where it first appeared, but it is usually mixed with local project details.

This repository exists to:

- extract the reusable parts
- remove unnecessary repository-specific coupling
- preserve strong conventions where they matter
- organize the result so both humans and agents can discover and apply it easily

## What lives here

This repository contains two main kinds of assets:

### Skills

Skills capture reusable engineering practices or conventions.

Examples:
- NUnit + AutoFixture + FakeItEasy testing conventions
- nested fixture test structure
- minimal API endpoint design
- JSON serialization practices
- PR hygiene
- changelog policy

Each skill lives in its own folder:

```
/skills/<skill-name>/SKILL.md
```

A skill folder may also contain supporting files such as:

- examples
- templates
- sample code
- reference notes

### Instructions

Instructions capture higher-level agent behavior and repository-wide expectations.

Examples:

- root repository guidance
- PR review behavior
- authoring expectations
- maintenance rules

Each instruction set lives in its own folder:

```text id="x8qnm9"
/instructions/<instruction-name>/INSTRUCTIONS.md
```

## Repository structure

```text id="nhn7uz"
/skills
  /<skill-name>
    SKILL.md
    ...

/instructions
  /<instruction-name>
    INSTRUCTIONS.md
    ...

README.md
copilot-instructions.md
```

## Catalogs

Both top-level content folders include a catalog README to make discovery easier for humans and agents:

- `/skills/README.md`
- `/instructions/README.md`

These files provide:

- a comprehensive list of available items
- a quick explanation of each item
- enough context to help identify what is relevant for a given task

They are part of the repository’s discovery mechanism and should be kept up to date whenever content changes.

## Design principles

Assets in this repository should be:

- **portable** across repositories
- **clear** and easy to scan
- **opinionated** where conventions matter
- **structured** for agent consumption
- **low-noise**, with signal over fluff
- **easy to extend** with examples and templates

When deriving guidance from another repository, prefer extracting the underlying engineering practice rather than copying local business context.

## Portable vs repository-specific guidance

This repository focuses on guidance that can travel well.

Good candidates include:

- testing stack and structure conventions
- endpoint design patterns
- serialization defaults
- review behavior
- changelog rules
- source generation conventions
- CI hygiene

Poor candidates include:

- product-specific business entities
- exact local folder layouts
- temporary migration rules tied to one codebase
- domain language that only makes sense in one repository

Repository-specific details may still appear in examples or reference notes when they are useful, but the canonical guidance should remain as portable as possible.

## Typical workflow

A common use of this repository is:

1. identify useful guidance in an existing repository
2. separate portable conventions from local assumptions
3. create or update a skill or instruction here
4. add examples or templates if they improve reuse
5. update the relevant catalog README
6. copy or adapt the resulting asset into a consumer repository

## Testing guidance is a core use case

A particularly important use case for this repository is preserving strong testing conventions in reusable form.

For example, this repository may capture conventions around:

- NUnit
- AutoFixture
- FakeItEasy
- custom AutoData providers
- nested fixture structure
- frozen dependency patterns
- focused test arrangement and assertions

The intent is not to produce generic “write good tests” advice, but to preserve concrete, repeatable testing conventions that can be applied consistently across new repositories.

## Contribution expectations

When adding or updating content:

- keep the asset focused
- prefer strong, reusable conventions
- avoid unnecessary repository-specific details
- keep naming and structure consistent
- update `/skills/README.md` when skills change
- update `/instructions/README.md` when instructions change

## Intended audience

This repository is meant for:

- AI coding agents
- maintainers building new repositories
- teams that want reusable engineering guidance
- anyone curating conventions that should survive beyond a single codebase

## In short

This repository is a **library of reusable AI guidance assets**.

It exists to help turn hard-won repository conventions into portable skills and instructions that can be reused across future projects.
