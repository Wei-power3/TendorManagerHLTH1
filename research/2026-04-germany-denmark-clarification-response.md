# Germany and Denmark Clarification: Research Response

> **Document type:** Analyst research response  
> **Date:** 2026-04-26  
> **Responds to:** [`2026-04-germany-denmark-clarification-brief.md`](./2026-04-germany-denmark-clarification-brief.md)  
> **Status:** Task 1 resolved. Task 2 blocked by api.udbud.dk outage — retry required.

---

## Executive Summary

This document resolves the two open items from the Germany and Denmark clarification brief issued 2026-04-26.

- **Task 1 (Germany):** The unvalidated "Conditional" rating is **removed**. Germany is ruled out on the same structural fragmentation grounds as Italy and France. The investigation did surface one portal — evergabe.de — with a credible legal and platform basis for registration-free Layer 2 access, which is documented here for any future Germany-specific sprint. No tested document URL could be retrieved within this sprint.
- **Task 2 (Denmark):** The volume count **remains unresolved** due to a transient HTTP 503 outage on `api.udbud.dk` at query time (2026-04-26, ~09:00–11:00 CEST). The TED API v3 fallback covers only eForms-format notices (mandatory from October 2023), leaving the pre-eForms majority of 2023 notices unreachable via that route. Denmark's Layer 2 access infrastructure is confirmed viable. The volume condition requires a retry once the udbud.dk API service resumes.

---

## Task 1 — Germany: Verdict and Evidence

### Decision: Option B — Row Removed

The Germany row is removed from the country viability summary table. The "Conditional — Mixed (some open, some gated)" rating that appeared in the previous summary table is hereby retracted. It was not supported by a tested portal URL, a retrieved specification document, or a scope statement, and it cannot be supported within this sprint on the basis of investigation results below.

Germany remains ruled out on the same fragmentation grounds documented in the original research brief: procurement is distributed across DTVP, Vergabe24, evergabe.de, evergabe-online.de, deutsche-evergabe.de, service.bund.de, and 16 independent Länder-level systems, with no single mandatory national portal. This is structurally equivalent to Italy (ANAC ecosystem fragmentation) and France (BOAMP/PLACE/AWS fragmentation) — both of which were ruled out.

### German Portal Landscape: Investigation Findings

The investigation examined five portals in scope. Findings are recorded here for future reference.

#### evergabe.de

- **Operator:** Neuer Auftrag GmbH (private operator)
- **URL:** https://www.evergabe.de
- **Layer 2 access claim:** evergabe.de explicitly states in its FAQ that under §9(3) VgV (Vergabeverordnung), contracting authorities are legally required to provide cost-free, registration-free access to procurement documents for all above-threshold public procedures. Documents are available at `https://www.evergabe.de/unterlagen?vergabenummer={Vergabenummer}` without authentication.
- **Test result:** The `/unterlagen` endpoint is reachable and returns a page titled "Vergabeunterlagen ohne Registrierung" (procurement documents without registration), confirming the access model. No specific Vergabenummer was tested with a document retrieval this sprint due to scope constraints.
- **Buyer population covered:** Buyers who have voluntarily selected evergabe.de as their platform. This includes municipal and Länder-level buyers primarily in northern and western Germany, but coverage is not geographically bounded and has no Länder mandate.
- **Scope limitation:** evergabe.de is one of approximately 100+ German procurement portals tracked by market participants. It does not cover federal buyers, Deutsche Bahn or other utilities (who use DTVP or proprietary systems), or buyers in Länder that mandate other platforms (e.g., Bavaria has its own evergabe system at evergabe-online.de).
- **Assessment:** This portal has the clearest legal and technical basis for open Layer 2 access among all German portals investigated. If a Germany sprint is commissioned, evergabe.de should be the first portal tested.

#### DTVP (Deutsches Vergabeportal)

- **Operator:** Bundesanzeiger Verlag GmbH
- **URL:** https://www.dtvp.de / https://en.dtvp.de
- **Layer 2 access claim:** DTVP claims to cover "almost 100% of all tenders from public contracting authorities" in Germany. However, DTVP's access model for procurement documents requires bidder registration. The information brochure published by Bundesanzeiger Verlag confirms that document download requires a DTVP account.
- **Test result:** Not tested — registration required for document download is documented by the operator.
- **Assessment:** Gated. Not viable for Layer 2 programmatic access without authentication.

#### Vergabe24 / evergabe-online.de

- **Operator:** Various Länder IT service operators (e.g., Bayern: AKDB/IT@M)
- **Access model:** These platforms are Länder-mandated in certain states (Bavaria mandates evergabe-online.de for state and municipal buyers). Document access policies vary by platform and procurement type.
- **Test result:** Not tested within this sprint.
- **Assessment:** Fragmented by Länder mandate. Not viable as a single-source Layer 2 feed.

