# Week 3 — Pipeline Decisions, Stage Contracts and Traceability

Diagram: `docs/week3_pipeline_diagram.png`. Notebook: `notebooks/week3_pipeline.ipynb`.
Fields marked `[FILL]` are copied from your Colab output. Never type numbers you did not observe.

## 1. Requirements served
| Requirement | How this pipeline supports it |
|---|---|
| FR7 / RAI2 | Every chunk keeps `source_id`, `url`, `page`, `locator`, so later answers can cite a source. |
| FR3, FR6 / NFR2 | Numeric goal values and clinical thresholds (servings, minutes, HbA1c, glucose, BMI, waist) must stay whole inside one chunk. |
| FR8 / NFR8 / RAI6 | Programme pages (S04, S05) carry URL and access date so pathway details can be shown with a source date and re-checked. |
| NFR6 | Registry + manifests let one source be replaced and re-ingested without touching the others. |
| RAI3 | Source documents contain no personal data; no user or tracker data enters this pipeline. |

## 2. Selected sources
S01 MoH CPG 6th ed. (PDF), S03 Malaysian Dietary Guidelines 2020 (PDF), S02 WHO fact sheet, S04 ProtectHealth PeKa Sihat and S05 MyGov PeKa B40 (print-PDFs of web pages). Details: `week3_data_inventory.md`.

## 3. Part B comparison (same input, two approaches)

| Approach A: fixed-size 1000-char slices over the whole document | Approach B: page-aware sentence-packing (≤ 1000 chars, never crosses a page) | Observed evidence | Selected approach and trade-off |
|---|---|---|---|
| Course baseline; no page numbers | Keeps page number; splits at sentence ends | From Cell 5A1 table: chunks `[FILL]`; % with page A `[FILL]` vs B `[FILL]`; % mid-sentence start A `[FILL]` vs B `[FILL]`; % thresholds intact A `[FILL]` vs B `[FILL]` | **Expected choice: B**, because FR7/RAI2 need a page and FR3/FR6 need intact numeric targets and thresholds. Confirm with your numbers; if A wins on thresholds or B shows broken tables, say so. Accepted limitation: a sentence that spans a page break is split, and flattened tables may read badly. |

## 4. Chosen configuration
`method = page-aware`, `chunk_size = 1000` characters, whitespace-only cleaning (headers, footers and tables are **not** removed automatically). Initial values; revisit when retrieval is evaluated in a later topic.

## 5. Accepted limitations
- Scanned or figure-only pages produce no text; OCR is out of scope for V1 (pages listed by Cell 5A1 and Week 4 audit).
- Print-PDFs of web pages have unstable page numbers, so S02, S04 and S05 cite by URL (`page_is_stable = False`); record the date each page was saved.
- Table content may be flattened into running text; V2 counts broken numeric thresholds but cannot judge table meaning.
- Sentence splitting is a regular-expression heuristic, not a linguistic parser.

## 6. ETL vs ELT, batch vs streaming
- **ETL**: transform (clean, chunk) before storing the prepared records, because only the chunked form is used downstream and raw PDFs stay local.
- **Batch**: three small, slow-changing documents; re-run on demand when a source changes. Streaming adds nothing here.

## 7. Output locations
- Part B: `data/processed/prepared_text.jsonl`, `data/week3_manifest.json`
- Part C: `data/pipeline_v1/documents/prepared_text.jsonl`, `data/pipeline_v1/documents/pipeline_manifest.json`
- Cell 9 reports whether Part B and Part C outputs are byte-identical: result `[FILL]`.

## 8. Provenance and access caveats
Redistribution rights for S01–S03 are **Not Verified**. The repository ignores raw PDFs and chunk text; manifests (URLs, dates, SHA-256) are committed instead. Source versions for web pages are "living page", so the accessed date is part of the evidence.

## 9. Stage contracts

| Stage | Contract / evidence | Failure response |
|---|---|---|
| Ingest | Each registered file exists, is permitted, and has a SHA-256 | Stop for a missing input |
| Inspect | Text is extractable; page counts and near-empty pages reported | Stop if a whole source has no text; list near-empty pages |
| Prepare | Page-aware chunks, none empty, each with a page where pagination is stable | Stop on zero chunks |
| Validate | Output file exists, non-empty, JSONL readable, record count matches | Stop for missing or empty output |
| Record lineage | Every source maps to the output with a processing label; checksums re-verified (Cell 6B) | Stop if a mapping or checksum is missing |

## 10. Two failure points annotated on the diagram
1. **Scanned / near-empty page:** silent missing text → ungrounded answers. Response: flag pages, stop if an entire source is empty.
2. **Split numeric threshold / garbled table:** wrong cut-off could be quoted. Response: V2 check counts broken thresholds; review affected chunks before retrieval.

## 11. Added because of the Malaysian re-scope
- S04/S05 are short web pages, so chunk-boundary differences will be small; the main Week 4 risk for them is **timeliness and contradiction between pages**, not chunking. S03 is the largest new risk for chunking, because serving-size tables and the Healthy Plate figure may flatten badly in text extraction.
- Programme pages should be re-saved and re-ingested before each evaluation round (NFR8); the manifest checksum shows whether a page changed.
