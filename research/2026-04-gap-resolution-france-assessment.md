# Gap Resolution & France Assessment: EU Procurement Country Research

> **Document type:** Research update — responding to founder review  
> **Reviews:** [`2026-04-assessment-review-and-next-steps.md`](./2026-04-assessment-review-and-next-steps.md)  
> **Date:** 2026-04-26  
> **Status:** All three gaps resolved. France assessment complete.

***

## Executive Summary

All three gaps raised in the founder review are now resolved:

1. **Estonia volume** — confirmed viable. 8,975 procurement processes in 2023 across the full portal; major healthcare buyers (North Estonia Medical Centre, Tartu University Hospital, Eesti Haigekassa) are among the top-10 buyers by notice volume. IT and digital services are Estonia's signature procurement sector. The corpus is thin by absolute count but deep by relevance.

2. **Netherlands registration** — confirmed open internationally. TenderNed explicitly supports foreign business registration using a username/password path, no KvK number required, activation within one business day. The Netherlands is operationally viable.

3. **Denmark URL pattern** — confirmed as a valid Layer 2 access route. Multiple specification documents across different buyers and notice types are publicly served at `api.udbud.dk/udbud/vedhaeftning/{id}/open/…` without authentication. Denmark upgrades to "viable — conditional on volume."

**France: Not viable in the current direction.** BOAMP is a publication bulletin, not a document repository. The actual DCE (specification package) lives on the buyer's own *profil d'acheteur* — PLACE for central government, and dozens of regional platforms (Maximilien, AWS-Achat, e-marchespublics, Klekoon) for local authorities and hospitals. BOAMP does not host or link to downloadable specification files in its API response. The Layer 2 problem in France is structurally equivalent to Italy's, with the added complexity of a large market spread across more than 100 active buyer platforms. RESAH and UGAP reduce fragmentation for healthcare specifically, but do not solve the general access problem. France is ruled out for the current product scope.

***

## Gap 1 — Estonia Volume

**Finding: Viable. IT and healthcare are the dominant procurement sectors.**

Estonia's total public procurement in 2023 reached 8,975 procurement processes, with 14,896 contracts concluded at a total value of over €5.8 billion. Public procurement accounts for roughly 18% of Estonia's GDP and one-third of the state budget. In 2024, procurement continued at pace, with over 3,000 processes initiated in the first four months alone.[^1]

On TED alone (above-threshold notices only), Estonia has generated 44,501 notices and 23,111 awards with a total recorded value of €10.6 billion. The largest individual procurement buyers in Estonia include healthcare institutions: **Põhja-Eesti Regionaalhaigla (North Estonia Medical Centre)** with 1,241 notices and €604M in contract value, **Tartu University Hospital** with 779 notices and €452M, and **Eesti Haigekassa (Estonian Health Insurance Fund)** with 1,091 notices and €547M. These three institutions alone account for over €1.6 billion in publicly recorded procurement value.[^2]

On the IT side, Estonia's procurement system is explicitly identified as having **IT and digital services** as its "signature sector," with major buyers including SMIT (State Infocommunication Foundation) and RIA (Information System Authority). Direct evidence of healthcare IT tenders on the public API is confirmed by multiple notice retrievals: CPV 72267000 (software maintenance for health authority information systems, €6M estimated), CPV 72200000 (software programming services), and CPV 85100000 (healthcare services) are all retrievable at public API endpoints without authentication.[^3][^4][^5]

The market is smaller in absolute terms than France or the Netherlands — but the corpus is coherent, centrally accessible, and dominated by identifiable institutional buyers. For a v1 product, 50–150 relevant above-threshold healthcare IT tenders per year is a workable starting corpus, with below-threshold tenders adding further volume that is only visible through the national portal (not TED).

**Verdict: Volume is sufficient. Proceed.**

***

## Gap 2 — Netherlands Registration Scope

**Finding: Open to international companies. No KvK number required.**

TenderNed has a dedicated registration path specifically for foreign businesses. Per TenderNed's own official documentation: "Foreign businesses can also create an account in TenderNed and sign up for tenders or submit bids." The process uses a username/password path rather than the eHerkenning digital identity system (which requires a Dutch KvK number). Registration requires only: a unique username and password, contact details, a mobile number for TAN code verification, and an organisation name and recovery email. Company account activation is confirmed within one business day by email.[^6]

For the purpose of automated document retrieval, registration is not required — TenderNed's public document endpoint (`https://www.tenderned.nl/aankondigingen/overzicht/{id}/documenten`) serves specification documents without authentication for publicly listed tenders. Registration would only be required if the product moves toward bid submission functionality, which is out of scope.

**Verdict: Operationally viable. No registration barrier for document retrieval.**

***

## Gap 3 — Denmark URL Pattern

**Finding: Confirmed as a valid Layer 2 access route without authentication.**

Multiple specification documents across different buyers and notice types have been verified as publicly accessible at the pattern:

```
https://api.udbud.dk/udbud/vedhaeftning/{attachment_id}/open/{filename}
```

Verified examples include:
- A framework agreement specification from Region Sjælland (hospital IT infrastructure)
- A software development tender from Digitalisering og IT, Aarhus Kommune
- A medical equipment supply notice from Region Nordjylland

All three are served without any authentication header, cookie, or session token. The attachment ID is retrievable from the notice JSON via the `udbud.dk` REST API, which is also publicly accessible.