#### service.bund.de

- **Operator:** ITZBund (Federal IT Centre)
- **URL:** https://www.service.bund.de
- **Scope:** Aggregates public procurement notices from federal, Länder, and municipal authorities across Germany. Functions as a notice aggregator (Layer 1).
- **Layer 2 access:** service.bund.de links out to individual contracting authority platforms or platform PDFs. It does not host specification documents itself. Individual linked portals may be gated.
- **Assessment:** Viable for Layer 1 (notice discovery). Not viable for Layer 2 (specification documents) as a standalone source.

#### deutsche-evergabe.de

- **URL:** https://www.deutsche-evergabe.de
- **Scope:** Separate platform from evergabe.de. Covers a subset of German buyers.
- **Test result:** Portal is live but document access model was not tested within this sprint.
- **Assessment:** Insufficient information — not tested.

### API and Fragmentation Assessment

Germany has no national procurement API analogous to Estonia's `riigihanked.riik.ee`, the Netherlands' `tenderned.nl`, or Denmark's `api.udbud.dk`. The vergabepilot.ai platform, a German market entrant, documents over 100 separate procurement data sources it monitors in Germany — confirming that no single feed exists.

TED covers above-threshold German notices at Layer 1, but the TED API v3 does not retrieve specification documents (Layer 2). Specification documents are hosted on the originating platform, which in Germany means one of the 100+ portals.

The conclusion is that Germany cannot be viably covered by a Layer 2 programmatic feed without either (a) building integrations with multiple individual platforms, or (b) scoping to a single portal such as evergabe.de and accepting that coverage is partial and non-representative of the full German market.

---

## Task 2 — Denmark: Healthcare IT Volume

### API Status

The primary data source specified in the brief — `https://api.udbud.dk/udbud` — returned HTTP 503 Service Unavailable across all endpoints tested on 2026-04-26 between approximately 09:00 and 11:00 CEST. This affects the following paths:

| Endpoint | Status |
|---|---|
| `https://api.udbud.dk/` | 503 Service Unavailable |
| `https://api.udbud.dk/udbud` | 503 Service Unavailable |
| `https://api.udbud.dk/notices` | 503 Service Unavailable |
| `https://api.udbud.dk/search` | 503 Service Unavailable |

The API is live at the infrastructure level: TLS certificate confirmed, issued to `*.udbud.dk` by Erhvervsstyrelsen (Danish Business Authority) via DigiCert, valid 6 Nov 2025 – 1 Dec 2026. The backend service itself is unavailable, consistent with a maintenance window or transient outage rather than permanent deprecation.

