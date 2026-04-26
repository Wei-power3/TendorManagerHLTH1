# Post-Pivot Action Plan

> **Document type:** Active plan  
> **Author:** Weiche Lin  
> **Date:** 2026-04-26  
> **Status:** Active  
> **Replaces:** `PLAN.md` v1 (suspended — see `research/2026-04-pivot-decision.md`)

---

## Where We Are

The research phase is complete. Six research documents have produced the following confirmed state:

| Country | Verdict | Source |
|---|---|---|
| Estonia | Viable — primary market | `2026-04-gap-resolution-france-assessment.md` |
| Netherlands | Viable — second market | `2026-04-gap-resolution-france-assessment.md` |
| Denmark | Parked — Layer 2 access confirmed, volume count blocked by API outage | `2026-04-germany-denmark-clarification-response.md` |
| France | Ruled out | `2026-04-gap-resolution-france-assessment.md` |
| Italy | Ruled out | `2026-04-pivot-decision.md` |
| Germany | Ruled out | `2026-04-germany-denmark-clarification-response.md` |

Three actions remain before the build can restart:

1. **Write the market selection decision document** — close out the research phase with a single founder decision record
2. **Run technical validation on Estonia** — confirm the research findings work in practice before committing to the market
3. **Write the product architecture decision** — define what to build, based on validation results

Denmark is parked. Re-entry trigger: retry `api.udbud.dk` CPV queries when the service recovers. If the volume count comes back before Step 3 is complete, fold it into the architecture decision. If not, proceed without it — Denmark is a second-phase market at earliest.

---

## Step 2 — Market Selection Decision Document

### What this is

A single founder decision record that closes out the research phase. Anyone joining the project should be able to read this one document and understand which markets were selected, in what order, and why — without tracing back through all six research documents.

### Owner

Founder decision. Written by Claude based on consolidated research; approved and committed by founder.

### Tasks

| # | Task | Detail |
|---|---|---|
| 2.1 | Synthesise research trail | Review all six research documents and extract the confirmed facts, ruled-out options, and open conditions into a single structured summary |
| 2.2 | State the market selection | Estonia = primary; Netherlands = second; Denmark = conditional/parked. Define what "proceed" means concretely for each |
| 2.3 | Document the rationale | Trace each decision to its source document so the reasoning is auditable |
| 2.4 | Note what is not decided yet | Architecture, product scope, and implementation are out of scope for this document |
| 2.5 | Commit to `plan/` | File: `plan/2026-04-market-selection-decision.md` |

### Acceptance criteria

- [ ] Document states confirmed markets with priority order
- [ ] Each verdict is traced to a specific source document
- [ ] Denmark's parked status and re-entry trigger are recorded
- [ ] Document does not contain any architecture or build decisions — those are Step 4

---

## Step 3 — Technical Validation: Estonia

### What this is

The research confirmed Estonia's *infrastructure* is viable in principle. This step confirms it works in practice for this specific use case. It is the last gate before committing Estonia as the primary build target.

Two things need to be true for Estonia to pass:

1. **Layer 1 works:** The `riigihanked.riik.ee` public API returns healthcare IT notices queryable by CPV code without authentication
2. **Layer 2 works:** Specification documents linked from those notices are retrievable without authentication and contain useful content

A third thing needs to be assessed, not just assumed:

3. **Estonian-language documents are LLM-parseable:** The research flagged language as a risk. Estonian is a Finno-Ugric language, structurally unlike all Western European languages. If procurement specification documents are in Estonian only with no English, LLM extraction quality needs to be tested before building a pipeline that depends on it.

### Owner

Agentic AI researcher (technical validation sprint). Deliverable is a validation report committed to `plan/`.

### Tasks

**Layer 1 — API access**

