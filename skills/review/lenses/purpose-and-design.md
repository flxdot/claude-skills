# Lens: purpose and design

Every line of the diff has to **reveal its intent**. The question behind this lens is "Why is this needed?". When neither the code nor its context answers it, that gap is the finding: the code hides its intent.

Ask it as a genuine question. The author may have a good reason the reviewer does not know. The answer decides the fix:
- rename or restructure the code so that it shows the reason;
- add a comment that says why;
- delete the code when there is no reason.

Report every finding with:
- `file:line`;
- the finding;
- the evidence: a link to existing code, a scenario, or a quote from the diff;
- a severity.

The lens is done when:
- every added file, function, prop, parameter, option, table and column has been asked "why is this needed?";
- every removed line has been asked "what did it do, and where does it happen now?".

## Additions

- **Dead code.** Look for unused functions, imports, parameters, variables, watchers and casts, query clauses whose result is never read, empty style blocks, and wrapper elements that do nothing.
- **Options that push a decision onto the caller.** Shared components and functions should be **opinionated**: they build in the best practice and stay out of the caller's way. For example: "Why do I have to know and care about that? The component needs to handle that for me." Or: "Our goal is not to build a general usage UI library."
- **Two ways to do one thing.** For example, two flags or two bindings where one mode parameter would do.
- **Scope creep.** Look for features the ticket did not ask for, and for generated extras that "just add complexity for humans".
- **Tables and relations that are not needed.** Ask how the cardinality plays out with real data, for example: "Do we need a m:n relation here? How would you show this if one item appears in two categories?"

## Removals

- Look for validations, test cleanups, interface members or behaviour that disappear without a reason, for example: "Why is this validation no longer required?"
- Look for behaviour that changes inside a refactor, for example: "Now you always kill it. Is that intentional?"
- Check that a bug fix explains how it fixes the bug, for example: "The refactoring is good. But I don't understand how the refactoring solves the bug."

## Reuse what exists

- Before accepting a new helper, component, type or constant, search the codebase for an existing one and link it.
- Prefer standards and built-ins over custom code, such as `Intl`, `mimetypes`, `secrets`, `contextlib.suppress` and the platform's date and number formatting. As one comment put it: "There is no need to reinvent the wheel."
- Point out duplication within the diff and between the diff and existing code. Duplication shows that the code belongs in one shared place.

## One source of truth

Values that must stay in sync should be **formally connected**, so that a change in one place breaks the build in the other instead of drifting silently. For example:
- Frontend types and constants are generated from the API schema, not written by hand a second time.
- Ids are built by one helper, not put together at every call site.
- Colors, sizes and layers come from design tokens, not literals.
- A configuration value lives in one place, so that a docstring cannot "start to lie".
- A root cause is fixed once, not worked around at every use. Wrong data is fixed with a migration, not filtered out on every request.

## Data model and API shape

- **Revision and history tables.**
  - The primary key belongs on the main table.
  - The composite key belongs only on the history table.
  - A column goes on the history table only if its history matters.
  - Foreign keys that should always follow the latest revision point at the main table.
- **Columns and keys.**
  - Look for missing foreign keys.
  - Column types should be sized for their content: a hex color is not `varchar(32)`.
  - Use `BIGINT` where `INTEGER` can run out.
  - Put primary key columns first.
- **Unreleased tables.** Fix a table that has not been released yet in its initial migration, not in a follow-up migration.
- **Nullable fields.** Prefer a nested model over sibling nullable fields. Either the whole group is `None`, or every field in it is set.
- **REST resources.**
  - Each resource has find, create, get by id, update and delete.
  - Mandatory identifiers are path parameters.
  - A find endpoint never returns 404.
  - Settings need get and set, not full CRUD, and cannot be deleted.
- **Ambiguous interfaces.** A parameter like `dict[int, int]` becomes a named tuple or a typed model.
- **Public API.** A route that external clients integrate with stays in place. Breaking it is a blocking finding. Extend the API instead of changing it.

## Placement and boundaries

- Code specific to one integration or one customer does not belong in the core. Generic code and docs do not mention a specific integration.
- Generic components go into the shared UI library. Single-use components sit next to the page that uses them.
- Prefer **deep functions**. Pass the domain object to one function and let that function decide. Avoid a pair of shallow functions that the caller has to choose between correctly.
- Do not create `utils` or `*_helper` modules. Such a name "reveals that you are unable to define a purpose".
- Split a store, composable or form that does too much. For example, a form holds inputs and the page sends the requests.
