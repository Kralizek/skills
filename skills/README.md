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

## Catalog

### `csharp-testing`

**Purpose:** Portable C# testing conventions for .NET using NUnit 3, AutoFixture 4, and FakeItEasy 8.

**Use when:**

- creating or refactoring C# tests that use the NUnit + AutoFixture + FakeItEasy stack
- standardizing test structure and assertion style across repositories
- bootstrapping a reusable AutoData attribute pattern

**Key conventions:**

- nested fixture structure with NUnit `[TestFixture]`, `[TestOf]`, and `[Test]`
- test classes are annotated with `[TestOf]` for class or method under test
- custom `AutoDataProvider` and `InlineAutoDataProvider` attributes with `IFixtureCustomization`-first naming
- `[Frozen]` dependency ordering before SUT parameters
- FakeItEasy interaction setup/verification and NUnit constraint assertions

**Supporting files:**

- `SKILL.md`
- `template.AutoDataProviderAttributes.cs`

**Notes:** Includes a cross-repository-friendly AutoData template with extension points for project-specific fixture customizations.

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
