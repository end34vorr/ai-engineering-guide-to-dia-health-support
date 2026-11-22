# Week 4 — Data Quality & Bias Report: Guide to Dia

**How to finish this report:** run the three *Week 4 audit* cells at the end of `notebooks/week3_pipeline.ipynb` on your real PDFs, then replace every `[FILL]` with the printed value. Use **N.V.** (Not Verified) where a check was impossible. Label claims as *Observed*, *Inferred risk* or *Not Verified*. Delete this paragraph before submitting. A decision (Section 8) must follow from your numbers, not from the expectation written here.

## 1. Project and Data Overview
- **Project:** Guide to Dia — explains a rule-based diabetes risk score in plain language, with citations, to adults aged 18–35 in Malaysia.
- **Two Topic 2 requirements audited:** **FR4 / RAI2** (every statement needs a verifiable citation → needs source identity and page on every chunk) and **FR3 / FR5 with NFR2** (explanations quote correct clinical thresholds → needs numeric cut-offs preserved intact).
- **Pathway / data type:** A — PDF/TXT, unstructured text.
- **Audited output:** Topic 3 **Part C** output `data/pipeline_v1/documents/prepared_text.jsonl`; **matching manifest:** `data/pipeline_v1/documents/pipeline_manifest.json` (Part B copy and `data/week3_manifest.json` used for lineage).
- **Population and sample:** inspection unit = chunk. Population `[FILL: total chunks]`. Automatic checks cover **all** chunks. Manual accuracy spot-check: 15 chunks drawn with a fixed random seed (42) from the whole population (`data/week4_spotcheck_sample.csv`). A 15-item sample does **not** establish quality of the whole dataset.
- **Limitations:** web pages S02/S03 are print-PDFs saved on `[FILL: date]`; keyword-based coverage counts show presence only.

## 2. Five-Dimension Quality Scorecard

| Dimension | Finding (Observed) | Sample / evidence | Impact on intended task | Decision |
|---|---|---|---|---|
| Completeness | Pages with text / pages with chunks per source: S01 `[FILL]`, S02 `[FILL]`, S03 `[FILL]`. Near-empty pages: `[FILL]` | Cell W4-A "Completeness" | Missing pages mean missing guidance, so the model may have nothing to cite (FR6 fallback) | `[Pass / Warning / Review]` |
| Consistency | Distinct record schemas `[FILL]` (expect 1); unregistered source IDs `[FILL]`; bad page types `[FILL]` | Cell W4-A "Consistency" | Mixed schemas would break citation building | `[FILL]` |
| Accuracy | Automatic: chunk text is a faithful substring of extracted page text `[FILL]`%. **Manual:** `[FILL]` of 15 sampled chunks match the printed source page; mismatches `[FILL]` | Cell W4-A; `week4_spotcheck_sample.csv` (reviewer: you) | The automatic check proves chunking fidelity, **not** extraction fidelity; a manual mismatch would put wrong text behind a citation | `[FILL]` (**N.V.** until the manual check is done) |
| Timeliness | S01 published 2020-12, age `[FILL]` years; S02/S03 living pages accessed `[FILL]`. Newer CPG edition: **N.V.** (none found in a Nov 2026 web search; not proof) | Cell W4-A "Timeliness"; inventory | Out-of-date thresholds or recommendations would be cited as current | `[FILL]` |
| Uniqueness | Duplicate chunk groups `[FILL]`; records inside them `[FILL]` | Cell W4-A "Uniqueness" | Duplicates bias retrieval toward repeated text | `[FILL]` |

## 3. Validation Findings

| Rule | Risk / requirement | Condition and evidence source | Tested | Observed result | Failure example | Action if failed |
|---|---|---|---|---|---|---|
| **V1** | A chunk without source, URL, locator or page cannot be cited (FR4, RAI2) | Every record has non-empty `source_id`, `title`, `url`, `locator`; `page` present where pagination is stable. Cell W4-B | `[FILL]` chunks | `[FILL]` pass / `[FILL]` fail | `[FILL]` record ID or none | **Hard fail:** do not index the chunk until fixed |
| **V2** | A split or flattened clinical threshold could be quoted wrongly (FR3, FR5, NFR2; Part B chunking risk) | Every numeric threshold matched by `NUM_RE` (for example `6.5%`, `7.0 mmol/L`, `27.5 kg/m2`, `90 cm`) appears whole inside some chunk of its source. Cell W4-B | `[FILL]` thresholds | `[FILL]` intact / `[FILL]` broken | `[FILL]` source + threshold | **Review:** inspect each case manually; adjust chunking only if the case is real |

*Limit:* V2 finds numbers split by chunking. It does not detect numbers misread by PDF extraction, or a number separated from its row label in a table.

## 4. Duplicate, Anomaly and Coverage Findings
- **Duplicates / anomalies:** `[FILL: from Uniqueness and near-empty-page output; investigate before removing anything]`
- **Coverage of two in-scope categories** (keyword approximation, chunks matching): 

