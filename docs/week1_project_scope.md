Week 1 Project Scope — Guide to Dia



\## Project Name and Domain

Project Name: Guide to Dia — AI Diabetes Risk \& Screening Recommender

Domain: Health Education / Public Health Screening Support



\## Problem Statement

Many young adults in Malaysia do not know they might be at risk of Type 2 Diabetes. Existing calculators use medical terms like second degree relatives and only give you a scary score without explaining what to do next. Because it is too confusing, people just give up. Guide to Dia solves this by using AI to translate the medical jargon into simple, empowering words and giving you one clear next step.



\## Evidence of the Problem

Evidence 1: The Malaysian Clinical Practice Guidelines (CPG) state that \~50% of people with diabetes are undiagnosed. These highlighting the need for better screening awareness among the public.

Source / citation: Malaysian CPG on Management of Type 2 Diabetes (6th Edition).



Evidence 2: Existing risk calculators like ModAsian FINDRISC use clinical terminology such as "2nd degree relative" or "waist circumference" which creates a difficulty for laypeople and young adults that do not understand.

Source / citation: ModAsian FINDRISC Online Calculator user interface; CDC Prediabetes Risk Test documentation.



\## Initial Related Work / Existing Approaches



Study / solution 1: MyDiabeticRisk (2023) — A web-based application developed to predict early-stage diabetes risk using four factors (Glucose, Blood Pressure, BMI, and Age) via a Logistic Regression model.

Source / citation: Ab Zayin et al. (2023), Zulfaqar Journal of Defence Science, Engineering \& Technology.

Key limitation: It is an academic web prototype that calculates a risk percentage. It does not offer conversational explanations and does not translate medical terms into simple language.



Study / solution 2: ModAsian FINDRISC — A validated clinical questionnaire used to calculate a 10-year Type 2 Diabetes risk score for Asian adults.

Source / citation: Primary care validation study (2020) — https://pubmed.ncbi.nlm.nih.gov/32147094/

Key limitation: It is a static questionnaire. It does not explain why specific risk factors matter nor does it guide the user on what to do next.



Study / solution 3: General AI Chatbots

Source / citation: chat.openai.com

Key limitation: They are dangerous for medical queries because they hallucinate. They cannot be trusted to provide accurate clinical screening guidelines and lack strict safety guardrails.



\## Engineering Gap / Limitation

Existing calculators provide a numerical score using medical jargon without conversational context while general AI models are too risky for health topics. There is a gap for a hybrid system which is a rule-based risk calculator combined with a RAG-based AI assistant that acts as a medical translator that explaining the risk in simple terms and citing official clinical guidelines like CPG, CDC and WHO to recommend screening with strict safety guardrails.



\## Initial Project Objectives

O1: To design a hybrid AI assistant that calculates diabetes risk and explains it in simple term and non-medical language using official health documents.

O2: To develop the solution with retrieval, source citation and strict medical safety guardrails.

O3: To evaluate the solution based on explanation groundedness, safety and latency.



\## Target Users

Young adults aged 18–35 in Malaysia.



\## Data Sources

Source 1: CDC Prediabetes Risk Test Guidelines — https://www.cdc.gov/prediabetes/

Source 2: Malaysian Clinical Practice Guidelines (CPG) on Type 2 Diabetes (6th Edition) — http://www.moh.gov.my

Source 3: WHO Diabetes Fact Sheet \& Screening Guidelines — https://www.who.int/news-room/fact-sheets/detail/diabetes

Source 4: ModAsian FINDRISC Risk Calculator Documentation — https://diag.vn/en/risk-prediction-for-10-year-type-2-diabetes/



\## Comparison with EduAdvisor

Similar to EduAdvisor: Both take structured user input and recommend a "next step." Both use document-grounded AI to explain recommendations. Both require guardrails to prevent hallucination and out-of-scope advice.

Different from EduAdvisor: EduAdvisor provides academic advice (low risk if wrong). Guide to Dia deals with health data (high risk if wrong). Therefore, Guide to Dia requires much stricter Responsible AI guardrails: it must never diagnose a disease, never prescribe medicine, and always redirect to a real doctor. Furthermore, Guide to Dia applies behavioral science (Fogg Model) to reduce friction, focusing on empowering insights and actionable habits rather than just giving a dense academic answer.



\## Responsible AI Risks

Risk 1 — Medical Diagnosis (High Risk): The user might treat the AI's recommendation as a medical diagnosis.

Mitigation idea: Implement a hardcoded guardrail: The AI must always state "I am an AI, not a doctor. Please consult a clinic for a real diagnosis." It must never say "You have diabetes."



Risk 2 — Hallucination of Medical Advice:\*\* The AI might hallucinate a fake cure or wrong test.

Mitigation idea: RAG strictly grounds the AI in the CPG, CDC, and WHO documents. Every response must include an inline citation. If the answer is not in the documents, it says "Please consult a doctor."



Risk 3 — Privacy: Users inputting sensitive health data (Age, BMI, Blood Pressure).

Mitigation idea: The system must not store or transmit personal health data. The risk calculation will be done in-memory.

