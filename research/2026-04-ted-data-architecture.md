# TED Tender Data Architecture: What It Is, What It Isn't, and Where the Value Lives

> **Document type:** Research summary & product development user story  
> **Author:** Weiche Lin  
> **Date:** 2026-04-23  
> **Intended audience:** Product Manager — Tender Monitor system  
> **Status:** Draft for GitHub product backlog

---

## Executive Summary

After hands-on exploration of the TED (Tenders Electronic Daily) database, SPARQL API, and eForms REST API, a critical architectural reality has emerged: **TED is a public notice board, not a tender specification repository.** The high-value intelligence that suppliers and market analysts actually need — technical requirements, evaluation rubrics, winning criteria — lives in a fragmented ecosystem of buyer-owned platforms and PDF documents that TED only loosely references.

This document captures that learning as a foundation for product design decisions in the Tender Monitor system.

---

## 1. What TED Actually Is

### The Legal Purpose of TED

TED exists solely to fulfill the EU's **public procurement transparency obligation**. Above-threshold public contracts in EU member states must be announced in TED before the tender process begins. The publication is a legal formality — a notice that a procurement opportunity exists — not a complete specification of what is being bought.

Think of TED as a **legal gazette or public billboard**: it tells you something is happening and points you toward where to learn more. It was never designed to host the full tender documentation.

### What TED Stores (The Notice Layer)

TED's SPARQL endpoint and REST API expose **structured metadata fields** extracted from the notice:

| Field | Description |
|---|---|
| Publication number | Unique TED identifier (e.g., `822442-2025`) |
| Date | Publication date |
| Buyer name | Contracting authority (e.g., ASST Papa Giovanni XXIII) |
| CPV code / label | Product/service category (e.g., "Telemetry surveillance system") |
| Procedure type | Open, restricted, negotiated, etc. |
| Estimated value | Contract value (when declared) |
| Contract duration | Length and renewal options |
| High-level description | 1–3 paragraph summary of the procurement object |
| Award criteria names + weights | e.g., "Quality 60% / Price 40%" — available in eForms notices |
| Links to procurement documents | URL pointing to the buyer's own platform or website |

The **eForms standard** (mandatory since October 2023 for above-threshold EU notices) improved this significantly. Award criteria weightings, lot-level descriptions, and selection criteria are now structured machine-readable fields in the TED API — not free text buried in attachments.

---

## 2. What TED Does Not Store

The full tender specification — the documents that define exactly what a buyer wants and how bids will be scored — is **never uploaded to TED**. The buyer decides where to host these documents. TED only provides a link to wherever that is.

Documents that live outside TED:

| Document | Italian term | Contains |
|---|---|---|
| Technical specification | Capitolato tecnico | Exact product/service requirements, technical standards, minimum specs |
| Tender rules document | Disciplinare di gara | Full evaluation methodology, scoring rubric, submission rules |
| Technical annexes | Allegati tecnici | Quantities, lot breakdowns, integration requirements |
| Supplier Q&A | Chiarimenti | Buyer answers to supplier questions during the tender period |
| Award decision detail | Verbale di aggiudicazione | Score breakdown per evaluation criterion, winning bid details |

---

## 3. The Two-Layer Data Problem

This creates a fundamental two-layer architecture for any tender intelligence system:

```
LAYER 1: TED / ANAC
─────────────────────────────────────────────────────────
Structured · Open · API-accessible · Machine-readable

→ Tells you WHAT exists and WHO is buying
→ Solved with standard API calls (TED SPARQL, TED REST, ANAC API)
→ Available for all above-threshold EU notices

LAYER 2: Buyer Platforms / Documents
─────────────────────────────────────────────────────────
Unstructured · Fragmented · Often login-gated · PDF-heavy

→ Tells you WHAT they actually want and HOW to win
→ No systematic API access — fragmented across dozens of platforms
→ The unsolved, high-value problem

         ▲
         │
         │  THE GAP = WHERE PRODUCT MOAT LIVES
         │
```

### Where the Documents Live (Italian Context)

Italian healthcare tenders are hosted across multiple regional and national platforms. None of these offer public programmatic access:

| Platform | Region / Scope | Access |
|---|---|---|
| SINTEL (ARIA Lombardia) | Lombardia | Login required |
| STELLA | Various regions | Login required |
| TUTTOGARE | National | Login required |
| MePA (Consip) | National | Login required |
| Buyer's own website | Varies | Public, but unstructured |
| ANAC / BDNCP | National | Open — award results only |

---

## 4. The API Access Reality

### TED APIs Available

| API | Endpoint | What it returns |
|---|---|---|
| SPARQL endpoint | `https://ted.europa.eu/api/sparql` | Structured query across all notices |
| REST notice API | `GET /api/v3.0/notices/{id}` | Full eForms JSON/XML for a single notice |
| Viewer (HTML) | `https://ted.europa.eu/en/notice/-/detail/{id}` | Human-readable rendering |

### Key eForms Fields (Available via REST API)

