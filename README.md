# doc-info-archi

A [Claude Code](https://claude.com/claude-code) skill that helps Claude design and improve the **documentation information architecture** of an existing codebase — so AI agents (and humans) can navigate it efficiently.

Instead of producing one bloated `CLAUDE.md`, it builds a **layered information architecture** rooted in *progressive disclosure*: a lean entry file that routes to specialized, co-located docs read only when a task needs them.

It applies the ideas from [Harness Engineering](https://prittamravi.dev/writing/harness-engineering/) — the repo as the system of record, the five harness subsystems, and the six documentation principles — to the narrow job of structuring a codebase's docs.

## What it does

When invoked, the skill walks Claude through:

1. **Survey reality** — read the repo for ground truth before writing anything.
2. **Design a layered doc tree** — a lean entry file (`CLAUDE.md`/`AGENTS.md`) plus specialized docs co-located with the code they describe.
3. **Write to the six documentation principles** — proximity, standardized entry, minimalism, intent over implementation, discoverability, sync.
4. **Make it verifiable** — every command and claim checkable; link instead of duplicate.
5. **Hand off** — proposed doc tree, rationale, and a maintenance note.

The skill itself practices progressive disclosure: a short `SKILL.md` workflow that routes to reference files for depth.

```
doc-info-archi/
├── SKILL.md                 # always-loaded: core principle + workflow
└── references/
    ├── principles.md        # the reasoning, read on demand
    └── templates.md         # doc scaffolds, read on demand
```

## Install

Clone into your personal skills directory:

```bash
git clone https://github.com/jeraldkiew/doc-info-archi.git ~/.claude/skills/doc-info-archi
```

Or, for a single project, clone into `.claude/skills/` inside the repo.

## Use

Ask Claude to *"document this codebase"*, *"set up AGENTS.md"*, *"organize the docs"*, or invoke `/doc-info-archi` directly.

## License

MIT — see [LICENSE](LICENSE).
