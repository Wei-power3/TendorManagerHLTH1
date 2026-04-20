# Data dictionary

This document defines the fields in `data.json` — the static data file that powers the Tender Monitor frontend.

---

## Source

TED CSV open data — Contract Award Notices (CAN).
Published by the European Commission Directorate-General for Internal Market.
URL: https://data.europa.eu/data/datasets/ted-csv?locale=en
Documentation: TED(csv)_data_information_v3.6.1 (available at the same URL)

Files ingested for v1:
- `export_CAN_2022.csv`
- `export_CAN_2023.csv`

---

## Filters applied during ingestion

| Filter | Value |
|---|---|
| Country | DE (Germany) and IT (Italy) only |
| CPV division | 72xxxxxxx, 48xxxxxxx, 85xxxxxxx |
| Cancelled notices | Excluded (CANCELLED = 1 dropped) |

---

## Fields

### `notice_id`
Source: `ID_NOTICE_CAN`
Unique identifier for the contract award notice. Format is a numeric string. Use this to look up the full notice on TED via the `notice_url` field.

### `notice_url`
Source: `TED_NOTICE_URL`
Direct URL to the full notice on the TED website. TED hosts notices for five years after publication — older notices may return a 404.

### `year`
Source: `YEAR`
Year the notice was published on TED. Integer.

### `winner_name`
Source: `WIN_NAME`
Official name of the company or organisation that won the contract, as submitted by the contracting authority. In the original language of the notice (German or Italian). May contain multiple winners separated by `---` if the contract was awarded to a group.

### `contract_value_eur`
Source: `AWARD_VALUE_EURO`
Total final contract value in EUR, excluding VAT. May be empty if the contracting authority did not disclose the value, or if the notice covers a framework agreement where individual call-off values are not specified. See `AWARD_VALUE_EURO_FIN_1` in the TED codebook for the imputed fallback value (not used in v1).

### `award_date`
Source: `DT_AWARD`
Date the contract was awarded. Format: DD-MON-YY (e.g. 15-MAR-23). May be empty for older notices.

### `buyer_name`
Source: `CAE_NAME`
Official name of the contracting authority. In the original language of the notice.

### `country`
Source: `ISO_COUNTRY_CODE`
Two-letter ISO country code of the contracting authority. In v1, only DE and IT are present.

### `cpv_code`
Source: `CPV`
Main Common Procurement Vocabulary code for the contract. Eight-digit numeric string (e.g. `72000000`). The first two digits identify the division. See https://ted.europa.eu/en/simap/cpv for the full CPV taxonomy.

### `title`
Source: `TITLE`
Contract title as submitted by the contracting authority. In the original language of the notice (German or Italian). May be empty for some older notices.

---

## Known data quality issues

These are documented in the TED CSV codebook and apply to this dataset:

- `WIN_NAME` is empty for some notices where the contracting authority did not complete section V.2.3 of the award form.
- `contract_value_eur` is empty or zero for framework agreements, where the total value is not required to be disclosed at award time.
- Some notices contain multiple awards per notice (one row per award in the source CSV). In `data.json` these are preserved as separate records sharing the same `notice_id`.
- Data quality is lower for notices published before 2017, when TED moved to XSD version 2.0.9. v1 uses 2022 and 2023 data only, so this is not a concern for the current dataset.
- `DT_AWARD` format is inconsistent across older notices. For 2022–2023 data it is reliably DD-MON-YY.

---

## What is not in `data.json`

These fields exist in the source CSV but are excluded from `data.json` to keep the file size manageable:

- `WIN_ADDRESS`, `WIN_TOWN`, `WIN_POSTAL_CODE` — winner address details
- `CAE_ADDRESS`, `CAE_TOWN`, `CAE_POSTAL_CODE` — buyer address details
- `CRIT_CRITERIA`, `CRIT_WEIGHTS` — award criteria text (unstructured, often in original language)
- `NUMBER_OFFERS` — number of bids received
- All lot-level fields (`ID_LOT`, `ID_LOT_AWARDED`)
- All framework agreement fields (`B_FRA_AGREEMENT`, `FRA_ESTIMATED` etc.)

These fields are accessible via the `notice_url` link to the full TED notice.
