# Market Selection Decision

> **Document type:** Founder decision record  
> **Author:** Weiche Lin  
> **Date:** 2026-04-26  
> **Status:** Final — research phase closed  
> **Follows from:** `plan/2026-04-post-pivot-action-plan.md` Step 2

---

## Decision

**Estonia is the primary market. The Netherlands is the second market. Denmark is parked.**

This document closes the research phase. Anyone joining this project can read this document alone and understand which markets were selected, in what order, and why.

Architecture, product scope, and implementation are not decided here. Those are Step 4.

---

## Market Selection

| Market | Status | Priority |
|---|---|---|
| Estonia | Selected — primary | 1 |
| Netherlands | Selected — second | 2 |
| Denmark | Parked — viable infrastructure, volume unconfirmed | 3 (conditional) |
| France | Ruled out | — |
| Italy | Ruled out | — |
| Germany | Ruled out | — |

---

## What "Proceed" Means for Each Market

### Estonia — Primary

Estonia is the first build target. The next action is Step 3: technical validation to confirm that what the research found works in practice.

"Proceed" means:
- Run the technical validation defined in `plan/2026-04-post-pivot-action-plan.md` Step 3
- If Step 3 passes, Estonia is the primary market for v1 architecture and build
- If Step 3 fails (API gated, documents not retrievable), escalate immediately — Netherlands becomes the primary validation candidate

### Netherlands — Second Market

The Netherlands is ready to enter the pipeline once the Estonia layer is established. No further research is required.

"Proceed" means:
- Begin Netherlands data access once the v1 pipeline runs on Estonia
- TenderNed document retrieval is confirmed open without authentication for public tenders; registration (one-time, free, open internationally) is only required for bid submission, which is out of scope

### Denmark — Parked

Denmark's Layer 2 infrastructure is confirmed viable. The volume question is the sole remaining open item, blocked by a transient API outage.

"Proceed" means:
- No action now
- Re-entry trigger: `api.udbud.dk` service recovers → retry the four CPV queries (72, 48, 85, 33) for 2023 and 2024 → if combined annual count ≥50, Denmark is viable and folds into v1.5 or earlier if the count comes back before Step 4 is complete
- If the count comes back before Step 4 is complete, fold Denmark into the architecture decision as a potential parallel market

---

## Rationale

### Why Estonia — Primary

**Source:** `research/2026-04-eu-procurement-country-viability-assessment.md`, `research/2026-04-gap-resolution-france-assessment.md`

Estonia solves both layers of the two-layer problem:

- **Layer 1 (notices):** Public REST API at `riigihanked.riik.ee/rhr/api/public/v1/notice` — no authentication, machine-readable, stable public endpoints
- **Layer 2 (specification documents):** Registration is required for bid submission, not for reading notices and documents. This is the critical distinction that rules out Italy, France, and Germany

Volume confirmation (`research/2026-04-gap-resolution-france-assessment.md`, Gap 1): 8,975 procurement processes in Estonia in 2023; €5.8 billion total value; major healthcare buyers (North Estonia Medical Centre — 1,241 notices, €604M; Tartu University Hospital — 779 notices, €452M; Eesti Haigekassa — 1,091 notices, €547M) are among the top-10 buyers by notice volume. IT and digital services are Estonia's signature procurement sector. Estimated 50–150 relevant above-threshold healthcare IT tenders per year — sufficient for a v1 corpus.

The only assessed downside is language: Estonian is a Finno-Ugric language unlike all Western European languages. LLM parsing quality on Estonian procurement documents is flagged as a risk to test in Step 3, not a reason to rule the market out.

### Why Netherlands — Second

**Source:** `research/2026-04-eu-procurement-country-viability-assessment.md`, `research/2026-04-gap-resolution-france-assessment.md`

The Netherlands has the most technically mature procurement system assessed:

- **Layer 1:** TED + TenderNed API, OCDS-compliant, daily/monthly structured updates
- **Layer 2:** TenderNed public document endpoint (`https://www.tenderned.nl/aankondigingen/overzicht/{id}/documenten`) serves specification documents without authentication for public tenders

Registration gap resolved (`research/2026-04-gap-resolution-france-assessment.md`, Gap 2): TenderNed registration is explicitly open to foreign businesses via a username/password path with no Dutch KvK number required. Registration is only required for bid submission, which is out of scope. Document access is open without any account.

The Netherlands sits behind Estonia in priority only because Estonia's infrastructure is cleaner (no registration at any level) and Estonia was identified as the best starting point before the Netherlands validation was confirmed. Both markets are viable; the sequencing is a resourcing choice, not a quality distinction.

### Why Denmark — Parked, Not Ruled Out

