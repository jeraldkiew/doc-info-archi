---
name: doc-info-archi
description: Design and improve the documentation information architecture of an existing codebase so AI agents (and humans) can navigate it. Builds a lean entry file (CLAUDE.md/AGENTS.md) that progressively discloses to specialized, co-located docs. Use when the user asks to "document the codebase", "set up AGENTS.md/CLAUDE.md", "organize the docs", "make this repo agent-readable", "fix our documentation structure", or "apply progressive disclosure / harness engineering" to a repo. For documentation *structure and navigation* — not for writing API reference, docstrings, or end-user content.
license: MIT
---

# Document Information Architecture

Design how a codebase explains itself to an agent — not by writing one giant file, but by building a **layered information architecture** rooted in *progressive disclosure*. The goal is that a fresh agent (or human) lands on one entry file and can pull exactly the context a task needs, and no more.

**Core principle:** Optimize for *retrieval under a token budget*, not for completeness. A doc that says everything gets skimmed and ignored; a lean entry file that routes precisely to deeper docs gets used. Document what is *non-obvious from the code itself* — intent, constraints, how to run and verify — and let the code be the source of truth for everything else.

The two ideas this skill operationalizes (read `references/principles.md` for the full treatment, including the harness-engineering source):

- **Progressive disclosure** — a small, always-read entry file; everything else lives in specialized docs read *only when relevant*.
- **The repo is the system of record** — "information that doesn't exist in the repo doesn't exist for the agents." Capture knowledge that currently lives only in people's heads — by *asking the user* for the intent and gotchas you can't recover from the code, not by inventing plausible-sounding rationale.

**Done when:** the four startup questions (run, test/verify, current state, next) are each answerable from the entry file or one hop away; the entry file's core content is ≤ ~200 words (routing links don't count); every documented command has been run or explicitly flagged unverified; and each harness subsystem is either covered or consciously marked "nothing to say." If a check fails, return to the relevant step — don't hand off partial work as done.

## Step 1 — Survey reality before writing a word

Never document what you assume. Read the repo and establish ground truth:

- Top-level layout, languages, package/build manifests, entry points.
- **Existing docs**: `README`, `CLAUDE.md`, `AGENTS.md`, `CONTRIBUTING`, `docs/`, scattered `*.md`. Note what's stale, duplicated, or wrong.
- **The four startup questions** every agent needs answered: How do I run it? How do I test/verify it? What's the current state? What's next? Find where (if anywhere) each is answered today.
- Build / test / lint commands — and confirm they're real (don't transcribe a command you haven't seen succeed; flag unverified ones).

Report what exists vs. what's missing, and the doc tree you propose, **before writing any files.**

### Stop and ask first

Surface the fork instead of guessing silently when:

- Both `CLAUDE.md` and `AGENTS.md` exist, or the repo mixes conventions — don't pick one unilaterally.
- A meaningful entry file already exists and your plan would rewrite or replace it — propose a diff/migration and confirm; never overwrite hand-written docs silently.
- The repo spans multiple packages/services or has no single obvious entry point (large enough to need scoping).
- A startup question can't be answered from what you found (e.g. the run/test command is genuinely unknown, not just unverified).
- The user's goal is ambiguous — documentation *structure* vs. API reference/docstrings/end-user content, or which repo.

## Step 2 — Design the layered architecture

Sketch the doc tree *before* writing. Two layers:

1. **Entry file** (`CLAUDE.md` or `AGENTS.md` — match the repo's existing convention, or ask). This is the landing page, kept lean: priority-ordered, with the most critical instructions at the **top and bottom** (models lose the middle). It should mostly *route* — "for X, see `path/to/X.md`" — not contain everything.
2. **Specialized docs**, each read only when its task arises. Co-locate them with the code they describe (**proximity** — a rule about `payments/` lives in `payments/`, not in a far-off wiki).

Use the harness five-subsystem checklist to find gaps — see `references/principles.md`. Don't create a file for a layer that has nothing real to say; minimalism beats scaffolding. Prefer the smallest change: edit or trim existing docs rather than replacing them, and don't add `DECISIONS.md`/`PROGRESS.md`/`VERIFY.md` if an existing doc already fills that role.

## Step 3 — Write each doc to the six principles

Write to the six documentation principles — **Proximity, Standardized entry, Minimalism, Intent over implementation, Discoverability, Sync** — defined in full in `references/principles.md` (templates in `references/templates.md`). The two that change behavior most: keep each area's rules co-located with the code they govern, and link instead of copying so there's one source of truth.

When a doc looks wrong or stale but you can't confirm it against the code or a passing command, **flag it to the user as a question — don't silently rewrite or delete it.** Distinguish "verified wrong" (a command actually fails) from "looks wrong" (your assumption). Likewise, only record decisions and rationale you can source from commits, PRs, comments, or the user; if the *why* isn't recoverable, leave a `TODO: confirm with maintainer` rather than inventing it.

## Step 4 — Make it verifiable and minimal

- Every command and claim should be **checkable**. Prefer "run `pnpm test:e2e`" over "the tests pass." Where you can, run the command to confirm it works.
- Count the entry file's core content (excluding routing links): if it runs over ~200 words, move detail into a linked doc. This is a gate, not a suggestion.

## Step 5 — Hand off

Before handing off, **simulate a cold start**: using only the entry file, answer the four startup questions and name which doc you'd open for a sample task. If any answer needs knowledge not reachable in one hop, fix the IA and repeat — loop until it passes.

Then show the doc tree with a one-line rationale per file, leave a short maintenance note (how to keep docs synced as code changes), and offer to wire it into review/CI if the user wants enforcement. When unsure how deep to go, deliver the lean entry file plus the single highest-value specialized doc first and let the user pull more.
