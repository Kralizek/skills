# Copilot Instructions

This repository is a **meta repository** for reusable AI guidance assets.

Its purpose is to help create, refine, organize, and maintain **portable skills and instructions** that can later be copied or adapted into other repositories. The assets in this repo are not primarily meant to describe this repo’s own product code. They are meant to capture reusable engineering conventions, review guidance, testing practices, and repository authoring patterns in a form that can be applied elsewhere.

## Primary intent

When working in this repository:

- Treat `/skills` as the canonical source for reusable skills.
- Treat `/instructions` as the canonical source for reusable instruction files.
- Optimize for **portability**, **clarity**, and **reusability across repositories**.
- Prefer extracting **general patterns** over preserving repository-specific details.
- Preserve strong conventions when they are intentional and broadly reusable.
- Avoid coupling a skill to one product, one company, one domain model, or one repository layout unless the asset is explicitly meant to be repo-specific.

## Repository structure

### `/skills`
This folder contains reusable skills.

Each skill lives in its own folder:

- `/skills/<skill-name>/SKILL.md`

A skill folder may also contain supporting files next to `SKILL.md`, such as:

- examples
- templates
- sample code
- helper/reference material

Examples:
- `examples.md`
- `template.SampleTests.cs`
- `template.TestAutoDataAttribute.cs`

The `/skills` folder must also contain a `README.md` file that provides:

- a comprehensive list of all available skills
- a short explanation of each skill
- enough context to help agents identify which skills are relevant for a given task

Agents should keep this README up to date when skills are added, renamed, removed, merged, or significantly repurposed.

### `/instructions`
This folder contains reusable instruction sets.

Each instruction set lives in its own folder:

- `/instructions/<instruction-name>/INSTRUCTIONS.md`

Instruction folders are used for higher-level repository or agent behavior, such as:
- root repository guidance
- PR review behavior
- authoring guidance
- maintenance expectations

The `/instructions` folder must also contain a `README.md` file that provides:

- a comprehensive list of all available instruction sets
- a short explanation of each instruction set
- enough context to help agents identify which instruction sets are relevant for a given task

Agents should keep this README up to date when instruction sets are added, renamed, removed, merged, or significantly repurposed.

## What agents should do here

Agents working in this repository should help with tasks like:

- creating reusable skills from repository-specific skills
- distilling instructions from one or more existing repositories
- identifying which parts of a skill are portable vs repo-specific
- extracting reusable testing conventions
- normalizing tone, structure, and wording across skills
- adding examples or templates that make a skill easier to reuse
- keeping instructions aligned with the current skill catalog
- keeping `/skills/README.md` and `/instructions/README.md` accurate and useful for discovery

## Portability rules

When deriving a skill from another repository:

- extract the **underlying practice**, not the local business context
- remove repository names, company names, product names, domain entities, and local architecture details unless essential
- avoid references to specific paths, namespaces, classes, endpoints, tables, or business workflows unless the skill is explicitly about a narrow technical pattern
- prefer neutral examples that demonstrate the convention without carrying over unrelated domain noise
- keep the original strength of the convention when it reflects a real engineering preference

A good portable skill should explain:
- when it applies
- what conventions it enforces
- what patterns are preferred
- what should be avoided
- what a good minimal example looks like

## Local vs portable guidance

When converting source material from another repository, separate it into:

### Portable guidance
Reusable across many repositories.

Examples:
- NUnit + AutoFixture + FakeItEasy conventions
- nested fixture test structure
- JSON serialization defaults guidance
- changelog hygiene
- minimal API endpoint design patterns

### Local guidance
Specific to a particular repository and usually **not** appropriate for a portable skill.

Examples:
- exact project names
- exact namespace conventions tied to one solution
- business-specific roles, entities, workflows, or endpoint names
- folder paths that only exist in one product
- review rules tied to one team’s temporary migration

If a source repository contains both, preserve the portable parts here and leave the local parts out, or move them into clearly marked examples/reference notes.

## Preferred style for skills

Skills should be:

- direct
- structured
- specific
- short enough to be usable by an agent
- opinionated where needed
- free from unnecessary filler

Skills should usually include:

1. purpose
2. when to apply
3. required conventions
4. preferred patterns
5. anti-patterns
6. small examples
7. notes about local override points, when relevant

Do not make skills long just to be comprehensive. Favor signal over noise.

## Preferred style for instructions

Instructions should:

- define behavioral expectations for agents
- clarify precedence and intent
- explain how to use and maintain the skills in this repository
- remain concise and operational

Use instructions for repo-wide behavior. Use skills for specific engineering practices or reusable conventions.

## Discovery and catalog maintenance

This repository is meant to be easy for both humans and agents to navigate.

When adding or changing content:

- update `/skills/README.md` so it remains a reliable catalog of available skills
- update `/instructions/README.md` so it remains a reliable catalog of available instruction sets
- make sure each entry includes a quick explanation that helps an agent decide whether the item is relevant
- prefer concise summaries with strong discriminators rather than vague descriptions
- remove or revise stale entries when assets are renamed, merged, or retired

The README files are not optional documentation extras. They are part of the discovery mechanism of this repository.

## Testing skills are especially important

A key use case for this repository is preserving strong testing conventions in a portable form.

When authoring or refining testing skills, preserve conventions such as:

- NUnit as the test framework
- FakeItEasy as the mocking library
- AutoFixture-based data generation
- custom AutoData providers where relevant
- nested fixtures for class/method organization
- preferred SUT construction patterns
- frozen dependency patterns
- focused assertions and low-noise arrangement

Do not water these down into generic “write good tests” guidance. Capture the actual convention in a reusable way.

## Output expectations for generated skills

When creating or updating a skill:

- place the main guidance in `SKILL.md`
- add supporting files only when they materially improve reuse
- keep examples neutral and portable
- make the skill usable as a source asset for future repositories
- avoid accidental dependence on this meta repo’s own structure beyond the documented folder conventions

## Maintaining consistency

When updating or adding skills:

- keep naming consistent
- keep structure consistent
- avoid overlapping skills unless separation is intentional
- prefer evolving an existing skill when the new guidance is clearly the same concern
- create a new skill when the topic is distinct enough to stand alone cleanly

When updating instructions:

- make sure they still reflect the current role of the repository
- make sure they remain aligned with the skill catalog
- avoid duplicating large portions of skill content inside instruction files

## If source material is provided

When given one or more repositories, files, skills, or instruction files as source material:

- inspect the source carefully
- identify reusable patterns
- identify repo-specific assumptions
- propose or create a portable skill structure
- preserve meaningful conventions
- rewrite examples so they are portable unless the task explicitly calls for preserving the original context

## General rule

This repository is a **library of reusable AI guidance assets**.

Always optimize for:
- reuse in future repositories
- clear structure
- strong conventions
- low noise
- maintainability over time