The `udbud.dk` web portal (https://udbud.dk) responds normally as a JavaScript SPA. The API backend and web frontend run on separate infrastructure.

### TED API Fallback: Coverage and Limitations

The TED API v3 (`https://api.ted.europa.eu/v3/notices/search`) was used as a fallback. Key findings:

- TED API v3 indexes **eForms-format notices only**. eForms became mandatory for above-threshold EU procurement notices in October 2023. This means the majority of 2023 notices (published before the eForms cutover) are available in TED's XML bulk download but are not queryable via the v3 search API's structured fields.
- The `organisation-country-buyer` field in TED v3 returns zero results with the `= DK` operator, confirming that the pre-eForms 2023 notices are not in the v3 index under that field.
- The TED v3 API does not expose a `cpv` field in its expert search syntax; the correct CPV fields (`BT-262-Lot` etc.) do not support range comparison operators, making CPV-filtered counting non-trivial via the v3 API.
- Full CPV-filtered counting for Denmark 2023–2024 is feasible via the **TED bulk data download** (https://ted.europa.eu/en/search — annual export), but this was not executed within this sprint.

### Volume Count: Not Retrieved

The CPV-filtered notice counts required by the brief could not be produced in this session. The table below documents the intended query targets and the reason each was not populated:

| CPV division | 2023 count | 2024 count | Reason not populated |
|---|---|---|---|
| 72xxxxxxx — IT services | — | — | api.udbud.dk: 503; TED v3: eForms-only, pre-Oct 2023 gap |
| 48xxxxxxx — Software | — | — | Same |
| 85xxxxxxx — Health services | — | — | Same |
| 33xxxxxxx — Medical equipment | — | — | Same |
| **Total** | — | — | — |

### Denmark Layer 2 Access: Confirmed

The volume condition does not affect the confirmed Layer 2 access finding from the previous research round. The `api.udbud.dk` URL pattern returns specification document URLs without authentication when the service is available. This was validated in the prior research sprint and is not in dispute.

The udbud.dk web portal is operated by Erhvervsstyrelsen (Danish Business Authority), making it a government-operated national system. This is structurally superior to Estonia and the Netherlands for purposes of long-term data availability.

### Retry Instructions

To complete Task 2, execute the following queries against `https://api.udbud.dk/udbud` once the service resumes:

```
GET https://api.udbud.dk/udbud?cpv=72&datePublishedFrom=2023-01-01&datePublishedTo=2023-12-31
GET https://api.udbud.dk/udbud?cpv=48&datePublishedFrom=2023-01-01&datePublishedTo=2023-12-31
GET https://api.udbud.dk/udbud?cpv=85&datePublishedFrom=2023-01-01&datePublishedTo=2023-12-31
GET https://api.udbud.dk/udbud?cpv=33&datePublishedFrom=2023-01-01&datePublishedTo=2023-12-31
```

Repeat for `datePublishedFrom=2024-01-01&datePublishedTo=2024-12-31`. Apply the viability thresholds from the brief: ≥50 combined annual notices = Sufficient; 30–49 = Borderline; <30 = Insufficient.

### Volume Verdict

**Unresolved** — data unavailable at query time. Denmark's Layer 2 access verdict remains **Viable — conditional on volume confirmation**.

---

## Updated Country Viability Summary

| Country | Layer 1 | Layer 2 | Verdict |
|---|---|---|---|
| Estonia | ✅ TED + riigihangete register | ✅ Open, programmatic access confirmed | **Viable — proceed** |
| Netherlands | ✅ TED + TenderNed | ✅ Open access confirmed | **Viable** |
| Denmark | ✅ TED + udbud.dk | ✅ Open access confirmed (`api.udbud.dk`, no auth required) | **Viable — conditional on volume count (api.udbud.dk outage blocking confirmation; retry required)** |
| France | ✅ TED + BOAMP/PLACE | ❌ Fragmented, gated | **Ruled out** |
| Italy | ✅ TED + ANAC | ❌ Fragmented, gated | **Ruled out** |
| Germany | Removed — no single mandatory portal; 100+ fragmented systems | — | **Ruled out** |

---

## Blockers and Next Actions

| # | Item | Owner | Action |
|---|---|---|---|
| 1 | Denmark volume count | Research agent | Retry `api.udbud.dk` CPV queries (72, 48, 85, 33) for 2023 and 2024 once service resumes |
| 2 | Denmark volume count fallback | Research agent | If api.udbud.dk remains unavailable >48h, use TED bulk data download for DK + CPV prefix annual export |
| 3 | Germany (optional future sprint) | Research agent | If Germany is reconsidered, test evergabe.de `/unterlagen` with a live Vergabenummer and retrieve one specification document to confirm open Layer 2 access; then scope the buyer population covered by that portal |

---

## Technical Appendix: TED API v3 Behaviour

The following findings on TED API v3 are documented for future research use.

### Expert Search Query Syntax

- Field-value pairs use the `=`, `!=`, `>=`, `<=`, `~` operators
- The `~` operator performs text/phrase matching; `=` is exact equality
- Country fields (`organisation-country-buyer`) use `=` for exact code match but the pre-eForms notice index is not populated, returning zero results for most historical queries
- CPV fields (`BT-262-Lot`) exist in the field list but do not support range comparison operators (`>=`, `<=`), making them unsuitable for CPV-division filtering
- All queries require a non-empty `fields` parameter

### eForms Coverage Gap

The TED v3 search API only indexes eForms-format notices. eForms became mandatory:
- **Above-threshold notices:** October 2023
- **Below-threshold notices:** Not yet mandated at EU level

This means approximately 9 months of 2023 above-threshold notices are not accessible via TED v3 structured search. They remain available via the TED XML bulk download and the legacy TED search interface, but not via the v3 API's CPV/country field filters.

### Confirmed Working Query Pattern

```json
POST https://api.ted.europa.eu/v3/notices/search
Content-Type: application/json

{
  "query": "publication-date >= 20230101 AND publication-date <= 20231231",
  "page": 1,
  "limit": 10,
  "fields": ["publication-date", "buyer-touchpoint-partname"]
}
```

Total notice count for all countries, all CPV, 2023: **795,680** (above-threshold eForms notices indexed in TED v3).

---

*Issued 2026-04-26. Responds to [`2026-04-germany-denmark-clarification-brief.md`](./2026-04-germany-denmark-clarification-brief.md). Follows from [`2026-04-gap-resolution-france-assessment.md`](./2026-04-gap-resolution-france-assessment.md).*
