# UK Startups — Data Cleaning Pipeline (1,500 companies)

Turns messy, real-world scraped company data into clean, analysis-ready data — with every
cleaning decision documented and nothing silently deleted.

## The problem

Raw scraped or exported business data is rarely usable as-is: the same city spelled five
different ways, duplicate entries under slightly different company names, ambiguous fields
that mix "city" and "country" values. Before this data can power a dashboard, a mailing list,
or a market analysis, it needs a cleaning pass that a spreadsheet formula alone can't do
reliably.

This project takes a real dataset of 1,500 UK-based startups (scraped with
[eu-startups-lead-scraper](https://github.com/leonangheluta-cyber/eu-startups-lead-scraper))
and runs it through a full cleaning pipeline, turning inconsistent raw data into a
structured, verified dataset ready for analysis.

## What it does

- **Deduplication** — removes exact duplicate entries, and separately flags likely
  near-duplicates (same company listed twice under a slightly different name) for manual
  review instead of guessing which row to drop
- **Location standardization** — fixes casing, splits compound values like
  `"Birmingham, England, United Kingdom"` down to the actual city, and corrects spelling
  variants (`"Scottland"` → `"Scotland"`, `"Greater lonodn"` → `"London"`). Result: **205 → 184**
  unique, consistent city values
- **Transparent flagging over silent deletion** — rows where only a country/region was
  listed (not an actual city) are kept and flagged in a dedicated column, never dropped
  without a trace
- **Automated reporting** — generates 3 charts directly from the cleaned data: company
  distribution by city, founding-year trend, and funding-range breakdown

## Sample: before → after

| Raw `Based in`                        | Cleaned `Based in` |
|----------------------------------------|---------------------|
| `scottland`                            | Scotland            |
| `ST ALBANS`                            | St. Albans          |
| `Birmingham, England, United Kingdom`  | Birmingham          |
| `Greater lonodn`                       | London              |

*(illustrative example — not real dataset rows)*

## Output

- `Lead_cleaned.xlsx` — full cleaned dataset
- `images/top_cities.png`, `images/foundation_years.png`, `images/funding_distribution.png` —
  charts generated from the cleaned data

## Tech stack

Python · pandas · matplotlib

## How to run

```
git clone https://github.com/leonangheluta-cyber/data-cleaning-1500-uk-startups
cd data-cleaning-1500-uk-startups
pip install -r requirements.txt
```

Place a source file with the same structure (`Name`, `Based in`, `Description`,
`Foundation year`, `Funding`) named `Lead_british-startups_companies.xlsx` in the project
folder, then run `Cleaning_1500_uk_startups.ipynb` top to bottom.

## Notes

The full raw and cleaned datasets (1,500 rows) aren't included in this repo to avoid
redistributing scraped content — only a 25-row sample (`samples_data.xlsx`) is provided for
reference.