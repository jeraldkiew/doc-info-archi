# Templates

Starting points, not forms to fill blindly. Cut any section that has nothing real to say (minimalism). Match the repo's existing conventions and tone. Every command shown must be one you've confirmed exists.

---

## Entry file — `CLAUDE.md` / `AGENTS.md`

Keep the core lean and priority-ordered. Most critical instructions at the **top and bottom**; routing in the middle. Aim for ~50–200 words of core content — push detail into linked docs.

```markdown
# <Project> — Agent Guide

<One sentence: what this is and who/what it serves.>

## Critical
- <The 1–3 rules that must never be missed. e.g. "Never edit `generated/` by hand — run `make codegen`.">

## Run & verify
- Setup: `<command>`
- Run: `<command>`
- Test / typecheck / lint: `<command>`  ← this is how you know a change is good

## Map (where to look)
- `src/<area>/` — <one line>; see `src/<area>/README.md` for rules
- Architecture decisions → `docs/DECISIONS.md`
- Current state & next steps → `docs/PROGRESS.md`

## Conventions
- <Project-specific norms not obvious from the code.>

## Before you finish
- <Re-state the must-not-skip step: run tests, update PROGRESS.md, etc.>
```

---

## Co-located area doc — `src/<area>/README.md`

For a subsystem with non-obvious rules. Proximity: it lives *with* the code.

```markdown
# <Area>

**Purpose:** <what this area is responsible for — intent, not file listing.>

**Key entry points:** `<file>` — <role>.

**Rules / gotchas:**
- <Non-obvious constraint and *why* it exists.>

**Verify changes here:** `<command scoped to this area, if any>`
```

---

## Decision log — `docs/DECISIONS.md`

Captures the *why* behind choices — the head-knowledge that's otherwise lost. Append-only. Only fill these fields from a real source (commit, PR, comment, or the maintainer); if the rationale isn't recoverable, write `TODO: confirm with maintainer` — don't invent it.

```markdown
# Decisions

## <YYYY-MM-DD> — <Decision title>
- **Context:** <what forced the choice>
- **Decision:** <what we chose>
- **Why / alternatives rejected:** <reasoning, so no one re-litigates it>
- **Consequences:** <what this constrains going forward>
```

---

## State — `docs/PROGRESS.md`

Answers "current state" and "what's next" so a fresh agent rebuilds context fast.

```markdown
# Progress

**Now:** <the one active focus.>
**Done:** <recently completed, verified work.>
**Next:** <the immediate next step, with its verification command.>
**Open questions / blockers:** <anything unresolved.>
```

---

## Verification — `docs/VERIFY.md` (or a section in the entry file)

The least likely file to be warranted — keep this in the entry file's "Run & verify" section unless verification is genuinely multi-layered.

```markdown
# Verify

| Layer | Command | Pass looks like |
|-------|---------|-----------------|
| Types | `<cmd>` | no errors |
| Unit  | `<cmd>` | all green |
| E2E   | `<cmd>` | <observable signal> |

Common failures:
- `<symptom>` → `<cause / fix>`
```
