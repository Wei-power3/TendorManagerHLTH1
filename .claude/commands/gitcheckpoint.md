Write a project checkpoint to STATUS.md capturing the current state so work can be resumed in a future session.

Follow these steps exactly:

1. Run `git log --oneline -15` to review recent commits
2. Read `STATUS.md` to see the previous checkpoint
3. Read `plan/2026-04-post-pivot-action-plan.md` for the active plan and step definitions
4. If the current step has a corresponding output file in `plan/`, read it to confirm what was completed

Then write an updated `STATUS.md` using the schema below. Replace every field with the actual current state — do not copy the previous checkpoint verbatim.

If the user included notes with this command, incorporate them: $ARGUMENTS

---

## STATUS.md schema

```
# Project Status

**Last updated:** [today's date]
**Session summary:** [one sentence — what was worked on this session]

---

## Active Plan

[Name and path of the current governing plan document]

---

## Completed

[Checklist of all steps/tasks finished so far, most recent last]

---

## Current Position

**Step:** [step number and name from the plan]
**Status:** [In progress / Just completed / Blocked]
**What was just done:** [1–2 sentences on the last concrete action taken]

---

## Next Action

**Immediate next step:** [specific, actionable — enough for a new session to start without reading anything else]
**File to produce / edit:** [if applicable]
**Depends on:** [any prerequisite, or "none"]

---

## Open Blockers

[List any blockers with their re-entry conditions. If none, write "None."]

---

## Parked Items

[List deferred items with their re-entry triggers. If none, write "None."]

---

## Context Notes

[Anything an AI agent picking this up cold needs to know — key decisions made, risks flagged, constraints in force. Keep to bullet points.]
```

After writing STATUS.md, confirm with one sentence what was captured.
