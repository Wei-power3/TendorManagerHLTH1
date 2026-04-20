# Scripts

This folder will contain the Python ingestion script that produces `data.json` from the TED CSV source files.

## Status

Not yet written. Planned for the first build sprint.

## What the script will do

1. Load `export_CAN_2022.csv` and `export_CAN_2023.csv` from a local `/data/raw/` folder
2. Filter rows to ISO_COUNTRY_CODE in [DE, IT]
3. Filter rows to CPV codes starting with 72, 48, or 85
4. Drop cancelled notices (CANCELLED = 1)
5. Select and rename the output fields per the data dictionary
6. Write the result to `frontend/data.json`

## How to refresh data

1. Download the latest TED CAN CSV files from https://data.europa.eu/data/datasets/ted-csv?locale=en
2. Place them in `/data/raw/`
3. Run the ingestion script
4. Commit and push `frontend/data.json` to GitHub
5. GitHub Pages will redeploy automatically
