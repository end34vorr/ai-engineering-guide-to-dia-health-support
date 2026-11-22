# Week 3 — Data Inventory: Guide to Dia

**Pathway:** A (PDF/TXT). **Data type:** unstructured text. Pathways B–F are not used.
**Notebook:** `notebooks/week3_pipeline.ipynb`. **Source registry:** Cell 4B.
Fields marked `[FILL]` come from your Colab run. Do not guess them; write `N.V.` (Not Verified) if you cannot establish a value.

## A1 — Requirement-to-data mapping

| Requirement ID | Needed information / feature | Candidate source | Why usable / constraint |
|---|---|---|---|
| FR3 / FR4: explain factors with citations | Risk-factor explanations, testing guidance, with page-level source identity | S01 MoH CPG 6th ed.; S02 WHO fact sheet; S03 CDC risk-test page | Official authors; PDF pagination for S01 only. S02/S03 are web pages saved as print-PDF, so cite by URL. Rights not verified. |
| RAI2 / NFR6: grounding, reproducible update | Source ID, URL, date, checksum per chunk | Same sources + manifest | SHA-256 per source and output; web pages change over time, so accessed date matters. |
| FR2 / NFR3: calculator | ModAsian FINDRISC point table and category cut-offs | Lim et al. 2020 (not in the retrieval corpus) | Structured rules, transcribed and double-checked by hand (open item OI-1); copyrighted article, so cite, do not copy. |

## A4 — Inventory table

| Source ID | Filename / URL | Type and purpose | Version / date / owner | Access / rights | Inspection evidence |
|---|---|---|---|---|---|
| **S01** | `moh_cpg_t2dm_6th_ed.pdf` — URL `[FILL]` | PDF guideline; retrieval corpus (risk factors, screening, tests) | 6th ed., Dec 2020 (cover page); Ministry of Health Malaysia with Academy of Medicine Malaysia, MEMS, FMSA, Diabetes Malaysia | Publicly downloadable; licence/redistribution **N.V.**; do not commit the PDF | Pages `[FILL]`; extracted chars `[FILL]`; near-empty pages `[FILL]`; sample page `[FILL]` |
| **S02** | `who_diabetes_fact_sheet.pdf` — https://www.who.int/news-room/fact-sheets/detail/diabetes | Web page saved as PDF; retrieval corpus (global facts, definitions) | Living page; date on page `[FILL]`; WHO | Publicly viewable; reuse terms **N.V.** | Pages `[FILL]`; chars `[FILL]`; sample `[FILL]` |
| **S03** | `cdc_about_prediabetes_risk_test.pdf` — https://www.cdc.gov/prediabetes/risktest/about-the-test.html | Web page saved as PDF; retrieval corpus (what a risk test measures; US context) | Living page; date on page `[FILL]`; CDC / ADA | Publicly viewable; reuse terms **N.V.** | Pages `[FILL]`; chars `[FILL]`; sample `[FILL]` |
| **S04** (not ingested) | Lim et al. 2020, doi:10.1016/j.pcd.2020.02.008 | Journal article; calculator specification | 2020; Elsevier | Article rights apply; transcribe the scoring table only for citation and testing | Table transcribed and double-checked `[FILL: who/when]` |

## Notes for later topics
- **Timeliness:** S01 is the 6th edition (Dec 2020). Before relying on it, check whether a newer edition has been published; none was found in a November 2026 search, but that is not proof.
- **Consistency risk to check in Topic 4:** versions of CDC risk-test pages found online quote different prevalence figures (for example 88 million vs 98 million US adults with prediabetes), so record the exact page version you saved.
- **Do not commit** `data/raw/` or the chunk text (`prepared_text.jsonl`) until rights are verified. The manifests (hashes, URLs, dates) are safe to commit.
