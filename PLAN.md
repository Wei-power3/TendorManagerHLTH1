# Tender Monitor v1 — approved plan

Version: 1.0
Date: April 2026
Status: Approved — build ready

---

## Product summary

Tender Monitor v1 is a personal procurement intelligence tool. It ingests TED contract award notice CSV files for Germany and Italy, converts them to a static JSON file, and serves a browser-based query interface via GitHub Pages. It answers one question: who has been winning healthcare IT contracts in DE and IT over the past three years, under what requirements, and at what value?

---

## Target user

Solo healthcare IT strategy analyst. Non-technical on the frontend, able to run a Python script. Has domain knowledge of clinical vocabulary that substitutes for CPV code expertise.

---

## Problem statement

TED's search and filter interface requires CPV code fluency. A user without that knowledge cannot slice by country, clinical topic, and date in one move. Winners are recorded in separate award notices linked by procedure ID — not surfaced alongside the contract notice by default. The result is a two-hour manual process to answer one research question.

---

## Architecture

Static. No backend server.

```
TED open data portal
        │
        │  manual download (once per refresh cycle)
        ▼
CSV files (export_CAN_2022.csv, export_CAN_2023.csv)
        │
        │  Python script (run locally)
        ▼
data.json  ──────────────────────────────┐
        │                                │
        │  git push                      │
        ▼                                │
GitHub repo                              │
        │                                │
        │  GitHub Pages deploy           │
        ▼                                │
Static frontend (HTML/CSS/JS) ◄──────────┘
        │
        │  loads data.json, filters in browser
        ▼
Query form → Results table → CSV download
        │
        ▼
      User
```

All filtering happens client-side in JavaScript. No API calls at runtime.

---

## Design direction

Clean document aesthetic. Light background, readable table, generous whitespace. The results table is the product — winner name and contract value are the largest, most prominent elements in every results row.

The empty state shows three example queries so a first-time user is never facing a blank interface.

---

## Data fields

Source fields from TED CSV → output fields in data.json and UI:

| CSV field | Output field | Notes |
|---|---|---|
| `WIN_NAME` | `winner_name` | Company or organisation that won the contract |
| `AWARD_VALUE_EURO` | `contract_value_eur` | Final contract value in EUR |
| `DT_AWARD` | `award_date` | Date the contract was awarded |
| `CAE_NAME` | `buyer_name` | Contracting authority name |
| `ISO_COUNTRY_CODE` | `country` | DE or IT |
| `CPV` | `cpv_code` | Main CPV code |
| `TITLE` | `title` | Contract title (in original language) |
| `TED_NOTICE_URL` | `notice_url` | Direct link to full notice on TED |
| `ID_NOTICE_CAN` | `notice_id` | Unique notice identifier |
| `YEAR` | `year` | Publication year |

---

## CPV filter

Three divisions included:

| Division | Covers |
|---|---|
| `72xxxxxxx` | IT services |
| `48xxxxxxx` | Software packages and information systems |
| `85xxxxxxx` | Health and social work services |

Decision: cast wide on CPV, rely on keyword search in the UI to surface relevant results within that universe.

---

## Definition of done

v1 is complete when all of the following are true:

- [ ] Python script ingests 2022 and 2023 TED CAN CSV files and produces `data.json`
- [ ] `data.json` is filtered to DE and IT, CPV divisions 72, 48, 85
- [ ] Every record in `data.json` includes: winner name, contract value, award date, buyer name, country, CPV code, title, notice URL
- [ ] Frontend loads `data.json` and renders a results table
- [ ] Keyword search filters results in under one second
- [ ] Country filter (DE / IT / both) works
- [ ] Date range filter works
- [ ] Notice type filter works (contract award / all)
- [ ] Filtered results downloadable as CSV
- [ ] Tool is live at a GitHub Pages URL
- [ ] The owner can answer three named research questions without opening TED directly

---

## What is explicitly out of scope for v1

- Side-by-side tender comparison
- Live upcoming tenders feed (notices with future deadlines)
- Scheduled or automatic data refresh
- Admin UI for triggering a refresh
- Multi-user access or authentication
- Machine translation of notice titles
- AI-powered semantic search (keyword search only in v1)

---

## Implementation order

1. Write Python ingestion script — load CSVs, apply CPV and country filters, output `data.json`
2. Validate `data.json` — spot-check 10 records manually against TED website
3. Build frontend — query form, results table, filter panel
4. Wire frontend to `data.json` — client-side filtering in JavaScript
5. Add CSV download
6. Deploy to GitHub Pages
7. Run acceptance test — answer three named research questions from the UI

---

## Open questions at time of approval

- 2024 data: TED CSV currently runs through 2023. 2024 notices require the TED API or XML bulk download. Deferred to v1.5.
- CPV expansion: additional divisions (e.g. `33xxxxxxx` — medical equipment) may be added in v1.5 based on research findings.

---

## Key decisions log

| Decision | Choice | Date |
|---|---|---|
| Data source | TED CSV open data (not API) | April 2026 |
| Hosting | GitHub Pages — static, no server | April 2026 |
| Database | None — static JSON file | April 2026 |
| Refresh model | Manual: run script, push to GitHub | April 2026 |
| Language handling | Original languages (DE/IT) — no translation | April 2026 |
| Aesthetic direction | Clean document — light, readable | April 2026 |
| CPV scope | Divisions 72, 48, 85 | April 2026 |
| Comparison feature | Out of scope for v1 | April 2026 |
