---
name: fetch_and_extract_anthropic_labor_report
description: >
  Download the Anthropic report "Labor market impacts of AI: A new measure and
  early evidence", extract text from the PDF, and save normalized outputs for
  downstream analysis.
---

# Fetch and extract Anthropic labor market report

This skill downloads the Anthropic report page, finds the PDF, saves it locally, extracts text, and produces structured artifacts for later research.

## Source

- Report page: https://www.anthropic.com/research/labor-market-impacts
- Expected report title: Labor market impacts of AI: A new measure and early evidence

## Expected outputs

Save files under:

- `data/reports/anthropic/labor_market_impacts_of_ai_2026-03-05.pdf`
- `data/reports/anthropic/labor_market_impacts_of_ai_2026-03-05.txt`
- `data/reports/anthropic/labor_market_impacts_of_ai_2026-03-05.json`
- `data/reports/anthropic/labor_market_impacts_of_ai_2026-03-05.summary.md`

## Steps

1. Open the Anthropic report page.
2. Find the PDF link.
   - Prefer a link with text `Read in PDF`
   - Otherwise use the first relevant `.pdf` link on the page
3. Download the PDF.
4. Extract text from the PDF.
   - Prefer `pymupdf`
   - Fallback to `pypdf`
5. Normalize the extracted text.
6. Save:
   - original PDF
   - raw text
   - metadata JSON
   - short markdown summary
7. Print a short execution summary with output paths and page count.

## Required metadata in JSON

Include at least:

- `title`
- `source_page`
- `pdf_url`
- `publication_date`
- `extracted_at`
- `page_count`
- `text_length_chars`
- `pdf_path`
- `raw_text_path`

## Implementation

Run:

```bash
python scripts/fetch_and_extract_anthropic_labor_report.py
```
