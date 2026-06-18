# Principles — Progressive Disclosure & Harness Engineering

This skill applies the documentation ideas from **"Harness Engineering"** (https://prittamravi.dev/writing/harness-engineering/) to the narrow job of structuring a codebase's docs for AI agents. Read this file when you need the reasoning behind a recommendation, not just the steps.

## The repo is the system of record

> "Information that doesn't exist in the repo doesn't exist for the agents."

Anything living only in someone's head, a Slack thread, or a closed ticket is invisible to a fresh agent. The highest-value documentation captures exactly this kind of knowledge — the *non-obvious why*, the gotchas, the "we tried X and it broke." Don't waste lines restating what the code already shows.

## Progressive disclosure

A single bloated entry file fails two ways: it blows the context budget, and the important lines get buried. Instead, layer the information:

- **Entry file (always read):** keep the core to roughly **50–200 words**. Priority-ordered. It mostly *routes* to deeper docs rather than containing them.
- **Specialized docs (read on demand):** `DECISIONS.md`, `VERIFY.md`, a domain `README` inside a subdirectory — pulled into context only when a task touches them.

### Lost in the middle

Models attend most reliably to the **beginning and end** of a document and degrade in the middle. So in the entry file put the most critical, must-not-miss instructions at the **top and bottom**, and the routing/reference material in between.

## The harness five subsystems (gap checklist)

A well-documented codebase answers for each of these. Use the list to spot what's missing — not as a mandate to create five files.

1. **Instructions** — the entry file (`AGENTS.md` / `CLAUDE.md`): conventions, what to do and avoid.
2. **Tools** — available commands, scripts, generators, MCP servers, CLIs the agent should use.
3. **Environment** — how to get to a runnable state: setup, dependencies, env vars, services.
4. **State** — what's done and in flight: `PROGRESS.md`, `DECISIONS.md`, an architecture-decision log.
5. **Feedback** — how to know it worked: test/lint/typecheck commands, verification steps, what good output looks like.

## The four startup questions

Any agent picking up the repo cold needs these answered fast — make sure the IA answers them, ideally from the entry file or one hop away:

1. How do I **run** it?
2. How do I **test / verify** it?
3. What's the **current state**?
4. What's **next**?

## The six documentation principles

1. **Proximity** — keep rules near the code they govern (subdirectory docs over a distant wiki).
2. **Standardized entry** — one predictable landing page agents always start from.
3. **Minimalism** — remove redundant rules; fewer, sharper instructions beat exhaustive ones.
4. **Intent over implementation** — document *why* and *what for*; the code is authoritative for *how*, and implementation detail in prose goes stale fastest.
5. **Discoverability** — clear naming and structure so the relevant doc is findable without reading everything.
6. **Sync** — documentation must change with the code or it becomes actively misleading. Prefer a single source of truth (link, don't copy) and note how each doc stays current.

## Verification & observability

Don't let docs assert success vaguely. Make claims **checkable**: name the exact command and what a pass looks like. Good error/verification guidance is specific — "`POST /api/reset-password` returns 500 → check `EMAIL_API_KEY`" beats "the endpoint fails sometimes." Where practical, actually run the command before documenting it.

## Initialization vs. implementation

Separate "how to get a runnable, verifiable environment" (initialization) from "how features are built" (implementation). The startup-readiness material — run, test, state, next — should be quick to reach and not tangled into feature-level detail.
