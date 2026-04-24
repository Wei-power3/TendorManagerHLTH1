# EU Procurement Data Accessibility: Country Viability Assessment

> **Document type:** Research output — analyst handover response  
> **Research brief:** [`2026-04-country-research-brief.md`](./2026-04-country-research-brief.md)  
> **Date:** 2026-04-24  
> **Status:** Complete — viable candidates identified

---

## Executive Summary

Three EU member states offer procurement infrastructure that meaningfully solves or substantially reduces the two-layer problem identified in the pivot decision note: **Estonia**, **the Netherlands**, and **Denmark**.

Of these, Estonia is the most compelling starting point — it has a fully centralised, mandatory single portal with a public REST API, open document access accessible to international suppliers, and reliable healthcare IT volume.

The Netherlands is the strongest open data play (OCDS-compliant, daily or monthly structured updates) but Layer 2 documents require a registered account. Denmark sits in between, with a centralised portal and predictable URL-based document access, but the bulk data API requires an approved role from the national authority.

Finland and Portugal are partially viable but both reproduce a "fragmentation-lite" version of the Italy problem: centralised notice publication but fragmented downstream platforms for actual procurement documents. Sweden and Austria are not viable because neither has a single mandatory national portal.

---

## Background: The Two-Layer Problem

As documented in [`2026-04-pivot-decision.md`](./2026-04-pivot-decision.md), the core problem is the gap between:

- **Layer 1** — TED notice data: publicly accessible, machine-readable, API-ready. Contains buyer identity, CPV codes, award outcomes, and, for post-October 2023 eForms, award criteria weights.
- **Layer 2** — Specification documents: technical specifications, evaluation methodologies, scoring rubrics. These reveal how a tender is structured and what it takes to win.

Italy was ruled out because its Layer 2 is split across dozens of regional platforms, none with public APIs, and most requiring procurement-authority credentials. The research question was whether any EU member states had solved this at the national level.

A relevant legal baseline is Article 53 of Directive 2014/24/EU, which requires unrestricted and full direct access free of charge to procurement documents from the date of publication. In practice, implementation varies: some countries expose this through open portals, while others route access through free-but-mandatory registration.

---

## Country Assessments

### Estonia

**Country:** Estonia  
**National portal:** `https://riigihanked.riik.ee`  
**Centralised?** Yes — one mandatory national portal handles the market above procurement thresholds.  
**Document access:** Open / free access for suppliers; registration is needed for bid submission, not for reading notices and documents.  
**Programmatic access:** Public REST API and OCDS/open data export.  
**API documentation URL:** `https://riigihanked.riik.ee/rhr-web/#/open-data`  
**Healthcare IT volume:** Moderate but sufficient for a v1 market.  
**Language:** Estonian, with partial English interface.  
**eForms adoption:** Confirmed.  
**Verdict:** **Viable**  
**Recommendation:** Best starting market. Test unauthenticated retrieval of healthcare IT tender documents from the public API and linked attachments.

Estonia's procurement platform is the cleanest match to the research brief. It is genuinely centralised and supports public notice retrieval through stable public endpoints such as `/rhr/api/public/v1/notice/{id}/html`.

The key distinction is that bid submission is gated by electronic identity, but document access is not. That makes Estonia materially different from Italy and Germany.

The main downside is language. Estonian is less convenient for LLM parsing than Dutch, Danish, or German, but the trade-off is worth it because the infrastructure is far cleaner.

---

### Netherlands

**Country:** Netherlands  
**National portal:** `https://www.tenderned.nl`  
**Centralised?** Yes — authorities publish national and European tenders through TenderNed.  
**Document access:** Free registration required.  
**Programmatic access:** API available; OCDS/open data available in machine-readable form.  
**API documentation URL:** `https://www.tenderned.nl/info/swagger`  
**Healthcare IT volume:** High.  
**Language:** Dutch.  
**eForms adoption:** High / structured publication is strong.  
**Verdict:** **Viable**  
**Recommendation:** Strong second market. Use the open Layer 1 data immediately and accept the one-time registration step for Layer 2 document access.

TenderNed is one of the most technically mature procurement systems in the EU. It stands out because the notice data is already aligned with open contracting publication standards and is accessible as structured data.

Its weakness is not fragmentation but friction. Documents are not anonymous-download by default; they sit behind a free registration flow for suppliers.

That makes the Netherlands highly workable if the product can tolerate a one-time operational setup rather than pure no-login access.

---

### Denmark

**Country:** Denmark  
**National portal:** `https://udbud.dk`  
**Centralised?** Yes.  
**Document access:** Publicly accessible via direct attachment URLs.  
**Programmatic access:** API exists, but bulk access requires approved credentials/role.  
**API documentation URL:** `https://api.udbud.dk/udbud` and public SDK/OpenAPI references.  
**Healthcare IT volume:** Moderate and likely sufficient.  
**Language:** Danish.  
**eForms adoption:** Confirmed.  
**Verdict:** **Possibly viable**  
**Recommendation:** Run a small technical validation. If direct attachment URLs are reproducible at scale, Denmark becomes a strong candidate.

