---
name: doc-info-archi
description: Design and improve the documentation information architecture of an existing codebase so AI agents (and humans) can navigate it. Builds a lean entry file (CLAUDE.md/AGENTS.md) that progressively discloses to specialized, co-located docs. Use when the user asks to "document the codebase", "set up AGENTS.md/CLAUDE.md", "organize the docs", "make this repo agent-readable", "fix our documentation structure", or "apply progressive disclosure / harness engineering" to a repo.
license: MIT
---

# Document Information Architecture

Design how a codebase explains itself to an agent — not by writing one giant file, but by building a **layered information architecture** rooted in *progressive disclosure*. The goal is that a fresh agent (or human) lands on one entry file and can pull exactly the context a task needs, and no more.

**Core principle:** Optimize for *retrieval under a token budget*, not for completeness. A doc that says everything gets skimmed and ignored; a lean entry file that routes precisely to deeper docs gets used. Document what is *non-obvious from the code itself* — intent, constraints, how to run and verify — and let the code be the source of truth for everything else.

The two ideas this skill operationalizes (read `references/principles.md` for the full treatment, including the harness-engineering source):

- **Progressive disclosure** — a small, always-read entry file; everything else lives in specialized docs read *only when relevant*.
- **The repo is the system of record** — "information that doesn't exist in the repo doesn't exist for the agents." Capture knowledge that currently lives only in people's heads.

## Step 1 — Survey reality before writing a word

Never document what you assume. Read the repo and establish ground truth:

- Top-level layout, languages, package/build manifests, entry points.
- **Existing docs**: `README`, `CLAUDE.md`, `AGENTS.md`, `CONTRIBUTING`, `docs/`, scattered `*.md`. Note what's stale, duplicated, or wrong.
- **The four startup questions** every agent needs answered: How do I run it? How do I test/verify it? What's the current state? What's next? Find where (if anywhere) each is answered today.
- Build / test / lint commands — and confirm they're real (don't transcribe a command you haven't seen succeed; flag unverified ones).

Report what exists vs. what's missing before proposing structure. If the repo is large, scope with the user rather than documenting everything shallowly.

## Step 2 — Design the layered architecture

Sketch the doc tree *before* writing. Two layers:

1. **Entry file** (`CLAUDE.md` or `AGENTS.md` — match the repo's existing convention, or ask). This is the landing page, kept lean: priority-ordered, with the most critical instructions at the **top and bottom** (models lose the middle). It should mostly *route* — "for X, see `path/to/X.md`" — not contain everything.
2. **Specialized docs**, each read only when its task arises. Co-locate them with the code they describe (**proximity** — a rule about `payments/` lives in `payments/`, not in a far-off wiki).

Use the harness five-subsystem checklist to find gaps — see `references/principles.md`. Don't create a file for a layer that has nothing real to say; minimalism beats scaffolding.

## Step 3 — Write each doc to the six principles

Apply these while drafting (full detail + templates in `references/templates.md`):

1. **Proximity** — keep guidance next to the code it governs.
2. **Standardized entry** — one obvious landing page.
3. **Minimalism** — cut redundant or self-evident rules; every line earns its place.
4. **Intent over implementation** — explain *why* and *what for*; let code show *how*.
5. **Discoverability** — clear names and predictable structure so the right doc is findable.
6. **Sync** — docs that drift become lies. Note how each doc stays current; prefer linking to copying so there's one source of truth.

## Step 4 — Make it verifiable and minimal

- Every command and claim should be **checkable**. Prefer "run `pnpm test:e2e`" over "the tests pass." Where you can, run the command to confirm it works.
- **Link, don't duplicate.** If a fact lives in code or another doc, point to it.
- Re-read the entry file against a token budget: could a fresh agent get oriented from it in under ~200 words of core content, then drill down? If not, move detail out.

## Step 5 — Hand off

Show the proposed doc tree and a one-line rationale per file. Leave a short maintenance note (how to keep docs synced as code changes) and offer to wire it into review/CI if the user wants enforcement.

When in doubt about depth, deliver the lean entry file plus the highest-value specialized doc first, then let the user pull more — practice the progressive disclosure you're documenting.
