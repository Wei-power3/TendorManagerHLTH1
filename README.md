# Tender Monitor v1

A personal procurement intelligence tool for researching healthcare IT contracts awarded in Germany and Italy.

## What it does

Tender Monitor pulls contract award data from the EU's public procurement database (TED — Tenders Electronic Daily), filters it to healthcare IT notices in Germany and Italy, and serves a browser-based query interface. The tool answers one question: who has been winning healthcare IT contracts in DE and IT over the past three years, under what requirements, and at what value?

## How it works

There is no backend server. A Python script downloads pre-built CSV files from the TED open data portal and converts them into a single `data.json` file. A static frontend hosted on GitHub Pages loads that file and runs all filtering in the browser. Refreshing the data means re-running the script and pushing to GitHub.

```
TED CSV files → Python script → data.json → GitHub Pages → browser
```

## Who it is for

v1 is a single-user research tool. No authentication, no user accounts, no multi-user features.

## Scope

**In scope for v1:**
- Keyword search across notice titles and winner names
- Filter by country (DE or IT), date range, and notice type
- Award data: winner name, contract value, award date, buyer name
- CSV download of filtered results
- Manual data refresh via script

**Out of scope for v1:**
- Side-by-side tender comparison
- Live upcoming tenders feed
- Scheduled or automatic data refresh
- Any form of user authentication

## Data source

TED CSV open data, published by the European Commission.
URL: https://data.europa.eu/data/datasets/ted-csv?locale=en
Files used: `export_CAN_2023.csv`, `export_CAN_2022.csv` (Contract Award Notices)

## CPV scope

The tool filters on three CPV divisions:
- `72xxxxxxx` — IT services
- `48xxxxxxx` — Software packages and information systems
- `85xxxxxxx` — Health and social work services

## Tech stack

- Data processing: Python, pandas
- Frontend: HTML, CSS, vanilla JavaScript
- Hosting: GitHub Pages
- Data format: static JSON file bundled with the frontend

## Setup

_To be completed when the build starts._

## Refresh data

_To be completed when the build starts._

## Project status

v1 — in build planning. Data source validated April 2026.
