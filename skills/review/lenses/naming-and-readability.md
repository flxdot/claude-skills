# Lens: naming, readability and tests

Code is read far more often than it is written. A reader should understand each line without stopping to unpack it.

The baseline is the project's own naming and style rules, plus [`../guidelines/naming.md`](../guidelines/naming.md) for every language and [`../guidelines/python.md`](../guidelines/python.md) for Python. Where they differ, the project's rules win. This lens adds the points those rules do not cover.

Report every finding with:
- `file:line`;
- the finding;
- a ```suggestion block with the better name or code, where there is one;
- a severity.

The lens is done when every new or renamed identifier, every comment and every changed test has been checked.

## Names that lie

A name must promise exactly what the code delivers.

- "The variable claims to be an `ApplicationId`, but is a `Record` of application types to module ids?"
- A function "claims to return all settings" but returns only some of them.
- A verb such as `build…` returns a value in one place and changes its argument in another.
- A component name suggests a generic component, but the component handles one specific case.
- A singular name holds a list, or a plural name holds a single value ("It is only one range").

## One word per concept

- Do not introduce new terms for well known concepts: "Tree, Child, Parent." For example, `subRows` should be `childRows`, and "`map_` is a new word for an existing concept."
- Do not reuse the name of a domain entity for something else: "This name conflicts with the actual entity. Please use distinct names for different concepts."
- Be specific: "which index? rowIndex? columnIndex?" Name a variable after what it contains (`yValues`, `carbonContentPercent`), not after its type (`list`, `profile`).
- Use the glossary's terms precisely. Do not mix up near-synonyms such as environment and deployment.

## Readability

- Rewrite nested ternaries and deep nesting into early returns and flat control flow.
- Make dynamic code explicit when that reads more easily: "making it explicit might be the more readable choice."
- Name the cost when a passage is hard to read: "It took me about 2min to understand what is going on here."
- Replace bitwise tricks used for logic with `not` or a helper that has a clear name.
- Use the right data structure, such as `defaultdict` instead of juggling `KeyError`. Build static values once, outside the loop.
- TypeScript:
  - no `any`;
  - prefer a generic with `KEY extends keyof …`, and `Pick<>` over `Partial<>`;
  - type labels as `Record<Status, string>` so that a missing translation fails the type check;
  - define a union type first and derive the other types from it.

## Comments

- A comment says **why**, never what. Remove generated comments that only restate the code: "They explain __what__… A good comment explains __why__."
- Flag comments that no longer match the code, comments that use an undefined word ("what is an 'arm'?"), and absolute claims ("'never' is a bit strong. It currently does not support it.").
- Ask for a comment where the reason is missing: on an early return, on an unclear state flag, on a workaround.
- The comment on a public component or entity says what the thing *is*. One example: "Before it explained a ton of things, without explaining what this is."
- Public components document their props, slots, events and exposed members.

## Tests

A test proves behaviour through the public interface, in a way a reader can follow.

- **Public interface only.**
  - Do not test private functions.
  - Do not assert that an internal function was called: "How does calling a private function prove the correctness of the code?"
  - A test that a broken implementation could pass is not a test: "I could implement this in a way that it would pass this test but would not work in reality."
- **Real collaborators over mocks.** Prefer the real database: "No need to mock something here… Simply create a second connection and read." Flag tests so heavy with mocks that "it's hard to spot where the mocking ends and the testing begins."
- **Independent expectations.** Derive expected values from the input instead of repeating constants. Do not depend on shared seed data that is maintained far away from the test.
- **Readable intent.** A test that only makes sense after a long explanation needs code that shows its intent, or a docstring: "It is impossible for me to understand why this test should fail."
- **Concrete cases.**
  - Name the specific edge cases that are missing instead of asking for "more tests".
  - Remove duplicate parametrized cases and fix wrong case ids.
  - Parametrize over enum members, not raw strings.
- **Coverage exclusions.** Excluding code from coverage while also testing it is a contradiction.
