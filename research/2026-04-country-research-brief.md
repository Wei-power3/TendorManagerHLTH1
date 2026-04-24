# Country Research Brief: EU Procurement Data Accessibility

> **Document type:** Analyst handover — research brief  
> **Author:** Weiche Lin  
> **Date:** 2026-04-24  
> **For:** Data analyst  
> **Status:** Active — research task

---

## Why This Brief Exists

This project is building a tender intelligence tool for healthcare IT procurement. The original plan targeted Italy and Germany. Italy has been ruled out as the primary market due to a data access problem (see context below). Germany's fragmentation is similar.

Your task is to identify which EU member states have procurement infrastructure that makes this product viable without building a custom data collection pipeline.

---

## Context: The Two-Layer Problem

Every EU public tender has two layers of data:

**Layer 1 — The Notice (always public)**
Published on TED (Tenders Electronic Daily). Structured, API-accessible, machine-readable. Contains: buyer name, CPV category, estimated value, award outcome, high-level description, and — for post-October 2023 eForms notices — award criteria weights.

**Layer 2 — The Specification Documents (the hard part)**
The actual tender documents: technical specification, evaluation methodology, scoring rubric, Q&A. These live on the buyer's own platform or a national/regional procurement portal. TED only links to wherever the buyer chose to host them.

**The Italy problem:** Italy's Layer 2 is fragmented across dozens of regional platforms (SINTEL, STELLA, TUTTOGARE, MePA, and individual buyer websites). None have public APIs. Most require registered procurement-authority accounts to access document packages. This makes automated retrieval infeasible at the current stage.

**The hypothesis for this research:** Some EU member states have invested in centralised, open, machine-accessible procurement infrastructure that solves or significantly reduces the Layer 2 problem. Your job is to find them.

---

## Research Question

> Which EU member states operate a national procurement portal where:
> 1. tender specification documents are publicly accessible without a registered bidder account, and
> 2. document access is programmable (API, structured download, or at minimum consistent URL patterns)?

---

## Evaluation Criteria

Score each country against these dimensions. A country is **viable** if it scores well on criteria 1–4. Criteria 5–6 are tie-breakers.

### Criterion 1 — Centralisation
Is there a single national procurement portal, or is the market fragmented across regional/sectoral platforms?

- **Good:** One national portal handles the majority of public tenders
- **Bad:** Dozens of platforms, no single access point (Italy, Germany)

### Criterion 2 — Public document access
Can specification documents be downloaded without a registered bidder account?

- **Good:** Documents are publicly available (open by default)
- **Acceptable:** Registration is open to any organisation, not restricted to certified procurement authorities
- **Bad:** Login requires procurement-authority credentials or is otherwise restricted

### Criterion 3 — Programmatic access
Is there an API, a bulk data download, or at minimum a predictable URL structure for accessing documents?

- **Good:** REST API or SPARQL endpoint returning document URLs or document content
- **Acceptable:** Bulk download (ZIP/XML) updated on a regular schedule
- **Bad:** Documents only accessible via interactive web forms or CAPTCHA-protected pages

### Criterion 4 — Healthcare IT procurement volume
Is healthcare IT procurement represented in meaningful volume on this platform?

- Proxy: search for CPV codes 72xxxxxx (IT services), 48xxxxxx (software), 85xxxxxx (health services), 33xxxxxx (medical equipment) on the platform
- A rough threshold: at least 50–100 relevant tenders per year to make analysis meaningful

### Criterion 5 — Language
What language are specification documents written in?

- **Best:** English (UK, Ireland)
- **Good:** Germanic or Nordic languages (German, Dutch, Swedish, Danish, Norwegian, Finnish) — LLM parsing is reliable
- **Harder:** Romance languages (French, Portuguese, Spanish, Romanian) — still manageable
- Note: Italian is not ruled out as a language, only as a *platform*

### Criterion 6 — eForms adoption rate
What share of notices are published in the eForms standard (mandatory EU-wide since October 2023)?

- Higher eForms adoption = more structured Layer 1 data = less cleanup work
- Check: TED statistics by country, or the national portal's own documentation

---

## Known Leads — Start Here

These countries have a reputation for strong digital procurement infrastructure. Investigate them first.

| Country | National portal | Why it's a lead |
|---|---|---|
| **Netherlands** | TenderNed | Frequently cited as one of the most open EU procurement portals; API documented publicly |
| **Denmark** | udbud.dk | Centralised national portal; Denmark has high digital government maturity |
| **Estonia** | e-Procurement (riigihange.ee) | Consistently ranked top in EU digital government indices; national e-procurement since early 2000s |
| **Finland** | HILMA (hankintailmoitukset.fi) | Single national portal; open data orientation |
| **Sweden** | Visma TendSign / national portals | Mixed — investigate whether there is a true single national platform |
| **Portugal** | BASE.gov.pt | Reportedly open award data; investigate whether specs are accessible |
| **Austria** | Bundesvergabeamt / e-Vergabe | Less certain — include as a check |

---

## What to Deliver

For each country investigated, produce a one-page summary with the following structure:

```
Country: [name]
National portal: [URL]
Centralised? [Yes / Partial / No — brief explanation]
Document access: [Open / Registration required (open registration) / Restricted]
Programmatic access: [API / Bulk download / URL pattern / None found]
API documentation URL: [if exists]
Healthcare IT volume: [rough count of relevant CPV notices per year, or "not assessed"]
Language: [language of documents]
eForms adoption: [% or "not found"]
Verdict: [Viable / Possibly viable / Not viable]
Recommendation: [1–2 sentences]
```

Prioritise the leads table above. If any of those countries are clearly viable, a full sweep of all EU states is not necessary — flag the viable candidates and stop.

---

## Out of Scope for This Research

- Non-EU countries (UK post-Brexit Find a Tender, Norway via EEA — note these separately if strong, but they are secondary)
- Commercial tender intelligence platforms (Mercell, Tenders Info, Tenderio) — licensing has been ruled out for now
- Any investigation of Italian or German regional platforms — these are already ruled out

---

## Reference Material

The following files in this repository provide background:

- [`2026-04-ted-data-architecture.md`](./2026-04-ted-data-architecture.md) — full explanation of the two-layer problem and what TED does and does not contain
- [`2026-04-pivot-decision.md`](./2026-04-pivot-decision.md) — why Italy was ruled out

---

*Brief prepared 2026-04-24. Expected turnaround: researcher's judgement.*