| In-scope category | Expected evidence | Observed (S01 / S02 / S03) | Possible downstream effect |
|---|---|---|---|
| C1 Risk factors (family history, weight, waist, activity, blood pressure) | Explanations of why each calculator input matters | `[FILL]` / `[FILL]` / `[FILL]` chunks | A thin category means FR3 explanations may fall back to "not found" |
| C2 Testing and next steps (HbA1c, fasting glucose, screening, see a clinician) | Basis for the single next step (FR5) | `[FILL]` / `[FILL]` / `[FILL]` chunks | Missing guidance means unsupported next steps |

- *Optional third category C3 (Asian BMI/waist thresholds):* `[FILL]`. 
- **Limits of evidence:** keyword hits show topic presence, not that the guidance is correct or sufficient.

## 5. Bias Assessment
- **Plausible risk (Inferred, not proven):** *representation / source-mix bias.* Only S01 is Malaysian; S03 reflects a US population and notes that Asian Americans are at higher risk at lower weights. **Source or preparation decision** (corpus mixes S01–S03) → **gap/skew** (retrieval may return US-oriented risk statements for a Malaysian user) → **effect** (explanation could cite cut-offs or prevalence that differ from Malaysian guidance) → **proposed control:** prefer S01 in retrieval when sources overlap, state which source a statement comes from, and test with RAI4 stratified scenarios later.
- **Pipeline-induced risk to check:** print-PDFs of web pages may drop collapsed sections or add navigation text. Compare each saved web PDF with the live page, and record the result: `[FILL]` (**N.V.** if not done).
- Not claimed: no statement about model fairness or system accuracy is made here.

## 6. Privacy, Rights and Governance

| Question | Finding |
|---|---|
| Provenance and version | S01: 6th ed., Dec 2020, URL `[FILL]`, SHA-256 in manifest. S02/S03: living pages, accessed `[FILL]`, SHA-256 in manifest. |
| Rights and permissions | Use for private study: `[FILL]`. Redistribution in a public repository: **N.V.** for S01–S03. |
| Privacy / sensitive data | Source documents contain no personal data (**Not Verified** by full read; spot-check only). User answers (health history) are not in this pipeline and are sensitive; design rule is session-only, no identifiers (RAI3). |
| Consent / collection basis | Not applicable to public documents; applicable later to user inputs (open item OI-3 on PDPA 2010). |
| Ongoing maintenance | Owner: project author. Revalidate when a new CPG edition appears, when a web page changes, or when a checksum changes. |

**Safeguard / required action:** raw PDFs and chunk text are git-ignored until rights are confirmed; manifests (URL, date, SHA-256) are committed instead.

## 7. Mitigation and Impact on Requirements

| Priority | Action | Owner / timing | Requirements affected | Risk that remains |
|---|---|---|---|---|
| 1 | Verify licence and redistribution terms for S01–S03; record the exact S01 URL | Author, before chunks are committed or the project is shared | FR4, RAI2 (provenance) | Terms may forbid redistribution; then only manifests can be public |
| 2 | Resolve every V2 break and near-empty page (manual repair or documented exclusion); complete the 15-item manual accuracy check | Author, before Topic 5 retrieval work | FR3, FR5, NFR2 | Table content may stay unreliable; OCR not in V1 |
| 3 (optional) | Add a source-priority rule (S01 first) and check for a newer CPG edition | Author, Topic 5 | RAI4 | Newer guidance may change thresholds |

## 8. Quality-Gate Decision

**Decision:** `[GO / GO WITH CONDITIONS / HOLD — choose after your run]` for the audited scope (`prepared_text.jsonl` from S01–S03, English text, V1 corpus).

**How to choose (rule applied to your numbers):**
| Decision | Use when |
|---|---|
| GO | V1 = 100% pass, V2 has 0 unresolved breaks, manual spot-check shows no mismatch, **and** rights/version are verified for the intended use. |
| GO WITH CONDITIONS | V1 and V2 are clean or fully explained, but at least one item is N.V. (for example redistribution rights, manual check not finished, newer edition unchecked). List each as a condition with owner and re-review trigger. |
| HOLD | Any source has no extractable text, V1 fails for records that cannot be repaired, V2 breaks remain unresolved, or manual spot-check shows extraction errors. |

*Prediction to confirm, not a result:* rights for redistribution cannot be verified from inside the notebook, so GO WITH CONDITIONS is the most likely honest outcome; do not pick it unless your numbers support it.

- **Evidence points (at least two):** `[FILL: V1 result]`, `[FILL: V2 result]`, `[FILL: manual spot-check x/15]`.
- **Two priority investigations:** see Section 7, items 1 and 2.
- **Residual risks:** `[FILL]`.
- **Re-review triggers:** new CPG edition; change of any source checksum; any V1/V2 failure after re-ingestion; change in chunking configuration.

## Appendix — evidence used
`data/pipeline_v1/documents/pipeline_manifest.json`, `data/week3_manifest.json`, `data/week4_audit_summary.json` (notebook output), `data/week4_spotcheck_sample.csv` (completed by you), `docs/week3_data_inventory.md`, `docs/week3_pipeline_decisions.md`.
