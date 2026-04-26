# CEO Review — TenderMonitorHLTH1

> **Document type:** gStack /plan-ceo-review output  
> **Date:** 2026-04-26  
> **Scope:** Research folder review — post-pivot product plan  
> **Status:** Complete — hand to /plan-eng-review after Step 3

---

## 1. Problem clarity

Real and specific. The user case document nails it: a domain expert needs procurement intelligence but TED requires CPV fluency, SPARQL, and multilingual search — a 2–3 day task that should take 2 minutes. The pivot decision sharpened this further: TED is a notice board, not a spec repository. The two-layer problem is clearly articulated. Problem is real.

---

## 2. User specificity

Well-defined: a solo healthcare IT strategy analyst with domain knowledge and no procurement or technical expertise. The user case document reinforces this. The post-pivot direction has not drifted from this definition.

---

## 3. Scope assessment — hold scope

The post-pivot plan is correctly scoped. The original Italy+Germany static CSV tool was too narrow in ambition and too closed in architecture. The pivot to Estonia (primary) + Netherlands (secondary) with a live API pipeline is the right call. The Phase 1/2/3 framework in `ted-data-architecture.md` is coherent: metadata first, specification intelligence second.

One risk: **Step 3 (Estonia technical validation) is still open.** The plan correctly gates Step 4 on Step 3. That dependency must not slip.

---

## 4. 10-star vision

The product a healthcare IT BD professional would rave about: type "remote cardiac monitoring" in English, get a ranked list of every Estonian and Dutch public tender from the last 3 years — translated, with winner names, contract values, evaluation criteria weights, and a one-paragraph summary of what each buyer technically required. Filter by buyer type, sort by value, export to CSV. New matching tenders arrive weekly by email. Today that is a 3-day research job. This makes it a 3-minute job.

---

## 5. Wedge strategy

Estonia first. Specifically: **metadata layer only** (Layer 1 — Phase 1 in the architecture doc). Prove the API can be ingested, winner names and contract values surfaced in a clean UI, and a user can answer “who won HIT contracts in Estonia in 2024 and at what value” without touching the TED portal. That is the wedge. It is buildable, demonstrably useful, and validates the pipeline before committing to the harder LLM extraction work.

---

## 6. Competitive reality

The research brief ruled out licensing from Mercell, Tenders Info, and Tenderio. The reason is unstated — presumably cost, data rights, or strategic control. If LLM extraction on Estonian documents fails in Step 3, this decision needs revisiting. Commercial data providers are not a competitor here — they are a potential fallback, and that option should stay open.

The direct competitive gap is real: no tool exists that serves a non-technical healthcare domain expert with plain-English query, auto-translated results, and CPV-invisible UX.

---

## 7. Business model

Not defined anywhere in the repository. The user case document, PLAN.md, and all research treat this as a personal tool. If it remains personal, no model is needed. If it becomes a product, the natural model is analyst-seat subscription or per-market access tiers. Worth noting now so it does not become a hidden assumption later.

---

## 8. Kill assumptions

Three assumptions that, if wrong, make this product worthless:

**Estonian spec documents are LLM-parseable in Estonian.** The research flags this as the primary language risk. If extraction quality is poor, Phase 2 collapses and the product is a better RSS feed.

**50–150 relevant healthcare IT tenders/year in Estonia is enough corpus.** Estonia has 1.3 million people. If the actual count of relevant tenders is 20–30, there is not enough volume for pattern analysis to be meaningful. Step 3 must confirm this number.

**The user will tolerate original-language titles in v1.** Translation is listed as a future feature in the post-pivot plan, not a v1 requirement. If results arrive in Estonian with no translation, usability drops sharply. This assumption needs to be made explicit in Step 4.

---

## 9. Red flags

**Volume is unresolved.** Denmark's API was down during validation. Estonia's volume is estimated, not confirmed. The plan sequences this correctly, but it is still open.

**No defined output format.** The research phase produced clean documents. The architecture phase (Step 4) has not run. There is no wireframe, data model, or UI spec. Step 4 needs to be time-boxed or it will sprawl.

**Language handling is deferred, not decided.** Machine translation of Estonian procurement documents is non-trivial. The plan treats it as a conditional patch if LLM extraction degrades. It should be a first-class architectural decision in Step 4.

---

## 10. Approved plan

Run Estonia technical validation (Step 3) against the acceptance criteria in `2026-04-post-pivot-action-plan.md`. If Layer 1 and Layer 2 pass, proceed to Step 4 with this scope: a single-market (Estonia) metadata-only pipeline that ingests the public API, stores winner name, contract value, buyer, CPV, and date, and serves a browser UI with keyword search and CSV export — no LLM extraction in v1.

**Success metric for v1:** a user can answer “who won healthcare IT contracts in Estonia in 2024, at what value, and from which buyer” in under 2 minutes without opening the riigihanked portal.

---

## Next action

Step 3 Estonia validation is the gate. Everything else waits on that. Once Step 3 closes, hand this output to `/plan-eng-review` to lock in architecture.
