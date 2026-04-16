---
name: csharp-testing
description: "Portable C# testing conventions with NUnit 3, AutoFixture 4, and FakeItEasy 8. USE FOR: unit and service tests, handler tests, endpoint-adjacent tests, test structure standards, custom AutoData patterns. DO NOT USE FOR: frontend component tests, load testing, and non-.NET test stacks."
---

# Skill: C# Testing (NUnit 3 + AutoFixture 4 + FakeItEasy 8)

## Purpose

Define a portable, opinionated C# testing convention for .NET code using:
- NUnit 3 for test execution and assertions
- AutoFixture 4 for data generation and SUT composition
- FakeItEasy 8 for fakes, stubs, and call verification

This skill is optimized for service-layer, handler, endpoint-adjacent, and application-logic tests, and can be reused for other test layers with the same stack.

## When To Apply

Apply this skill when:
- adding or refactoring C# tests that follow the NUnit + AutoFixture + FakeItEasy stack
- introducing a standard AutoData pattern in a repository
- migrating mixed test styles into a single convention
- setting up a new test project that should be easy to replicate across repositories

Do not apply this skill for:
- full HTTP/integration tests requiring real infrastructure
- UI/component frontend tests
- performance/load testing

## Required Conventions

1. Use NUnit 3 attributes and structure:
- `[TestFixture]` on outer test class
- mark test classes with `[TestOf]` targeting either:
    - the class under test, or
    - the method under test
- use nested class per method under test when helpful
- `[Test]` for test cases

2. Use custom AutoData attributes from a shared file:
- `AutoDataProviderAttribute`
- `AutoDataProviderAttribute<TCustomization>`
- `InlineAutoDataProviderAttribute`
- `InlineAutoDataProviderAttribute<TCustomization>`

3. Use AutoFixture + AutoFakeItEasy to construct SUTs and dependencies.

4. Use FakeItEasy for interaction setup and verification:
- `A.CallTo(...).Returns(...)`
- `A.CallTo(...).MustHaveHappened...`

5. Use constraint-style assertions only:
- `Assert.That(actual, Is.EqualTo(expected))`
- prefer `Assert.Multiple` for related assertions

## Preferred Patterns

### Class and method naming

- Test class: `{ComponentUnderTest}Tests`
- Nested class: method-under-test name, e.g. `Create`, `Delete`, `ListUsers`
- Test method: sentence style with underscores, e.g. `Throws_when_user_not_found`

### Parameter ordering with `[Frozen]`

Freeze shared dependencies before the SUT parameter:

```csharp
[Test]
[AutoDataProvider]
public async Task Saves_user(
    [Frozen] IUserRepository userRepository,
    UserService sut,
    CreateUserRequest request)
{
    // ...
}
```

### Customization pattern for local fixture tuning

Use `AutoDataProviderAttribute<TCustomization>` when a test class needs deterministic fixture setup:

```csharp
file class TestCustomization : IFixtureCustomization
{
    public void Customize(IFixture fixture)
    {
        fixture.Register(() => new MyOptions { Enabled = true });
    }
}

[Test]
[AutoDataProvider<TestCustomization>]
public void Uses_custom_fixture(MyService sut) { }
```

Keep customizations file-scoped and close to tests that use them.

Use `IFixtureCustomization` by default for new or refreshed test stacks.
If a repository already has an `IFixtureConfigurator`-based stack, keep using it as-is and do not migrate only for naming consistency.

## Anti-Patterns

Avoid:
- mixing test frameworks (xUnit, MSTest) in the same repository convention
- creating `new Fixture()` directly inside test methods
- constructing the SUT manually when AutoFixture can wire it
- assertion styles like `Assert.AreEqual` / `Assert.True`
- hidden shared state between tests

## Suggested Project Setup

Add these package references to test projects:

```xml
<ItemGroup>
  <PackageReference Include="NUnit" Version="3.*" />
  <PackageReference Include="NUnit3TestAdapter" Version="4.*" />
  <PackageReference Include="AutoFixture" Version="4.*" />
  <PackageReference Include="AutoFixture.NUnit3" Version="4.*" />
  <PackageReference Include="AutoFixture.AutoFakeItEasy" Version="4.*" />
  <PackageReference Include="FakeItEasy" Version="8.*" />
</ItemGroup>
```

## Supporting Files

- `template.AutoDataProviderAttributes.cs`: portable AutoData and fixture factory template for direct reuse across repositories.

## Notes For Cross-Repository Reuse

- Keep the template namespace generic (commonly `Tests`).
- Add repository-specific customizations in fixture customizations instead of rewriting attribute classes.
- If a repository already uses `IFixtureConfigurator`, keep that convention and avoid churn-only migrations.
- If domain entities require recursion/cycle handling, centralize that in the fixture factory, not in individual tests.