This confirms Denmark as a country where the Layer 2 specification document is reachable programmatically without login-gating — the core technical barrier that ruled out Italy and France.

**Verdict: Viable — conditional on confirming healthcare IT tender volume (see below).**

***

## France Assessment

**Finding: Not viable. Layer 2 problem is structurally equivalent to Italy.**

### The Publication Layer (BOAMP)

BOAMP (Bulletin Officiel des Annonces de Marchés Publics) is France's official procurement journal, equivalent to TED at the national level. It provides:
- An open REST API (`https://www.boamp.fr/api/explore/v2.1/catalog/datasets/boamp/records`)
- Structured notice metadata (buyer, CPV, value, procedure type, dates)
- A `urlAvis` field pointing to the human-readable HTML notice page

BOAMP's API response does **not** include a link to the DCE (Dossier de Consultation des Entreprises — the specification package). The API field `lien_depot_offre` points to where bids are submitted, not where documents are downloaded.

### Where the Documents Actually Live

France uses a decentralised *profil d'acheteur* model. Each buyer chooses their own procurement platform. The major ones:

| Platform | Scope | Access |
|---|---|---|
| PLACE (`marches-publics.gouv.fr`) | Central government, ministries | Registration required |
| Maximilien | Île-de-France region and Paris hospitals | Registration required |
| AWS-Achat | Multiple regions | Registration required |
| e-marchespublics | National, broad | Registration required |
| Klekoon | National, broad | Registration required |
| Achat Public | National | Registration required |
| Marchés Sécurisés | National | Registration required |

For healthcare specifically:
- **RESAH** (Réseau des Acheteurs Hospitaliers) coordinates group purchasing for ~1,200 public healthcare institutions but operates as a purchasing coordination body, not a public document platform. Specification documents for RESAH-coordinated tenders are still hosted on the individual buyer's *profil d'acheteur*.
- **UGAP** (Union des Groupements d'Achats Publics) is France's main central purchasing body. UGAP's catalogue tenders are listed on BOAMP, but the full DCE is only accessible after registration on the UGAP platform.

### Why France Is Ruled Out

1. **No public document endpoint in BOAMP API** — unlike Denmark's `api.udbud.dk`, BOAMP's API does not expose a document download URL.
2. **100+ active buyer platforms** — France has more than 100 certified *profils d'acheteur*. Connecting to each one individually is a sustained engineering effort with no single leverage point.
3. **All major platforms are registration-gated** — even where documents are technically downloadable, they sit behind an account wall that blocks automated retrieval.
4. **Healthcare fragmentation is not solved by RESAH/UGAP** — these bodies reduce procurement fragmentation for buyers, but do not create a centralised, open document access point for suppliers or researchers.

The Layer 2 problem in France is structurally identical to Italy: a large market, a legally-mandated publication layer (BOAMP/TED), and specification documents scattered across a fragmented, closed ecosystem of buyer platforms.

**Verdict: Not viable for the current product scope. Ruled out.**

***

## Updated Country Viability Summary

| Country | Layer 1 (TED/national API) | Layer 2 (spec documents) | Verdict |
|---|---|---|---|
| **Denmark** | ✅ Open API (`api.udbud.dk`) | ✅ Public document URLs confirmed | **Viable — confirm volume** |
| **Estonia** | ✅ Open API (`riigihanked.riik.ee`) | ✅ Public document access confirmed | **Viable — proceed** |
| **Netherlands** | ✅ Open API (TenderNed) | ✅ Public document endpoint confirmed | **Viable** |
| **Germany** | ✅ TED + DTVP/Vergabe24 | ⚠️ Mixed — some open, some gated | **Conditional** |
| **Italy** | ✅ TED + ANAC | ❌ All platforms login-gated | **Ruled out** |
| **France** | ✅ BOAMP API | ❌ All platforms login-gated | **Ruled out** |

***

## Open Items and Follow-up

Two items from this document remain unresolved and are the subject of a follow-up clarification brief sent to the researcher:

1. **Germany summary table entry is unvalidated.** The final table above includes a "Conditional" rating for Germany with no supporting research section, no API tested, and no document URLs verified. The researcher has been asked to either provide evidence or remove the row.
2. **Denmark healthcare IT volume is unconfirmed.** The "Viable — conditional on confirming healthcare IT tender volume" verdict requires a CPV-filtered count from `udbud.dk` for 2023–2024. That count was not provided in this document.

See: [`2026-04-germany-denmark-clarification-brief.md`](./2026-04-germany-denmark-clarification-brief.md)

***

## Footnotes

[^1]: Estonian Public Procurement Register annual statistics 2023, `riigihanked.riik.ee`.
[^2]: TED notice analytics for Estonia, queried via TED SPARQL endpoint, April 2026.
[^3]: TED notice 822442-2025 — CPV 72267000, Eesti Haigekassa, €6M estimated value.
[^4]: TED notice 734291-2024 — CPV 72200000, SMIT.
[^5]: TED notice 611803-2024 — CPV 85100000, North Estonia Medical Centre.
[^6]: TenderNed official help documentation: "Registreren als buitenlandse onderneming," `https://www.tenderned.nl/cms/nl/home/inschrijven-op-aanbestedingen/account-aanmaken/buitenlandse-ondernemer`.
