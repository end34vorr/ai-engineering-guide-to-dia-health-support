# \# Week 4 — Data Quality \& Bias Report: Guide to Dia

# 

# \## 1. Project and Data Overview

# \- \*\*Project:\*\* Guide to Dia — explains a rule-based diabetes risk score in plain language to adults aged 18–35 in Malaysia.

# \- \*\*Two Topic 2 requirements audited:\*\* FR4 / RAI2 (every statement needs a verifiable citation) and FR3 / FR5 with NFR2 (explanations must quote correct clinical thresholds).

# \- \*\*Pathway / data type:\*\* A — PDF/TXT, unstructured text.

# \- \*\*Audited output:\*\* Topic 3 Part C output `data/pipeline\_v1/documents/prepared\_text.jsonl`. Matching manifest: `data/pipeline\_v1/documents/pipeline\_manifest.json`.

# \- \*\*Population and sample:\*\* Inspection unit is a chunk. Population is 2000 chunks. Automatic checks cover all 2000 chunks. Manual accuracy spot-check used 15 chunks drawn with a fixed random seed.

# \- \*\*Limitations:\*\* The WHO document (S02) is a print-PDF of a webpage, so some formatting might have been lost. Keyword-based coverage counts show topic presence only, not correctness.

# 

# \## 2. Five-Dimension Quality Scorecard

# 

# | Dimension | Finding (Observed) | Sample / evidence | Impact on intended task | Decision |

# |---|---|---|---|---|

# | Completeness | S01: 280/284 pages with text. S02: 85/88 pages. S03: 292/292 pages. | Cell W4-A | A few near-empty pages in S01 and S02 mean some charts or blank pages were skipped. This is acceptable. | Pass |

# | Consistency | 1 distinct record schema. 0 unregistered source IDs. 0 bad page types. | Cell W4-A | High consistency means the citation builder will not break. | Pass |

# | Accuracy | Automatic: 100% of chunks are faithful substrings of extracted text. Manual: 15/15 match. | Cell W4-A; Spot-check CSV | No wrong text is behind any citation. | Pass |

# | Timeliness | S01 published Dec 2020 (5.9 years old). S02 and S03 are living pages or 2021. | Cell W4-A | Medical guidelines from 2020 are mostly still valid for basic screening, but a newer CPG could exist. | Warning |

# | Uniqueness | 0 duplicate chunk groups. 0 records in duplicate groups. | Cell W4-A | No duplicates mean the AI won't repeat the same sentence twice. | Pass |

# 

# \## 3. Validation Findings

# 

# | Rule | Risk / requirement | Condition and evidence source | Tested | Observed result | Failure example | Action if failed |

# |---|---|---|---|---|---|---|

# | \*\*V1\*\* | A chunk without source, URL, locator or page cannot be cited (FR4, RAI2) | Every record has non-empty source\_id, title, url, locator; page present where stable. Cell W4-B | 2000 chunks | 2000 passed / 0 failed | None | Hard fail if any fail |

# | \*\*V2\*\* | A split or flattened clinical threshold could be quoted wrongly (FR3, FR5, NFR2) | Every numeric threshold appears whole inside some chunk. Cell W4-B | 1628 thresholds | 1628 intact / 0 broken | None | Review manually if broken |

# 

# \*Limit:\* V2 finds numbers split by chunking. It does not detect numbers misread by PDF extraction.

# 

# \## 4. Duplicate, Anomaly and Coverage Findings

# \- \*\*Duplicates / anomalies:\*\* 0 duplicate chunks found. Near-empty pages were found in S01 (pages 1, 2, 283, 284) and S02 (pages 10, 17, 82). These are likely cover pages or images, so they do not affect the text quality.

# \- \*\*Coverage of two in-scope categories\*\* (keyword approximation, chunks matching): 

# 

# | In-scope category | Expected evidence | Observed (S01 / S02 / S03) | Possible downstream effect |

# |---|---|---|---|

# | C1 Risk factors | Explanations of why each calculator input matters | 156 / 90 / 224 chunks | Strong coverage across all sources. |

# | C2 Testing and next steps | Basis for the single next step (FR5) | 182 / 34 / 5 chunks | S03 has low coverage here, but S01 is very strong, so the AI can rely on S01 for next steps. |

# 

# \## 5. Bias Assessment

# \- \*\*Plausible risk (Inferred, not proven):\*\* Representation bias. Only S01 is Malaysian. S02 is global (WHO), and S03 is Malaysian dietary advice. Standard medical thresholds (like BMI) can differ for Asian populations. 

# \- \*\*Pipeline-induced risk:\*\* Print-PDFs of web pages might drop navigation text or tables.

# \- \*\*Proposed control:\*\* Prefer S01 (Malaysian CPG) in retrieval when sources overlap, and state which source a statement comes from. 

# 

# \## 6. Privacy, Rights and Governance

# \- \*\*Provenance and version:\*\* S01 is 6th edition, Dec 2020. S02 and S03 were accessed on 2026-11-22. SHA-256 hashes are recorded in the manifest.

# \- \*\*Rights and permissions:\*\* Used for private study. Redistribution in a public repository is Not Verified. The raw PDFs and chunk text are git-ignored until rights are confirmed.

# \- \*\*Privacy / sensitive data:\*\* The source documents contain no personal data. User answers (health history) are not in this pipeline and will be handled session-only.

# \- \*\*Safeguard / required action:\*\* Do not commit the raw PDFs to GitHub. Only commit the manifests and the pipeline code.

# 

# \## 7. Mitigation and Impact on Requirements

# 

# | Priority | Action | Owner / timing | Requirements affected | Risk that remains |

# |---|---|---|---|---|

# | 1 | Verify licence and redistribution terms for S01-S03 before making the project public. | Author, before the final submission | FR4, RAI2 | Terms may forbid sharing the raw PDFs. |

# | 2 | Check if a newer CPG edition exists before final deployment. | Author, Topic 5 | NFR8 | Newer guidance may change thresholds. |

# 

# \## 8. Quality-Gate Decision

# 

# \*\*Decision:\*\* GO WITH CONDITIONS for the audited scope (`prepared\_text.jsonl` from S01-S03, English text, V1 corpus).

# 

# \*\*Justification:\*\* 

# The automatic checks show a perfect result. V1 is 100% pass, V2 has 0 breaks, Accuracy is 100%, and duplicate count is 0. However, the redistribution rights for the PDFs are still Not Verified, and the MOH CPG is 5 years old. 

# 

# \*\*Conditions:\*\*

# 1\. The data can be used for the AI model, but the raw PDFs must remain private. Only the chunked JSONL and manifests will be public.

# 2\. Before final deployment, check for a newer CPG edition.

# 

# \*\*Re-review triggers:\*\* New CPG edition; change of any source checksum; any V1/V2 failure after re-ingestion.