Denmark is promising because document files appear to be served from predictable URLs under `api.udbud.dk/.../open/...`. That is materially better than a purely interactive portal.

The risk is that the official API itself is not fully open in the same way as Estonia's. Bulk or privileged access requires approval from the Danish authority.

So Denmark is best treated as an engineering validation candidate, not the first market.

---

### Finland

**Country:** Finland  
**National portal:** `https://www.hankintailmoitukset.fi`  
**Centralised?** Yes for notices, not fully for documents.  
**Document access:** Depends on the linked e-procurement platform.  
**Programmatic access:** Strong open API for notices.  
**API documentation URL:** `https://hns-hilma-prod-apim.developer.azure-api.net`  
**Healthcare IT volume:** High.  
**Language:** Finnish.  
**eForms adoption:** Strong / improving.  
**Verdict:** **Possibly viable**  
**Recommendation:** Attractive only if the product scope shifts toward Layer 1 analytics. Not ideal for automated specification-document retrieval.

Finland has one of the best notice APIs in the group. If the product only needed metadata, Finland would be near the top of the list.

But the research brief explicitly focuses on Layer 2 access, and Finland fails that test because procurement documents are commonly hosted on separate platforms such as Cloudia or Tarjouspalvelu.

This is a softer version of the same structural issue already encountered in Italy.

---

### Portugal

**Country:** Portugal  
**National portal:** `https://www.base.gov.pt`  
**Centralised?** Partial — contract transparency is centralised, tender execution is not.  
**Document access:** Platform-dependent.  
**Programmatic access:** REST API for BASE metadata, not for all specification documents.  
**API documentation URL:** `https://www.base.gov.pt/APIBase2`  
**Healthcare IT volume:** Moderate.  
**Language:** Portuguese.  
**eForms adoption:** Partial.  
**Verdict:** **Not viable** for this use case  
**Recommendation:** Useful for award intelligence, not for a product dependent on scalable Layer 2 retrieval.

Portugal's BASE portal is useful, but it is not the whole procurement stack. It aggregates contract-level information while active tender documentation is spread across multiple certified commercial platforms.

That architecture means Portugal is informative but not operationally clean for the current product direction.

---

### Sweden

**Country:** Sweden  
**National portal:** No single mandatory portal.  
**Centralised?** No.  
**Document access:** Fragmented across commercial platforms.  
**Programmatic access:** No single national access layer.  
**Healthcare IT volume:** High but irrelevant given fragmentation.  
**Language:** Swedish.  
**eForms adoption:** Mixed.  
**Verdict:** **Not viable**  
**Recommendation:** Rule out.

Sweden does not solve the core platform problem. Tenders are distributed across several commercial platforms rather than one mandatory national system.

That makes it structurally too similar to Germany for the current stage.

---

### Austria

**Country:** Austria  
**National portal:** No single mandatory portal.  
**Centralised?** No.  
**Document access:** Fragmented.  
**Programmatic access:** No single national route to documents.  
**Healthcare IT volume:** Moderate.  
**Language:** German.  
**eForms adoption:** Not enough to offset fragmentation.  
**Verdict:** **Not viable**  
**Recommendation:** Rule out.

Austria has search and aggregation layers, but actual procurement procedures still run across multiple platforms. That fails the core centralisation criterion.

---

## Summary Table

| Country | Centralised | Document access | Programmatic access | Verdict |
|---|---|---|---|---|
| Estonia | Yes | Open / free access | Public REST API | Viable |
| Netherlands | Yes | Free registration required | API + OCDS/open data | Viable |
| Denmark | Yes | Public via URL patterns | API but partly gated | Possibly viable |
| Finland | Notices only | Fragmented by platform | Strong notice API only | Possibly viable |
| Portugal | Partial | Fragmented by platform | Metadata API only | Not viable |
| Sweden | No | Fragmented | None at national level | Not viable |
| Austria | No | Fragmented | None at national level | Not viable |

---

## Final Recommendation

### Priority order

1. **Estonia** — start here first.
2. **Netherlands** — second market once the pipeline works.
3. **Denmark** — test technically before committing.
4. **Finland** — only if the use case shifts toward Layer 1 analytics.

### Immediate next step

Run a technical validation on Estonia using healthcare-related CPV searches and confirm that:

- notices can be fetched from the public API,
- linked specification documents are retrievable without restricted credentials,
- the attachment structure is stable enough for automation,
- the corpus is large enough to support a first market experiment.

If Estonia passes that validation, it becomes the best v1 market for the post-pivot product direction.
