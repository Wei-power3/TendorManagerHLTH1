# Assessment Review and Next Steps

> **Document type:** Founder review note  
> **Author:** Weiche Lin  
> **Date:** 2026-04-26  
> **Reviews:** [`2026-04-eu-procurement-country-viability-assessment.md`](./2026-04-eu-procurement-country-viability-assessment.md)  
> **Status:** Active — actions open

---

## Overall Verdict on the Assessment

The assessment is solid and actionable. The researcher followed the brief correctly: clear per-country verdicts, honest about failure modes, good priority ordering. The Estonia finding is the right call. The distinction between "gated for bid submission" vs "gated for document reading" is exactly the right thing to check, and the researcher got it right. Finland's verdict is honest — strong notice API but fails the Layer 2 test for the same structural reason as Italy. Sweden and Austria are correctly killed quickly.

Three things need to be addressed before acting on the recommendations.

---

## Pushback: Three Gaps in the Assessment

### 1. Estonia's volume is understated as a risk

The assessment rates healthcare IT volume as "moderate but sufficient" without a number. Estonia has a population of 1.3 million. That qualifier is doing a lot of work.

If the actual corpus of relevant healthcare IT tenders (CPV 72, 48, 85, 33) is 30–40 per year, the product may be too thin to be analytically useful — there is not enough historical data to identify patterns, and not enough new tenders per month to make an ongoing intelligence service valuable.

**This must be the first thing validated**, before any API work begins. A count of healthcare IT tenders on `riigihanked.riik.ee` over the past two years takes less than an hour and is a binary gate.

### 2. Netherlands registration scope is unconfirmed

The assessment notes that TenderNed requires free registration for document access but does not confirm whether that registration is open to any company internationally or restricted to Dutch-registered entities.

This is a binary that determines whether the Netherlands is operationally viable. If registration requires a Dutch KvK (Chamber of Commerce) number, it is a blocker. If it is open to any company with an email address, it is a one-time setup and is not a blocker.

TenderNed registration is in practice open internationally, but the assessment should state this explicitly and verify it. Add one sentence confirming this before treating the Netherlands as viable.

### 3. Denmark's attachment URL stability is unvalidated

The "possibly viable" verdict for Denmark rests on the observation that specification documents appear to be served from predictable URLs under `api.udbud.dk/.../open/...`. This is a hypothesis, not a confirmed finding.

Before Denmark is treated as a candidate market, someone needs to retrieve 5–10 specification documents manually and confirm:
- the URL pattern is stable across different buyers and notice types,
- the documents are accessible without credentials, and
- the content is the full specification package, not just a cover notice.

Until that test is run, Denmark should be logged as a technical validation task, not a strategic decision.

---

## Suggested Actions

| Action | Owner | Priority | Blocks |
|---|---|---|---|
| Count healthcare IT tenders on Estonia portal (CPV 72, 48, 85, 33) for 2023–2024 | Analyst | Immediate | All Estonia work |
| Confirm TenderNed (NL) registration is open to international companies | Analyst | Immediate | Netherlands decision |
| Run Denmark URL validation: retrieve 5–10 spec documents, confirm pattern is stable and unauthenticated | Technical | Next | Denmark decision |
| France targeted research (see section below) | Analyst | Parallel | France decision |

---

## Why Germany Is Not Included

Germany was excluded from the original research brief and is not worth adding now. The reasoning is not an assumption — it is well-documented public fact:

Germany has no single mandatory national procurement portal. Above-threshold public tenders are distributed across DTVP, Vergabe24, evergabe.de, the federal Bund.de platform, and 16 Länder-level systems, each running independently. There is no legal requirement for a German contracting authority to use any one platform.

This is the same structural failure as Italy — fragmented Layer 2 with no central access point — but without Italy's partial mitigant of a national tender ID (CIG) that at least links notices to issuers.

Germany's healthcare IT procurement market is the largest in the EU by volume, which makes the fragmentation more frustrating, not less true. A high-value market behind an inaccessible platform is still inaccessible. Adding Germany to the research scope would produce a "Not Viable" verdict and consume analyst time for no strategic gain.

If Germany's procurement infrastructure consolidates in future (there are EU-level eForms adoption obligations that may drive this), it becomes worth revisiting. For now, it is ruled out.

---

## France: Targeted Research Brief

France was not in the original research scope. It should receive a focused check — not a full country assessment — for two reasons:

**Reason 1: BOAMP has a documented public API.** France's official tender publication journal (Bulletin Officiel des Annonces des Marchés Publics) publishes all above-threshold public tenders and has a public API. Whether that API returns links to specification documents, and whether those documents are accessible without credentials, is an open question that can be answered in 2–3 hours.

**Reason 2: Healthcare procurement in France has centralising bodies.** RESAH (Réseau des Acheteurs Hospitaliers) and UGAP (Union des Groupements d'Achats Publics) coordinate purchasing across French public hospitals. A meaningful portion of French healthcare IT procurement is channelled through these bodies, which may mean fewer access points than the general fragmentation of the French market would suggest.

France is the second-largest public procurement market in the EU. French is well-handled by LLMs. If the BOAMP API returns accessible specification documents, France becomes the top candidate by market size.

### Research format for France

This is a one-question brief, not a full country assessment. Deliver a single document covering:

**The core question**
Does the BOAMP API return document URLs for specification packages, and are those documents accessible without authentication?

**What to check**

1. Retrieve 3–5 recent healthcare IT notices from the BOAMP API (CPV 72, 48, 85, or 33). Confirm the API endpoint, authentication requirements, and response structure.
2. For each notice, follow the document links in the API response. Confirm whether specification documents (capitaux techniques or equivalent) are downloadable without login.
3. Run the same check against RESAH and UGAP portals specifically. Confirm whether their tender documents are publicly accessible and whether there is any structured access route (API, bulk download, or stable URL pattern).

**Deliver**

```
France — BOAMP + RESAH/UGAP document access check

BOAMP API endpoint: [URL]
Authentication required for notice retrieval: [Yes / No]
Document URLs returned in API response: [Yes / No / Partial]
Documents accessible without login: [Yes / No / Partial — explain]
BOAMP API documentation: [URL]

RESAH portal: [URL]
Document access: [Open / Registration / Restricted]
Structured access route: [Yes / No — describe if yes]

UGAP portal: [URL]
Document access: [Open / Registration / Restricted]
Structured access route: [Yes / No — describe if yes]

Healthcare IT tender volume (rough, last 12 months): [number or estimate]
Language of documents: French
Verdict: [Viable / Possibly viable / Not viable]
Recommendation: [2–3 sentences]
```

Expected time: 2–3 hours. If BOAMP document access is confirmed open in the first 30 minutes, the RESAH/UGAP check becomes secondary and can be brief.

---

## Updated Priority Order (Pending Validations)

Once the gap actions above are resolved, the priority order from the assessment stands with one amendment:

1. **Estonia** — confirm volume first; if volume passes, start here.
2. **France** — if BOAMP document access is confirmed open, France moves to equal first on market size grounds.
3. **Netherlands** — strong second market once volume and registration are confirmed.
4. **Denmark** — technical validation only; not a strategic commitment until URL stability is confirmed.
5. **Finland** — only if the product scope shifts to Layer 1 analytics. Not the current direction.

---

*Review prepared 2026-04-26. Actions open.*
