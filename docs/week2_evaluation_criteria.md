\# Evaluation Criteria for Guide to Dia



\## Evaluation Table



| Objective and Requirement ID | Evaluation Criterion | Capability Bucket | Measurement Method | Metric | Threshold | User and Business Outcome |

|---|---|---|---|---|---|---|

| O1 and FR1 | Risk Calculation Accuracy | Domain specific capability | Test 20 predefined user profiles against the official CDC scoring logic. | Correct calculation rate | 100 percent | Users get the exact same score as standard clinical tools. |

| O2 and RAI3 | Source Citation and Grounding | Instruction following capability | Inspect AI responses to ensure every medical claim has a citation from the CPG or WHO documents. | Citation presence rate | 100 percent | Users can trust the advice because they can verify it. |

| O2 and RAI1 | Medical Safety and Refusal | Instruction following capability | Test the system with 20 queries asking for a direct diagnosis or fake cures. | Refusal and redirect success rate | 100 percent | Prevents medical harm and protects the app from legal trouble. |

| O3 and NFR1 | Response Latency | Cost and latency | Measure the time taken from user input to final AI response. | p90 latency | Less than 3000 ms | Users do not get bored and leave the app while waiting. |

| O1 and FR3 | Jargon Translation | Generation capability | Evaluate if the AI can explain terms like second degree relative in simple words. | Clarity rubric score | Meets the plain English criterion | Young adults can understand the risk without needing a medical dictionary. |



\## Reflection Part B



\*\*Question\*\*

Which evaluation criterion is most important for determining whether your AI application will be useful to its intended users? Why?



\*\*Answer\*\*

For me the most important criterion is Medical Safety and No Diagnosis which is RAI1. If this app accidentally tells a young person that they are safe when they actually have high risk factors, or if it suggests a fake cure, it could cause real harm. People will only use a health app if they trust it completely. If the app acts like a real doctor and makes a mistake, users will delete it immediately. So making sure the AI knows its limits and always redirects to a real clinic is the most important thing for this app to be useful.

