# TendorManagerHLTH1 — Project Guide

## Read this first

At the start of every session, read `STATUS.md` in the repo root. It is the single source of current project state: what was last completed, what the next action is, and any open blockers. Brief the user from it if they ask where things stand.

---

## What this project is

A procurement intelligence tool for healthcare IT tenders. The product monitors public procurement portals in Estonia (primary market) and the Netherlands (second market), retrieves tender notices and specification documents, and surfaces intelligence about buyers, requirements, and award patterns.

The original v1 plan (Italy/Germany, static TED CSV) was suspended in April 2026 after a data architecture finding invalidated the core assumption. The project is now in a post-pivot research-to-build transition.

---

## Key documents

| Document | Purpose |
|---|---|
| `STATUS.md` | Current project state — read at session start |
| `plan/2026-04-post-pivot-action-plan.md` | Active plan — Steps 2, 3, 4 |
| `plan/2026-04-market-selection-decision.md` | Market selection decision record (Step 2 output) |
| `research/2026-04-pivot-decision.md` | Why the original plan was suspended |
| `research/2026-04-ted-data-architecture.md` | Two-layer data problem definition |
| `PLAN.md` | Original v1 plan — suspended, kept for reference |

---

## Branch convention

Active development branches are named `claude/[description]-[id]`. The current working branch is defined in the active task context. Never push to main without explicit instruction.

---

## How to capture project state

Run `/gitcheckpoint` when pausing work. This updates `STATUS.md` with the current position, last action, next step, and any open blockers.
