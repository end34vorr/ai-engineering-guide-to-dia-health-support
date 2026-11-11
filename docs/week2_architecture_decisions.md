# V1 Architecture Decisions and Traceability for Guide to Dia

## 1. Architecture Needs

Based on our requirements the simple architecture is not enough. We need specific components.

| Requirement ID | What does the requirement need | Architecture Response |
|---|---|---|
| FR1 | Calculate a risk score using user data like Age and BMI. | Rule Based Calculator component to do the math accurately. |
| FR2 and RAI3 | Explain the risk using official medical guidelines. | Knowledge Source containing CDC, CPG and WHO documents. Plus a retrieval component. |
| RAI1 | Refuse to diagnose and prevent medical harm. | Scope and Safety Control component to filter out dangerous medical queries. |
| FR3 | Translate medical jargon into simple words. | Language Model Service to process text and rewrite it in plain English. |

## 2. Architecture Decision Records

**Decision 1 Include Dynamic Knowledge Access or RAG**
Requirement or Constraint. Answers must be grounded in official clinical guidelines and must not hallucinate medical facts.
Alternatives Considered. Base model only or static web search.
Selected Option. Dynamic knowledge access using RAG.
Trade off Accepted. More system complexity and slightly higher latency but much better safety and traceability.

**Decision 2 Include Explicit Scope and Safety Control**
Requirement or Constraint. The system must never diagnose diabetes or give medical treatment advice.
Alternatives Considered. Relying only on the system prompt instructions.
Selected Option. Explicit scope control component as a hardcoded guardrail.
Trade off Accepted. Possible false refusals if the user asks a valid but weird question but much higher safety and legal protection.

## 3. Initial Requirements Traceability Matrix

| Objective | Requirement | Evaluation Criterion | Architecture Element | Later Evidence |
|---|---|---|---|---|
| O1 | FR1 | Risk Calculation Accuracy | Rule Based Calculator | Test results from 20 user profiles |
| O2 | RAI3 | Source Citation and Grounding | Knowledge Source and Language Model Service | Citation match rate from evaluation set |
| O2 | RAI1 | Medical Safety and Refusal | Scope and Safety Control | Refusal rate on 20 diagnosis queries |
| O3 | NFR1 | Response Latency | Application Interface and Language Model Service | p90 latency logs |

## Reflection Part C

**Question**
Which architecture decision involved the most important trade-off? Which requirement drove the decision, and what trade-off did you accept?

**Answer**
Adding the Dynamic Knowledge Access which is the RAG component for Decision 1 was the biggest trade-off. The requirement driving this was RAI3 for Transparency and Grounding and also FR2 for explaining the risk using official guidelines. If we just used a normal ChatGPT model without RAG it would be much faster and easier to build but it would definitely hallucinate fake medical rules. So we accepted the trade-off of a slightly slower response time and a more complicated system. I think it is worth it because for a health app getting the medical facts right is way more important than saving one second of loading time.