| # | Task | Detail |
|---|---|---|
| 3.1 | Confirm public API access | Hit `https://riigihanked.riik.ee/rhr/api/public/v1/notice` without authentication. Confirm 200 response. |
| 3.2 | CPV-filtered query | Query for healthcare IT notices using CPV codes: `72` (IT services), `48` (software), `85` (health services), `33` (medical equipment). Retrieve the first page of results for 2024. |
| 3.3 | Spot-check volume | Count results per CPV division and total for 2024. Confirm the research estimate of 50–150 relevant above-threshold notices per year is plausible. |
| 3.4 | Record API response structure | Document the JSON field names for: notice ID, buyer name, CPV code, estimated value, publication date, and document attachment links. These are the fields the pipeline will depend on. |

**Layer 2 — Document access**

| # | Task | Detail |
|---|---|---|
| 3.5 | Identify document attachment URLs | From 5 of the notices retrieved in 3.2, extract the linked specification document URLs. |
| 3.6 | Retrieve documents without authentication | Download each document. Confirm no authentication header, cookie, or session token is required. Record the file format (PDF, DOCX, ZIP, etc.). |
| 3.7 | Assess document completeness | For each retrieved document, confirm whether it contains: (a) technical requirements, (b) evaluation criteria, (c) scoring weights. A document that contains all three is a full specification package. Note any notices where only partial documents are attached. |

**Language and LLM parseability**

| # | Task | Detail |
|---|---|---|
| 3.8 | Language assessment | Across the 5 retrieved documents: what language are they in? Estonian only, bilingual (Estonian + English), or other? Record the distribution. |
| 3.9 | LLM extraction test | For 2 documents (choose the most representative — one large hospital buyer, one IT/software buyer), run a structured extraction prompt against Claude. The prompt should ask for: the top 3 technical requirements, the evaluation criterion names and weights, and the minimum qualification criteria. |
| 3.10 | Assess extraction quality | Rate the output on three dimensions: (a) factually correct — does it match the source document? (b) structurally complete — did it find all three field types? (c) language handling — did Estonian text cause hallucination, confusion, or refusal? |

### Acceptance criteria

- [ ] Layer 1: API confirmed accessible, CPV query works, response structure documented
- [ ] Layer 2: At least 5 specification documents retrieved without authentication
- [ ] Volume: plausible estimate confirmed (or revised with new count)
- [ ] Language: distribution assessed across the 5 documents
- [ ] LLM extraction: 2 documents tested, quality rated on factual accuracy / completeness / language handling
- [ ] Report committed to `plan/2026-04-estonia-validation-report.md`

### Pass / Conditional / Fail criteria

| Outcome | Condition | Consequence |
|---|---|---|
| **Pass** | Layer 1 works, Layer 2 works, LLM extraction rated good or acceptable on both test documents | Proceed to Step 4 with Estonia as primary market |
| **Conditional** | Layer 1 and 2 work, but LLM extraction quality is degraded by Estonian language (partial extraction, occasional errors) | Proceed to Step 4 but architecture must account for a language handling layer (translation or bilingual prompt strategy) |
| **Fail** | Layer 1 or Layer 2 does not work as documented (API gated, documents not retrievable) | Escalate to founder. Netherlands becomes primary market candidate. Re-run Layer 2 document retrieval test on TenderNed. |

---

## Step 4 — Product Architecture Decision

### What this is

Based on the Estonia validation results, define what to build for v1. This replaces the suspended `PLAN.md`. It answers: what is the product scope, what does the data pipeline look like, and what is the definition of done?

This step happens *after* Step 3. The architecture depends on what the validation revealed — specifically whether LLM extraction works cleanly on Estonian documents (which affects whether v1 includes specification intelligence or starts with metadata only).

### Owner

Founder decision. Written by Claude based on validation results; approved and committed by founder.

### Decision inputs

| Input | Source |
|---|---|
| Estonia validation report | Step 3 output |
| Denmark volume count (if available) | Parked — include if ready before this step |
| Original product hypothesis | `research/2026-04-ted-data-architecture.md` — Phase 1/2/3 framework |
| Original v1 scope | `PLAN.md` (suspended) — reference for what was originally intended |

### Tasks