| eForms Field Code | Content |
|---|---|
| `BT-24` | Full description text |
| `BT-23` | Lot description |
| `BT-27` | Estimated value |
| `BT-540` | Award criterion name |
| `BT-541` | Award criterion weighting (%) |
| `BT-747` | Selection criteria description |

For **post-October 2023 notices**, a single API call to `/api/v3.0/notices/{id}` returns all of the above as structured JSON. This covers ~70% of the intelligence value without touching any external platform.

### The CIG: The Linking Key

The **CIG** (Codice Identificativo Gara) is Italy's national tender ID assigned by ANAC. It is the bridge between TED (EU layer) and all Italian national platforms. Once retrieved from the TED eForms JSON, the CIG can be used to:

- Query **ANAC** for the award decision and winner
- Search **SINTEL / regional platforms** for the tender document package
- Track contract execution data

---

## 5. Practical Implication for Tender Monitor

### Data Pipeline Architecture

```
Phase 1 — Discovery (automatable today)
  TED SPARQL → nightly sweep → identify relevant notices by CPV/keyword
        ↓
Phase 2 — Enrichment (automatable today)
  TED REST API /notices/{id} → fetch eForms JSON
  → extract: description, criteria weights, lot values, duration
        ↓
  ANAC API → fetch award result by CIG
  → extract: winner, awarded value, date
        ↓
Phase 3 — Specification retrieval (the hard problem)
  Platform document fetch → PDF capitolato
  → extract via LLM: technical specs, minimum requirements, scoring rubric
```

Phases 1 and 2 are **fully automatable with public APIs** today.  
Phase 3 is the unsolved, differentiated layer.

### Why Specification Content Is the Moat

Without tender specification content, the product is essentially a **better RSS feed** — useful for awareness, but not defensible. With specification content parsed and structured, the product becomes genuinely differentiated:

- **For suppliers:** "Here are 12 tenders this month where you meet ≥80% of technical requirements"
- **For market intelligence:** "Philips is specified by name in 34% of Italian cardiac monitoring tenders"
- **For strategic positioning:** "Lombardia consistently requires FHIR R4 — prioritise that integration"

### Recommended Phase Sequencing

| Phase | Focus | Goal |
|---|---|---|
| **Phase 1 — Now** | TED metadata + ANAC awards | Prove the market segment exists |
| **Phase 2 — Next** | Manual spec ingestion for pilot vertical (cardiac monitoring, ~20–30 tenders/month in Italy) | Prove the value of specification intelligence |
| **Phase 3 — Scale** | Automate spec retrieval for proven verticals | Build the moat |

Skipping to Phase 3 before validating Phase 2 is the classic data product mistake: building an expensive pipeline before confirming willingness to pay for the output.

---

## 6. User Story: Tender Intelligence Search

**As a** market intelligence analyst tracking Italian healthcare procurement,  
**I want to** search and filter tenders not just by metadata (buyer, CPV, value) but by the actual technical content of the specification,  
**So that** I can identify tenders where our product meets the requirements, and understand how buyers are defining their needs in this market.

### Acceptance Criteria

- [ ] System ingests TED notices daily via SPARQL and eForms REST API
- [ ] System stores structured metadata (buyer, CPV, value, criteria weights, description) per notice
- [ ] System retrieves and links ANAC CIG and award data for completed tenders
- [ ] System provides a mechanism to attach and parse specification documents (PDF capitolato) against notices
- [ ] System extracts and indexes: technical requirements, minimum qualification criteria, evaluation scoring methodology from specification documents
- [ ] Search returns results ranked by specification relevance, not just keyword match on title

### Current Friction Points

1. **No specification content in TED** — the gap between notice and actual requirements is unbridgeable via API alone
2. **Fragmented document hosting** — each Italian region uses a different platform; no single access point
3. **Login-gated platforms** — SINTEL and equivalents require registered accounts, blocking automated retrieval
4. **PDF-heavy documents** — specifications are in Italian legal-register PDFs requiring OCR and domain-aware parsing
5. **No CIG-to-document mapping** — there is no public API linking a CIG directly to its document package

---

## 7. Key Terminology Reference

| Term | Definition |
|---|---|
| **TED** | Tenders Electronic Daily — EU's official public procurement journal |
| **eForms** | EU standard for structured electronic procurement notices (mandatory Oct 2023) |
| **CPV** | Common Procurement Vocabulary — EU product/service classification system |
| **CIG** | Codice Identificativo Gara — Italian national tender ID assigned by ANAC |
| **ANAC** | Autorità Nazionale Anticorruzione — Italian national anti-corruption authority; manages tender registry |
| **SINTEL** | Lombardia's regional e-procurement platform (managed by ARIA SpA) |
| **Capitolato tecnico** | Technical specification document — defines what the buyer actually wants |
| **Disciplinare di gara** | Tender rules document — defines evaluation methodology and submission requirements |
| **COT** | Centrale Operativa Territoriale — Italian territorial healthcare coordination hub (PNRR M6C1) |
| **CDC** | Casa di Comunità — Italian community health house (PNRR M6C1 infrastructure) |

---

*Document prepared for GitHub product backlog. Last updated: 2026-04-23.*
