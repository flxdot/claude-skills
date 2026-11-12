---
name: review
description: Review a pull request or branch the way flxdot reviews code. Covers purpose, correctness, naming and product fit, and writes the result as inline comments.
disable-model-invocation: true
---

Review the diff of a pull request or a branch the way flxdot reviews code.

## Steps

1. **Collect the change.** Get the diff against its merge base: `gh pr diff <number>`, or `git diff <default-branch>...HEAD`. Also get the PR title and description and the linked ticket. Read the project's agent and contribution docs (`AGENTS.md`, `CLAUDE.md`, `CONTRIBUTING.md`, design and glossary files), because their rules and terms are the baseline every lens builds on. The step is done when you know:
   - which ticket or goal the change serves;
   - which areas it touches: backend, database and migrations, data pipelines, frontend, user-facing copy, translations, changelog or docs.
2. **Dispatch the lenses.** Start one subagent per lens, all in parallel. Give each one the diff, the ticket, the paths of the project docs, the content of its lens file and the absolute paths of the [`guidelines/`](guidelines/) files that its lens names:
   - [`lenses/purpose-and-design.md`](lenses/purpose-and-design.md)
   - [`lenses/correctness.md`](lenses/correctness.md)
   - [`lenses/naming-and-readability.md`](lenses/naming-and-readability.md)
   - [`lenses/product-and-copy.md`](lenses/product-and-copy.md). Skip this lens when the diff touches no UI, user-facing copy, translations, changelog or docs.

   Each lens returns its findings as `file:line`, the finding, the evidence and a severity of `blocking`, `should` or `optional`. The step is done when every dispatched lens has returned.
3. **Merge.**
   - Drop duplicates and keep the strongest wording.
   - Drop every finding a linter, formatter or type checker already enforces. People are not linters; the tools do that job.

   The step is done when each remaining finding points at one line and states what changes for the reader or the user.
4. **Write the comments** in the voice of [`voice.md`](voice.md). The step is done when every finding is an inline comment and the review has a verdict.

On a diff of more than about 1500 changed lines, review only the architecture and the module boundaries. Say so in the review body: "Superficial review due to size. Only looked at the general changes."
