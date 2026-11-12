# Lens: correctness

Find defects by **tracing a concrete scenario** through the code: a specific input, a specific sequence of user actions, or a specific state of the database or of an external system.

Report a finding only when you can name the scenario that breaks the code, and phrase the finding as that scenario. For example: "If I create an order for part A, then delete part A immediately… I get a ValueError."

Report every finding with:
- `file:line`;
- the scenario;
- the wrong result;
- a severity.

The lens is done when every changed branch, query, loop, exception path and state transition has been walked through with at least one realistic input and one edge case.

## Values

- Look for valid values that are falsy: an id or a value of `0`, an empty string, `NaN` (which `??` does not catch), or `None` passed to `min()` or `max()`.
- Look for sentinels that hide meaning, such as `0` for "unlimited". "One can overlook a comment fast", so prefer `number | null`.
- Check units and precision: rounding to precision 0, `0.0`, and trailing zeros.

## Exceptions and failure paths

- Follow each exception to where it ends up. Is it handled? Does it flood the error tracker? Does it stop the functions that come after it? Does it abort a whole batch run? In a batch loop, prefer recording the failure and continuing over raising.
- Check whether a "fetch exactly one" call can find none or several rows. Also flag handling for a case the primary key already makes impossible.
- A `ValueError` that reaches the HTTP layer becomes a 500. Most of these are client errors, so map them to 404 or 4xx.
- A plugin or hook that raises must not take the core down. Isolate each hook and report its errors.

## Concurrency and transactions

- Look for race conditions between requests, background tasks and deployments, such as an index error when a list changes, or a deadlock during a rollout.
- Check for chains of commits that can leave half-written state, and for commit boundaries around deletions.
- Check for concurrent work sharing one database session, and for the scope of row locks.

## Data and migrations

- Check whether a data-fixing `UPDATE` hits more rows than intended. For example: "That would mark all child jobs as finished. Even the ones that are indeed still running."
- Never edit a migration that has already been applied. Test data does not belong in migrations; seed data does.
- In a sync with an external system, check:
  - idempotency;
  - duplicates, which a partial unique index on the external reference prevents;
  - a changed key that recreates dependent records;
  - records that are deleted and then reactivated;
  - state machines with missing transitions.
- Check historic views. For example: "So if I go back to revision 1 from 10 years ago I'll still see all sites?"

## Permissions

- A tenant that has a feature enabled does not mean the user has the permission for it. Check both.
- Queries filter by the records the user can access.
- An optional integration stays optional: the code works on installations without it.

## Domain facts

- Check every assumption about the domain and about external systems against the code or the system's API.
- Ask when something is unclear, for example: "Afaik that is not true for all customers", or "That means the external system is lying to us?"
- Use the terms of the project's glossary.

## Cost

These count as correctness findings. Workers have memory limits, and every scheduled job shares one database.

- **Filtering in application code.** Look for data loaded only to be filtered or aggregated in application code. Filter it in the query instead: "This way the DB does not have to read it and the worker does not have to allocate memory for it."
- **Wasted queries.** Look for:
  - extra round trips;
  - a fetch only to check that a record exists, where an existence query or `RETURNING` would do;
  - the equivalent of `SELECT *`: "Better to query what you need."
- **Memory over a whole run.** Results kept in memory for an entire batch run should be streamed page by page instead.
- **Frontend.** Look for:
  - repeated calls in templates where a computed value would do;
  - needless array copies;
  - expensive formatters rebuilt on every render.
