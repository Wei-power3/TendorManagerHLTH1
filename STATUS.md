# Project Status

**Last updated:** 2026-04-26
**Session summary:** Completed Step 2 (market selection decision document) and set up project memory system (STATUS.md, /gitcheckpoint, CLAUDE.md)

---

## Active Plan

`plan/2026-04-post-pivot-action-plan.md`

---

## Completed

- [x] Original v1 plan suspended — Italy ruled out (pivot decision, 2026-04-24)
- [x] Two-layer data problem defined — TED as notice layer vs specification documents (`research/2026-04-ted-data-architecture.md`)
- [x] EU country viability sweep — 7 countries assessed (`research/2026-04-eu-procurement-country-viability-assessment.md`)
- [x] Founder review gaps resolved — Estonia volume, NL registration, Denmark URL pattern (`research/2026-04-gap-resolution-france-assessment.md`)
- [x] France assessed and ruled out (same document)
- [x] Germany ruled out, Denmark volume blocked by API outage (`research/2026-04-germany-denmark-clarification-response.md`)
- [x] Post-pivot action plan written (`plan/2026-04-post-pivot-action-plan.md`)
- [x] **Step 2: Market selection decision document written and committed** (`plan/2026-04-market-selection-decision.md`)

---

## Current Position

**Step:** Step 3 — Technical Validation: Estonia (not yet started)
**Status:** Ready to begin
**What was just done:** Step 2 complete. Market selection decision document committed to `plan/2026-04-market-selection-decision.md` on branch `claude/post-pivot-action-step-2-zMosj`.

---

## Next Action

**Immediate next step:** Run Step 3 — Estonia technical validation sprint
**File to produce:** `plan/2026-04-estonia-validation-report.md`
**Depends on:** None — Step 3 is independent of any remaining Step 2 work

Step 3 tasks in order:
1. Layer 1: Hit `https://riigihanked.riik.ee/rhr/api/public/v1/notice` without auth, confirm 200
2. Layer 1: CPV-filtered query for healthcare IT (CPV 72, 48, 85, 33) for 2024
3. Layer 1: Count results, confirm 50–150 annual notice estimate is plausible
4. Layer 1: Record JSON field names for notice ID, buyer, CPV, value, date, attachment links
5. Layer 2: Extract document URLs from 5 notices
6. Layer 2: Retrieve each document without authentication, record format
7. Layer 2: Assess completeness (technical requirements / evaluation criteria / scoring weights)
8. Language: Assess language distribution across 5 documents
9. LLM extraction: Run structured extraction prompt on 2 documents
10. LLM extraction: Rate output on factual accuracy / completeness / language handling

---

## Open Blockers

- **Denmark volume count** — `api.udbud.dk` was returning HTTP 503 on 2026-04-26. Re-entry trigger: retry the four CPV queries (72, 48, 85, 33) for 2023 and 2024 once the service recovers. If ≥50 combined annual notices, Denmark is viable. If this resolves before Step 4 is complete, fold into the architecture decision.

---

## Parked Items

- **Denmark** — Layer 2 access confirmed viable; volume unconfirmed due to API outage. See above.
- **Germany** — Ruled out. One future note: evergabe.de has a credible open Layer 2 access model under §9(3) VgV. If Germany is ever reconsidered, test that portal first (`research/2026-04-germany-denmark-clarification-response.md`).

---

## Context Notes

- Estonia is primary build target. Netherlands is second. Both have confirmed open Layer 2 document access.
- Step 3 pass/fail criteria are defined in `plan/2026-04-post-pivot-action-plan.md` — read before starting the validation sprint.
- If Step 3 fails (API gated, documents not retrievable), escalate to founder and rerun Layer 2 test on TenderNed (Netherlands).
- Step 4 (architecture decision) cannot start until Step 3 is complete. Step 2 and Step 3 were independent.
- Active dev branch: `claude/post-pivot-action-step-2-zMosj`. New work for Step 3 should go on a new branch.
- `PLAN.md` in root is the suspended v1 plan — kept for reference, not the active roadmap.
