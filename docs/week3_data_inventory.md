# Week 3 — Data Inventory: Guide to Dia

**Pathway:** A (PDF/TXT). **Data type:** unstructured text. Pathways B–F are not used.
**Notebook:** `notebooks/week3_pipeline.ipynb`. **Source registry:** Cell 4B.
Fields marked `[FILL]` come from your Colab run. Do not guess them; write `N.V.` (Not Verified) if you cannot establish a value.

## A1 — Requirement-to-data mapping

| Requirement ID | Needed information / feature | Candidate source | Why usable / constraint |
|---|---|---|---|
| FR6, FR7 / RAI2: grounded recap and answers with citations | Risk factors, prevention habits, testing guidance with source identity | S01 MoH CPG 6th ed.; S02 WHO fact sheet | Official authors; PDF pagination for S01 only. S02 is a web page saved as print-PDF, so cite by URL. Rights not verified. |
| FR3, FR6, FR7: Malaysian food and plate guidance; goal values | Malaysian Healthy Plate (Suku-Suku Separuh), fruit and vegetable servings, added-sugar advice | S03 Malaysian Dietary Guidelines 2020 | Official MOH document; edition year to confirm (cited as 2020 and 2021); pagination stable. Goal values must be extracted from it (OI-10). |
| FR8 / RAI6 / NFR8: Malaysian testing route | Who is eligible for free screening, what to bring, where to go; with page dates | S04 ProtectHealth PeKa Sihat; S05 MyGov PeKa B40 | Living pages that change (PeKa B40 was renamed PeKa Sihat), so access date and re-check are required. Nothing yet for under-40 Klinik Kesihatan screening (OI-7). |
| FR3: activity goal | Adult physical-activity recommendation | Not yet identified (OI-9) | Do not take the value from the CDC DPP page, which describes a US programme; find a Malaysian or WHO activity guideline. |
| FR2 / NFR3: calculator | ModAsian FINDRISC point table and category cut-offs | Lim et al. 2020 (not in the retrieval corpus) | Structured rules, transcribed and double-checked by hand (OI-1); copyrighted article, so cite, do not copy. |
| RAI2 / NFR6 / NFR8: reproducible update | Source ID, URL, date, checksum per chunk | All sources + manifest | SHA-256 per source and output; accessed date matters for web pages. |

## A4 — Inventory table

| Source ID | Filename / URL | Type and purpose | Version / date / owner | Access / rights | Inspection evidence |
|---|---|---|---|---|---|
| **S01** | `moh_cpg_t2dm_6th_ed.pdf` — URL `[FILL]` | PDF guideline; retrieval corpus (risk factors, prevention, testing) | 6th ed., Dec 2020 (cover page); Ministry of Health Malaysia with Academy of Medicine Malaysia, MEMS, FMSA, Diabetes Malaysia | Publicly downloadable; redistribution **N.V.**; do not commit the PDF | Pages `[FILL]`; chars `[FILL]`; near-empty pages `[FILL]`; sample page `[FILL]` |
| **S02** | `who_diabetes_fact_sheet.pdf` — https://www.who.int/news-room/fact-sheets/detail/diabetes | Web page saved as PDF; retrieval corpus (global facts, prevention) | Living page; date on page `[FILL]`; WHO | Publicly viewable; reuse terms **N.V.** | Pages `[FILL]`; chars `[FILL]`; sample `[FILL]`; access date `[FILL]` |
| **S03** | `moh_malaysian_dietary_guidelines_2020.pdf` — URL `[FILL]` | PDF guideline; retrieval corpus (Healthy Plate, servings, sugar) | Edition year `[FILL: from cover page]`; National Coordinating Committee on Food and Nutrition, MOH Malaysia | Publicly available; redistribution **N.V.** | Pages `[FILL]`; chars `[FILL]`; near-empty pages `[FILL]`; sample `[FILL]` |
| **S04** | `protecthealth_peka_sihat.pdf` — https://protecthealth.com.my/peka-b40-eng/ | Web page saved as PDF; retrieval corpus (screening pathway) | Living page; date on page `[FILL]`; ProtectHealth Corporation (MOH) | Publicly viewable; reuse terms **N.V.** | Pages `[FILL]`; chars `[FILL]`; sample `[FILL]`; access date `[FILL]` |
| **S05** | `mygov_peka_b40.pdf` — https://www.malaysia.gov.my/en/categories/aid-welfare-and-assistance/healthcare-aid/peka-b40 | Web page saved as PDF; retrieval corpus (eligibility and procedure) | Living page; date on page `[FILL]`; Government of Malaysia portal | Publicly viewable; reuse terms **N.V.** | Pages `[FILL]`; chars `[FILL]`; sample `[FILL]`; access date `[FILL]` |
| **S06** (not ingested) | Lim et al. 2020, doi:10.1016/j.pcd.2020.02.008 | Journal article; calculator specification | 2020; Elsevier | Article rights apply; transcribe the scoring table only for citation and testing | Table transcribed and double-checked `[FILL: who/when]` |

## Notes for later topics
- **Timeliness:** S01 is the 6th edition (Dec 2020). Before relying on it, check whether a newer edition has been published; none was found in a November 2026 search, but that is not proof.
- **Timeliness risk to check in Topic 4:** S04/S05 describe a programme that has been renamed (PeKa B40 → PeKa Sihat) and whose benefits are being expanded, so record the exact access date and compare the two pages for contradictions; also check S03 for a newer edition (for example eligibility age or name).
- **Known gaps (OI-7):** no official source yet for Klinik Kesihatan screening fees or eligibility for under-40s, or for the National Health Screening Initiative. Do not fill these from news articles; find the MOH page or leave them as N.V.
- **Not in the corpus:** the CDC National DPP and CDC risk-test pages describe US programmes; they are used as evidence in the README, not as retrieval sources, so the coach cannot quote US targets as Malaysian guidance.
- **Open gaps:** an official physical-activity guideline (OI-9) and goal values (OI-10).
- **Do not commit** `data/raw/` or the chunk text (`prepared_text.jsonl`) until rights are verified. The manifests (hashes, URLs, dates) are safe to commit.