| # | Task | Detail |
|---|---|---|
| 4.1 | Decide Phase 1 vs Phase 2 scope for v1 | Phase 1 = metadata + award outcomes only (who won, what value, which buyer). Phase 2 = metadata + specification document intelligence (what was required, how it was scored). Decision depends on LLM extraction quality from Step 3. |
| 4.2 | Define the data pipeline | Document the end-to-end data flow: source APIs → ingestion → storage → serving. Specify technology choices. |
| 4.3 | Define the user interface | What does the user actually interact with? What question does the product answer? |
| 4.4 | Write definition of done | List the specific acceptance criteria that make v1 complete — same format as the original `PLAN.md`. |
| 4.5 | Write implementation order | Sequenced task list. Step 1 of the build must be executable immediately after this document is committed. |
| 4.6 | Commit as new `PLAN.md` | Overwrite the suspended plan. Add a header noting it supersedes the April 2026 v1 plan. |

### Acceptance criteria

- [ ] Scope decision made: Phase 1 only, or Phase 1 + Phase 2 in v1
- [ ] Primary market confirmed: Estonia (or Netherlands if Estonia failed Step 3)
- [ ] Data pipeline documented end-to-end with technology choices
- [ ] Definition of done is a checklist, not prose
- [ ] Implementation order starts with a task that can be executed immediately
- [ ] New `PLAN.md` committed and the suspended v1 plan is superseded

---

## Denmark: Parked

Denmark's Layer 2 access infrastructure is confirmed viable. The volume count is the one remaining open item, blocked by a transient API outage on `api.udbud.dk`.

Denmark is not cancelled — it is deferred. Re-entry conditions:

- **Trigger:** `api.udbud.dk` service recovers and CPV-filtered notice counts for 2023–2024 can be retrieved
- **Task:** Run the four CPV queries from the clarification brief (`2026-04-germany-denmark-clarification-brief.md`, Task 2 retry instructions). Deliver count and viability verdict.
- **Integration point:** If the count comes back before Step 4 is complete, fold it into the architecture decision as a potential second market alongside the Netherlands. If it comes back after Step 4, treat it as a v1.5 market expansion.

No action required now. The retry runs when the API recovers.

---

## Sequence and Dependencies

```
[Now]
  │
  ├─► Step 2 — Market Selection Decision   (no dependencies — write now)
  │         │
  │         └─► plan/2026-04-market-selection-decision.md
  │
  ├─► Step 3 — Estonia Technical Validation   (can run in parallel with Step 2)
  │         │
  │         └─► plan/2026-04-estonia-validation-report.md
  │
  │   [Denmark retry — async, no dependency, feeds into Step 4 if ready in time]
  │
  └─► Step 4 — Product Architecture Decision   (requires Step 3 output)
            │
            └─► PLAN.md (new, supersedes suspended v1)

```

Steps 2 and 3 are independent and can run in parallel. Step 4 requires Step 3 to be complete.

---

## Research Reference Index

All research documents are in `research/`. Listed in order of production:

| Document | Summary |
|---|---|
| `2026-04-pivot-decision.md` | v1 plan suspended; Italy ruled out; four open questions |
| `2026-04-ted-data-architecture.md` | Two-layer problem defined; Phase 1/2/3 framework |
| `2026-04-country-research-brief.md` | Analyst brief for EU country viability sweep |
| `2026-04-eu-procurement-country-viability-assessment.md` | 7-country assessment; Estonia, NL, Denmark viable |
| `2026-04-assessment-review-and-next-steps.md` | Founder review; 3 gaps identified |
| `2026-04-gap-resolution-france-assessment.md` | Gaps 1–3 resolved; France ruled out |
| `2026-04-germany-denmark-clarification-brief.md` | Clarification brief for Germany rating and Denmark volume |
| `2026-04-germany-denmark-clarification-response.md` | Germany ruled out; Denmark volume blocked by API outage |

---

*Plan issued 2026-04-26. Active until superseded by new `PLAN.md` at end of Step 4.*
