# Product Pivot Decision

> **Document type:** Founder decision note  
> **Author:** Weiche Lin  
> **Date:** 2026-04-24  
> **Status:** Decision made — v1 plan suspended

---

## Decision

**Pivot.** The v1 plan (static TED CSV → `data.json` → browser query interface) is suspended. The core product hypothesis has been invalidated by the data architecture research completed this week.

---

## Original Belief (Now Invalidated)

The original assumption was:

> Connect to the EU tender database, pull the data, and that data is sufficient to analyse tender history — comparing evaluation criteria, understanding how tenders are structured, identifying patterns in how buyers score and award contracts.

This belief turned out to be wrong.

---

## What the Research Showed

See [`2026-04-ted-data-architecture.md`](./2026-04-ted-data-architecture.md) for the full technical finding. The key problem:

TED (and the TED CSV files v1 was built on) is a **public notice board**, not a specification repository. It captures:

- Who is buying
- Rough contract value
- High-level CPV category
- Award outcome (winner, awarded value)

It does **not** capture:

- The technical specification (*capitolato tecnico*) — what exactly the buyer wants
- The evaluation methodology (*disciplinare di gara*) — how bids are scored
- The scoring rubric — what weights each criterion carries in practice
- The award decision detail — how each bidder performed against each criterion

This is precisely the data needed to answer the original question: *how are tenders structured, how are they evaluated, and what does it take to win?*

---

## Why Closing the Gap Is Not Feasible Now

The specification documents live on the tender issuer's own platform — a fragmented ecosystem of regional and national procurement portals (SINTEL, STELLA, TUTTOGARE, MePA, and hundreds of individual buyer websites).

These platforms have three compounding problems:

1. **No public API access.** None of the major Italian regional platforms expose programmatic access.
2. **Login-gated.** Most require registered accounts with procurement-authority credentials to access document packages.
3. **Scale.** There is no single access point — connecting to one platform solves a fraction of the market. Connecting to all of them is a significant, sustained engineering effort with uncertain return.

The technical cost of bridging Layer 1 (TED, open and structured) to Layer 2 (issuer platforms, closed and fragmented) is very high relative to where the product is today.

---

## What This Means for the Codebase

- `PLAN.md` v1 plan is suspended. The implementation order defined there is no longer the active roadmap.
- `scripts/` and `frontend/` remain empty. No build work was started, so there is nothing to undo.
- The `data-dictionary.md` and research artifacts remain valid reference material.

---

## Open Questions for the Next Direction

The pivot decision is made. The destination is not yet defined. Questions to work through:

- **Narrow the scope:** Is there a specific buyer segment (e.g. one region, one hospital network) where document access is feasible through manual or partnership means?
- **Change the data source:** Are there commercial tender intelligence providers who have already solved the Layer 2 problem (e.g. Tenders Info, Tenderio, Doffin, Mercell) whose data could be licensed?
- **Change the use case:** If only Layer 1 data is used, is there a different, valuable product — one that works *with* metadata rather than requiring specification content?
- **Change the market:** Does the same two-layer problem exist in other EU member states, or is Italy's fragmentation unusually severe?

---

*Decision note prepared for product backlog. Last updated: 2026-04-24.*
