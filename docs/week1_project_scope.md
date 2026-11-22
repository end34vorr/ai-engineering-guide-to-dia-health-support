# Week 1 — Project Scope: Guide to Dia

**Project Name:** Guide to Dia
**Domain:** Public health / preventive health, type 2 diabetes prevention habits in Malaysia

## Problem Statement
A one-off diabetes risk score does not change habits, and habits are what prevention depends on. Malaysia has guidance for those habits, such as the Malaysian Healthy Plate (Suku-Suku Separuh), but about 80% of adults had never heard of it in NHMS 2019. Digital prevention programmes found in Malaysia are research pilots for people already high-risk or prediabetic and rely on human coaches. Guide to Dia is a habit tracker with a grounded AI coach for young adults who are not yet prediabetic: rules compute the risk score, goals and progress; a retrieval-grounded AI explains progress and answers questions with citations; data stays on the user's device.

## Evidence of the Problem
- **Evidence 1:** About 50% of people with diabetes are undiagnosed. — *MOH et al., CPG Management of T2DM, 6th ed. (Dec 2020), Quick Reference Guide.*
- **Evidence 2:** About 84% of adults aged 18–29 with diabetes were undiagnosed in NHMS 2023. — *The Star, 2 Aug 2025 (quoting the MOH). Check against the original report.*
- **Evidence 3:** In the US Diabetes Prevention Program, adults at high risk who reached ≥ 150 minutes of activity per week and 5–7% weight loss had 58% lower incidence of type 2 diabetes. The evidence is for high-risk adults, so this project does not claim the same effect for low-risk young adults. — *CDC National DPP: https://www.cdc.gov/diabetes-prevention/programs/what-is-the-national-dpp.html*
- **Evidence 4:** About 79.6% of Malaysian adults had never heard of the Malaysian Healthy Plate in NHMS 2019. — *https://pmc.ncbi.nlm.nih.gov/articles/PMC10351725*
- **Evidence 5:** Malaysian digital prevention studies (MyDiPP, PRIME, MY GODDESS) target high-risk, prediabetic or gestational-diabetes groups and use human coaches. — *clinicaltrials.gov NCT03997656; PRIME abstract (University of Sydney journals); Nutriweb MJN 29(1).*
- **Evidence 6 (observed 22 Nov 2026):** The DIAG ModAsian FINDRISC page is plain-language with per-level advice, but its next step is DIAG testing in Vietnam and it requests a name and phone number. PeKa Sihat covers STR recipients and spouses aged 40+. — *https://diag.vn/en/risk-prediction-for-10-year-type-2-diabetes/; MyGov PeKa B40.*
- **Hypotheses (not evidence):** H1 young adults do not know where to get tested; H2 many prefer Bahasa Malaysia; H3 users will keep up a two-minute weekly check-in (biggest risk; test first).

## Initial Related Work
1. **One-off risk tools:** ModAsian FINDRISC / DIAG (Lim 2020; Doan 2023), MyDiabeticRisk (Ab Zayin 2023), Malay ADA Diabetes Risk Test. Limitation: score and advice only; no tracking or coaching.
2. **MyDiPP, PRIME, MY GODDESS:** app plus human coaching for high-risk or prediabetic groups. Limitation: not for young adults who are not yet at that stage; research pilots.
3. **D'LITE** (Frontiers in Nutrition 2021): app with dietitian coaching for prediabetes. Limitation: clinical, not grounded in Malaysian guidance.
4. **CDC National DPP:** year-long coached programme for prediabetes, designed for the US.
5. **Malaysian Dietary Guidelines / PeKa Sihat:** authoritative but static.
6. **General-purpose AI chatbots:** may hallucinate, invent diet advice, quote outdated eligibility.

## Engineering Gap / Limitation
No reviewed approach combines a rule-based risk score, goal and progress engine; an AI coach grounded in cited Malaysian and international documents; on-device tracking without identity details; and a Malaysian route to testing, with health-safety guardrails, for young adults not yet prediabetic. Practical gap, not novelty. Real-world effect on behaviour is not tested.

## Initial Project Objectives
- **O1:** To design a prevention habit tracker with a hybrid AI coach for Malaysian young adults.
- **O2:** To develop it with retrieval, citation, on-device storage, a Malaysian pathway selector and strict health and wellbeing guardrails.
- **O3:** To evaluate engine correctness, groundedness, safety, supportive tone, readability, usability and latency (not clinical outcomes).

## Target Users
Young adults aged 18–35 in Malaysia (university students, early-career professionals) without a diabetes diagnosis.

## Data Sources
- **S01:** MOH et al., CPG Management of T2DM, 6th ed. (Dec 2020) — https://www.moh.gov.my (exact PDF URL to be recorded)
- **S02:** WHO Diabetes fact sheet — https://www.who.int/news-room/fact-sheets/detail/diabetes
- **S03:** MOH NCCFN, Malaysian Dietary Guidelines 2020 incl. Malaysian Healthy Plate — exact URL to be recorded
- **S04:** ProtectHealth, PeKa Sihat — https://protecthealth.com.my/peka-b40-eng/
- **S05:** MyGov, PeKa B40 — https://www.malaysia.gov.my/en/categories/aid-welfare-and-assistance/healthcare-aid/peka-b40
- **Calculator specification:** Lim et al. 2020 (ModAsian FINDRISC scoring table).

## Similar to EduAdvisor
Document-grounded Q&A over public documents; retrieval, citations, abstention and guardrails; evaluated on groundedness, safety and latency.

## Different from EduAdvisor
- A **tracker**: user data is stored over time (on the device), with a goal and progress engine in code.
- The AI **explains and coaches; it never computes** scores, progress or goals.
- Domain is **health**: stricter scope (no diagnosis, no diet or weight-loss plans), and wellbeing safeguards for eating-disorder signals.
- Handles **sensitive personal data**, so privacy is a design driver.

## Why this health project is acceptable under the Week 1 scoping rule
The system never diagnoses, never recommends treatment or calorie and weight-loss targets, answers only from retrieved official text, redirects red-flag symptoms to a doctor, and makes no claim that it prevents diabetes. Residual risks are tracked in `week2_requirements.md` (RAI1–RAI8).

## Responsible AI Risks
- **Hallucination:** invents a target, threshold, eligibility rule or food fact.
- **Disordered eating and over-exercise:** a tracker with food and weight data can reinforce harmful behaviour; mitigated by no calorie or weight targets and a wellbeing safeguard.
- **Privacy:** stored health habits and answers are sensitive.
- **Over-reliance and outcome claims:** users may believe the app prevents diabetes or replaces a clinic visit.
- **Demotivation and shame:** negative messages after missed weeks.
- **Equity and fairness:** assuming eligibility or ability to pay; Western food examples; sex-specific cut-offs; limited validation for ages 18–24.
