# Lens: product, UI and copy

Judge the change as the product owner would, and as the person who uses it every day. The project's design system, product description, copy guide and glossary are the baseline.

Report every finding with:
- `file:line`;
- the finding;
- a ```suggestion block with the better wording or markup, where there is one;
- a severity.

The lens is done when every changed screen, label, translation, changelog entry and docs page has been checked.

## UX

- Design for the product's own users, not to copy the legacy system it replaces: "I'm more concerned on how our UI/UX is and not what the old system did."
- **Forms and validation.**
  - Keep the submit button disabled until the form is valid.
  - Show `null` as invalid instead of silently turning it into `0`.
- **Feedback.** Show no success toast when the result is already visible: "Success is already communicated by the fact that the details page is shown."
- **Buttons.**
  - Use one primary button per dialog.
  - Keep button order consistent with a shared button group.
  - Put the add button into the table header.
- **Lists and layout.**
  - Hide deleted items by default.
  - Keep the layout from jumping when a button disappears.
- **Permissions.** When the tenant has a feature but the user lacks the permission, show the option disabled.
- **Controls.**
  - Choose a control by what it means: a choice between two options is not a toggle.
  - Decide deliberately between tabs and radio buttons.
- **Empty states.** Give empty slots and empty cells a default rendering, not blank space.
- **Settings.** Prefer storing state in user settings over adding a settings page.

## Theming

- Use semantic color variables, not palette steps, so that dark mode works: "it won't be enough to use colors from a palette, but to actually use some sort of semantic color variable."
- Colors read from the theme are computed values. Otherwise "a theme switch will never apply the changes".
- SVGs use `currentColor`.
- Layers, sizes, paddings and gaps come from their tokens, and paddings and gaps are not mixed up.

## Wording and translations

- **Labels** are actionable and accurate. A "Cancel" button that throws away changes is misleading.
- **Domain terms.**
  - Every language uses the domain's established terms, not a literal translation. Check the glossary, and the standards the industry cites.
  - Distinguish an umbrella term from a specific one, such as fault vs. alarm.
  - Write umlauts and other special characters properly, never as transliterations.
- **What stays untranslated.** Technical identifiers stay untranslated. Shared words come from the shared translations.
- **Completeness.** Every supported locale is present. Generated locale files are fixed at their source.
- **Units** follow the conventions of the rest of the product.

## Changelog and docs

- **Changelog entries.**
  - An entry is one short sentence about what the user notices: "Please summarize. This is way too verbose for an end user to read."
  - It goes into the unreleased section, never into a version that has already been released.
  - Every user-facing change has an entry, and nothing else gets one. Iterations on a feature that has not been released yet, and internal tools, get no entry.
- **User docs** change together with the feature, in every language. They do not depend on UI details that are likely to change.
- **Deferred work** becomes a ticket: "Please create a corresponding ticket and make it aware in sprint planning."