**Source:** `research/2026-04-gap-resolution-france-assessment.md`, `research/2026-04-germany-denmark-clarification-response.md`

Denmark's Layer 2 access is confirmed: specification documents are served publicly at `https://api.udbud.dk/udbud/vedhaeftning/{attachment_id}/open/{filename}` without authentication. Validated across multiple buyers (Region Sjælland, Aarhus Kommune, Region Nordjylland) and notice types. The udbud.dk portal is operated by Erhvervsstyrelsen (Danish Business Authority) — a government-operated national system, structurally strong for long-term availability.

The sole unresolved item is healthcare IT volume. The CPV-filtered count (CPV 72, 48, 85, 33 for 2023–2024) could not be retrieved because `api.udbud.dk` returned HTTP 503 across all endpoints on 2026-04-26 (~09:00–11:00 CEST). The outage is consistent with a transient maintenance window, not permanent deprecation.

Denmark is parked, not cancelled. The infrastructure is sound. The volume question is one API query away from resolution.

### Why France — Ruled Out

**Source:** `research/2026-04-gap-resolution-france-assessment.md`

BOAMP (France's national procurement journal) has a public API but does not return document download links. Specification packages (DCE — Dossier de Consultation des Entreprises) live on the buyer's chosen *profil d'acheteur* — PLACE for central government, and 100+ regional and commercial platforms for local authorities and hospitals. All major platforms are registration-gated.

RESAH and UGAP reduce buyer-side fragmentation but do not create a centralised, open document access point. The Layer 2 problem in France is structurally identical to Italy.

France was assessed as the second-largest EU procurement market by volume and was given a targeted check precisely because of that market size. The check confirmed it is not viable for the current product scope.

### Why Italy — Ruled Out

**Source:** `research/2026-04-pivot-decision.md`, `research/2026-04-ted-data-architecture.md`

Italy was the original v1 target market. The pivot decision was triggered by the Layer 2 finding: specification documents in Italy are distributed across SINTEL, STELLA, TUTTOGARE, MePA, and hundreds of individual buyer websites. None of the major regional platforms expose public programmatic access. Most require procurement-authority credentials. There is no single access point covering the market.

The v1 plan (static TED CSV → data.json → browser query interface) was suspended on this basis.

### Why Germany — Ruled Out

**Source:** `research/2026-04-assessment-review-and-next-steps.md`, `research/2026-04-germany-denmark-clarification-response.md`

Germany has no single mandatory national procurement portal. Tenders are distributed across DTVP, Vergabe24, evergabe.de, service.bund.de, and 16 independent Länder-level systems. This is the same structural failure as Italy, without Italy's partial mitigant of a national tender ID (CIG). The country was explicitly excluded from the research brief on this basis; a follow-up check confirmed the ruling.

One exception was noted: evergabe.de has a credible legal and technical basis for registration-free Layer 2 access under §9(3) VgV (Vergabeverordnung), but it covers only a subset of German buyers and was not tested within the research sprint. This is documented in `research/2026-04-germany-denmark-clarification-response.md` for any future Germany-specific sprint. It does not change the current ruling.

---

## Research Trail

Eight documents produced this decision. Listed in sequence:

| Document | Role in this decision |
|---|---|
| `research/2026-04-pivot-decision.md` | v1 plan suspended; Italy ruled out; four open questions posed |
| `research/2026-04-ted-data-architecture.md` | Two-layer problem defined; Phase 1/2/3 framework for data pipeline |
| `research/2026-04-country-research-brief.md` | Analyst brief commissioning the EU country viability sweep |
| `research/2026-04-eu-procurement-country-viability-assessment.md` | 7-country assessment; Estonia, Netherlands, Denmark identified as viable candidates |
| `research/2026-04-assessment-review-and-next-steps.md` | Founder review; three gaps in the assessment identified; France added to scope |
| `research/2026-04-gap-resolution-france-assessment.md` | Gaps 1–3 resolved (Estonia volume, NL registration, Denmark URL pattern); France ruled out |
| `research/2026-04-germany-denmark-clarification-brief.md` | Clarification brief: Germany unvalidated entry in summary table; Denmark volume unconfirmed |
| `research/2026-04-germany-denmark-clarification-response.md` | Germany ruled out; Denmark volume blocked by api.udbud.dk outage; retry instructions documented |

---

## What Is Not Decided Here

- **Architecture** — what to build for v1, what the data pipeline looks like, technology choices (Step 4)
- **Product scope** — Phase 1 vs Phase 2 in v1 (Step 4, depends on Step 3 Estonia validation)
- **Implementation order** — the sequenced build task list (Step 4)
- **Denmark final status** — parked until volume count is retrieved

---

*Decision record issued 2026-04-26. Research phase closed. Next action: Step 3 Estonia technical validation.*
