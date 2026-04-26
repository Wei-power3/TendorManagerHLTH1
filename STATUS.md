# Project Status

**Last updated:** 2026-04-26
**Session summary:** Completed Step 2 — market selection decision document written, committed, and merged via PR #2.

---

## Active Plan

`plan/2026-04-post-pivot-action-plan.md`

---

## Completed

- [x] Research phase: 8 documents produced, all country verdicts confirmed
- [x] Step 2: Market selection decision document — `plan/2026-04-market-selection-decision.md` committed and merged (PR #2)

---

## Current Position

**Step:** Step 3 — Technical Validation: Estonia
**Status:** Not started
**What was just done:** Step 2 is fully merged. The market selection decision document closes the research phase: Estonia = primary, Netherlands = second, Denmark = parked pending API recovery.

---

## Next Action

**Immediate next step:** Run the Estonia technical validation sprint defined in the plan (Steps 3.1–3.10): hit the `riigihanked.riik.ee` public API, retrieve CPV-filtered healthcare IT notices, pull 5 specification documents, assess language, and run LLM extraction on 2 representative documents.
**File to produce / edit:** `plan/2026-04-estonia-validation-report.md`
**Depends on:** None — Step 3 can start immediately

---

## Open Blockers

- Denmark volume count: `api.udbud.dk` was returning HTTP 503 on 2026-04-26. Retry when service recovers. No action required now — Denmark is parked. If volume count comes back before Step 4 is complete, fold into architecture decision.

---

## Parked Items

- **Denmark — volume confirmation:** Re-entry trigger: `api.udbud.dk` recovers → run 4 CPV queries (72, 48, 85, 33) for 2023–2024 → if combined annual count ≥50, viable for v1.5 or earlier.
- **Germany — evergabe.de subset:** A narrow exception under §9(3) VgV was noted but not tested. Documented in `research/2026-04-germany-denmark-clarification-response.md`. Germany remains ruled out for now; re-enter only in a dedicated Germany sprint.

---

## Context Notes

- The two-layer problem: Layer 1 = machine-readable notice API; Layer 2 = open access to specification documents without authentication. Italy, France, and Germany all fail Layer 2. Estonia and Netherlands pass both.
- Estonia validation risk to watch: Estonian is a Finno-Ugric language. If specification documents are Estonian-only, LLM extraction quality needs to be tested before committing architecture. Step 3 includes this test (Tasks 3.8–3.10).
- Step 4 (product architecture decision) cannot start until Step 3 is complete — the scope decision (Phase 1 metadata-only vs Phase 1+2 with spec intelligence) depends on the LLM extraction result.
- Steps 2 and 3 were designed to run in parallel; Step 2 is now done so Step 3 is the only active thread.
- Pass/Conditional/Fail criteria for Estonia are defined in the plan — if Estonia fails Layer 1 or 2, escalate immediately and pivot to Netherlands as primary validation candidate.
