# Week 1 — Project Scope: Guide to Dia

**Project Name:** Guide to Dia
**Domain:** Public health / preventive health education (type 2 diabetes risk awareness)

## Problem Statement
Young adults in Malaysia often do not know they may be at risk of type 2 diabetes. Existing risk tools give a score without explaining why a factor matters or what to do next, and general chatbots may invent medical facts. Guide to Dia combines a deterministic rule-based calculator with a retrieval-grounded AI that explains the result in simple words, cites official documents, and gives one next step.

## Evidence of the Problem
- **Evidence 1:** About 50% of people with diabetes are undiagnosed. — *MOH et al., CPG Management of T2DM, 6th ed. (Dec 2020), Quick Reference Guide, key messages.*
- **Evidence 2:** About 84% of Malaysian adults aged 18–29 with diabetes were undiagnosed in NHMS 2023. — *The Star, "One in six Malaysians are diabetic, many unaware", 2 Aug 2025 (quoting the MOH Disease Control Division). Check the figure against the original NHMS 2023 report before the final proposal.*
- **Evidence 3 (context):** Guidance recommends risk-based screening of asymptomatic adults from age 30, earlier with risk factors. — *CPG 6th ed. Quick Reference Guide; The Star, 28 Oct 2026.*

## Initial Related Work (at least 3)
1. **MyDiabeticRisk** — Ab Zayin, Zainol, Nohuddin, Mohamed (2023), *Zulfaqar Journal of Defence Science, Engineering & Technology* 6(2). Key limitation: needs clinical inputs (glucose, blood pressure) and returns a prediction without guideline-grounded, cited explanation.
2. **ModAsian FINDRISC** — Lim HM, Chia YC, Koay ZL (2020), *Primary Care Diabetes* 14(5):494–500, doi:10.1016/j.pcd.2020.02.008; Doan L et al. (2023), *J Multidiscip Healthc* 16:439–449. Key limitation: score and category only; validation in adults (Vietnam study ≥ 25 years), so validity for ages 18–24 is not established.
3. **CDC/ADA Prediabetes Risk Test** — https://www.cdc.gov/prediabetes/risktest/. Key limitation: US population and prediabetes outcome; not aligned to Malaysian guidance; no personalised explanation.
4. **General-purpose AI chatbots.** Key limitation: may hallucinate; no guaranteed citation or scope control.

## Engineering Gap / Limitation
No reviewed approach combines a transparent deterministic risk calculation with plain-language explanations grounded in and cited to official health documents, under explicit medical-safety guardrails. This is a practical engineering gap, not a claim of research novelty.

## Initial Project Objectives
- **O1:** To design a hybrid assistant that calculates diabetes risk with a rule-based calculator and explains it in simple language using official health documents.
- **O2:** To develop it with retrieval, source citation, and strict medical-safety guardrails.
- **O3:** To evaluate it in terms of explanation groundedness, safety, readability, and latency.

## Target Users
Young adults aged 18–35 in Malaysia (university students, early-career professionals) without a prior diabetes diagnosis.

## Data Sources (at least 2)
- **Source 1:** MOH et al., CPG Management of T2DM, 6th ed. (Dec 2020) — https://www.moh.gov.my (exact PDF URL to be recorded when downloaded)
- **Source 2:** WHO Diabetes fact sheet — https://www.who.int/news-room/fact-sheets/detail/diabetes
- **Source 3:** CDC/ADA Prediabetes Risk Test, about page — https://www.cdc.gov/prediabetes/risktest/about-the-test.html
- **Calculator specification (not retrieval text):** Lim et al. 2020 (ModAsian FINDRISC scoring table).

## Similar to EduAdvisor
Document-grounded Q&A over public PDFs; retrieval + citations + guardrails; evaluated on groundedness, safety and latency.

## Different from EduAdvisor
- Adds a **deterministic calculator** whose output the language model may explain but never change.
- Domain is **health**, so scope is narrowed to risk awareness and the safety controls are stricter (red-flag redirect, no diagnosis/treatment).
- Primary output is a **risk explanation plus exactly one next step**, not open Q&A.
- Collects **sensitive personal answers**, so privacy is a design driver (no identifiers, no storage beyond the session).

## Why this health project is acceptable under the Week 1 scoping rule
The lab warns against medical diagnosis systems unless the scope is carefully constrained. This project (a) never diagnoses, (b) never recommends treatment, (c) restricts answers to retrieved official text, (d) redirects red-flag symptoms to a doctor, and (e) states on every result that only a blood test can diagnose. Residual risks are tracked in `docs/week2_requirements.md` (RAI1–RAI5).

## Responsible AI Risks (at least 2)
- **Risk 1 — Hallucination:** invents a threshold or advice → user acts on false medical information.
- **Risk 2 — False reassurance / alarm:** "low risk" delays testing; "high risk" causes distress.
- **Risk 3 — Privacy:** health history answers are sensitive; leakage to logs or third-party APIs harms the user.
- **Risk 4 — Fairness / validity:** sex- and ethnicity-specific cut-offs, and limited validation for ages 18–24, can mislead some users.
