# claude-skills

Agent skills by flxdot, packaged as a Claude Code plugin.

| Skill | What it does |
|-------|--------------|
| `/flxdot:review` | Reviews a pull request or branch the way flxdot reviews code. Four lenses run as parallel subagents: purpose and design, correctness, naming and readability, product and copy. Their findings are merged and written as inline comments with a verdict. |

## Install

In Claude Code, add this repository as a marketplace and install the plugin:

```text
/plugin marketplace add flxdot/claude-skills
/plugin install flxdot@flxdot
```

Or from a terminal:

```bash
claude plugin marketplace add flxdot/claude-skills
```

```bash
claude plugin install flxdot@flxdot
```

Restart Claude Code, or run `/reload-plugins`, to load the skills.

To update to the latest version:

```bash
claude plugin marketplace update flxdot
```

```bash
claude plugin update flxdot@flxdot
```

## Use

```text
/flxdot:review 1234
/flxdot:review          # reviews the current branch against the default branch
```

The skill reads the project's own agent and contribution docs (`AGENTS.md`, `CLAUDE.md`, `CONTRIBUTING.md`, glossary, design system) and uses them as the baseline. Findings that a linter, formatter or type checker already enforces are dropped.

The skill only runs when you type it. Claude never starts it on its own.

## Layout

```text
.claude-plugin/
├── marketplace.json      # makes the repo installable as a marketplace
└── plugin.json           # the flxdot plugin
skills/review/
├── SKILL.md              # steps: collect, dispatch lenses, merge, write
├── lenses/               # one file per review lens, each run by a subagent
├── guidelines/           # naming and Python rules the lenses fall back on
└── voice.md              # how comments are worded, and how the verdict is chosen
```

## License

MIT
