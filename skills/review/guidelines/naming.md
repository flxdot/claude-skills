# Naming

These rules apply to everything that has a name: files, classes, functions, variables and so on. A project's own rules take precedence where they differ.

- **Use intention-revealing names.**
- **Make meaningful distinctions.**
- **Use pronounceable names.**
- **Do not abbreviate.**
  - Exception: common, internationally used abbreviations such as HTTP, UUID, JSON and MQTT.
  - Exception: acronyms that the project's glossary defines.
  - No exception, even in a domain context: `attr`, `cfg`, `mgr`, `util`, `val`, `obj`, `tmp`, `msg`.
- **Avoid encodings.**
- **Avoid mental mapping.**
- **Use one word per concept.**
- **Use terms from the problem domain** rather than the solution domain. Pure computer science algorithms, such as depth-first search or graph traversal, are the exception.

## Common pitfalls

"Name" is ambiguous. Use one of these instead:

- **display name**: a name shown to users that is not translated;
- **label**: a name that the frontend may translate.
