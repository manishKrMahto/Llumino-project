# Evaluation-as-a-Service
### Llumino, Task 2 Reference (text extracted from `for-comaprision-slide-pics/`)

> A governed, repeatable service for proving that AI applications are effective, safe and fit for purpose.

Source slides: `a.png` to `i.png` and `spider-map.png`. The text is transcribed word for word. The spider-map scores are read off the chart by eye, so treat them as approximate.

---

## What Is Evaluation-as-a-Service? (`a.png`)

Evaluation-as-a-Service provides a common enterprise approach for testing and monitoring AI models, applications and agents. It helps teams determine whether an AI solution is accurate, safe, reliable, cost-effective and appropriate for its intended business purpose.

Evaluation is treated as a recurring operational control, not as a one-time test performed before launch. Each application has a governed, versioned evaluation suite that supports model selection, release decisions, production monitoring and continuous improvement.

---

## Capability Overview (`b.png`)

| Field | Value |
|---|---|
| Capability | D6 — Evaluation & Quality |
| Capability Priority | **Must Have** |
| Control Question | Is the AI solution good enough, safe enough and still fit for purpose? |
| Capability Description | Governs application evaluation onboarding, offline and online evaluation, comparative model benchmarking, deployment gates, trajectory quality, automated judge models, human review and component-level testing. |
| Operating Principle | Evaluation must be application-specific, versioned and recurring. It should not be treated as a launch-time report or as a universal model leaderboard. |

---

## Primary Beneficiaries (`c.png`)

| Persona | How the Persona Benefits |
|---|---|
| Agent Builder / Developer | Receives repeatable tests, detailed failure evidence and clear release criteria. |
| Platform Admin / AI Operations | Gains standardized evaluation operations, production scoring and evidence of ongoing service quality. |
| Risk Analyst / Compliance / Security | Gains evidence that safety, policy, regulatory and control requirements have been tested. |
| Business Owner / LOB Sponsor | Gains evidence that the solution supports its intended outcome and remains fit for business use. Contributes feedback and human judgment to improve evaluation data (golden data) and consequently their application quality. |

---

## Core Evaluation Functions — Must Have (`d.png`, `e.png`, `f.png`)

| # | Function | Description | Personas Benefiting Most | Key Attributes |
|---|---|---|---|---|
| 1 | **Evaluation Suites (Application-specific Golden Datasets)** | Runs versioned datasets and scoring methods against candidate changes before they are promoted. | Agent Builder / Developer; Platform Admin / AI Operations; Risk Analyst / Compliance / Security; Business Owner / LOB Sponsor | **Coverage:** Includes expected tasks, edge cases, policy failures and known incidents. **Versioning:** Retains the dataset, scorer, prompt, model and agent configuration versions. |
| 2 | **Regression Gates in CI** | Blocks promotion when quality, safety, cost or policy regression exceeds an approved threshold. | Agent Builder / Developer; Platform Admin / AI Operations; Risk Analyst / Compliance / Security | **Gate Criteria:** Uses risk-tiered thresholds and compares the candidate against an approved baseline. **Failure Evidence:** Gives builders the failing cases and the rule that blocked promotion. |
| 3 | **Trajectory Evaluation** | Evaluates tools selection by agents, step efficiency, unnecessary calls, escalation and policy-compliant execution paths. | Agent Builder / Developer; Platform Admin / AI Operations; Risk Analyst / Compliance / Security | **Path Criteria:** Evaluates approved tools, step limits, delegation depth and required approvals. **Outcome Separation:** Prevents a correct-looking answer from hiding an unsafe or inefficient execution path. |
| 4 | **Judge Governance Suite** | Versions, calibrates, monitors and approves LLM-as-judge and other automated evaluators as governed models. | Risk Analyst / Compliance / Security; Agent Builder / Developer; Platform Admin / AI Operations | **Calibration:** Compares automated judge results with qualified human decisions and known cases. **Drift Control:** Versions and monitors the judge model, prompt and decision thresholds. |
| 5 | **Application Evaluation Onboarding & Suite Lifecycle** | Registers each application evaluation suite as a governed asset and manages its ownership, versions, datasets, scorers, judges, thresholds, run history, freshness, coverage and monitoring obligations. | Agent Builder / Developer; Platform Admin / AI Operations; Risk Analyst / Compliance / Security; Business Owner / LOB Sponsor | **Registration Package:** Records the application, owner, outcomes, risk tier, datasets, scoring methods, thresholds, environments and run cadence. **Version & Lineage:** Links each application release to the exact suite version that qualified it. **Freshness & Drift Triggers:** Starts a review when performance, requirements, incidents or system components change. |
| 6 | **Comparative Model Benchmarking & Recommendation — ModelAdvisor** | Runs the same application evaluation suite against multiple approved model configurations and produces an evidence-based comparison of quality, safety, reliability, latency and cost. | Agent Builder / Developer; Risk Analyst / Compliance / Security; Platform Admin / AI Operations; Business Owner / LOB Sponsor | **Controlled Comparison Profile:** Uses the same test cases, prompt, tools, retrieval state, parameters and scoring rules wherever possible. **Multi-Objective Scorecard:** Compares quality, safety, policy adherence, latency, cost, consistency and operational constraints. **Reproducible Recommendation:** Retains the candidate versions, results, trade-offs, selected model, decision owner and decision date. |
| 7 | **Online / Production Scoring** | Scores sampled or full production traffic for quality, safety, policy adherence and outcome correctness. | Platform Admin / AI Operations; Agent Builder / Developer; Risk Analyst / Compliance / Security; Business Owner / LOB Sponsor | **Sampling Policy:** Prioritizes high-risk, failed and anomalous sessions for evaluation. **Privacy Control:** Minimizes and redacts production content and retains it according to its data classification. |
| 8 | **Simulation** | Replays multi-turn, adversarial and operational scenarios against a candidate version in a controlled environment. | Agent Builder / Developer; Platform Admin / AI Operations; Risk Analyst / Compliance / Security; Business Owner / LOB Sponsor | **Scenario Fidelity:** Uses realistic tools, retrieval, permissions and failure conditions. **Repeatability:** Keeps inputs, environment and expected outcomes reproducible. |

---

## Supporting Evaluation Functions — Should Have (`g.png`)

| # | Function | Description | Personas Benefiting Most | Key Attributes |
|---|---|---|---|---|
| 9 | **Human Annotation Workflow** | Manages review queues, annotation guidelines, reviewer agreement and the conversion of human feedback into evaluation data. | Risk Analyst / Compliance / Security; Agent Builder / Developer; Platform Admin / AI Operations; Business Owner / LOB Sponsor; End User | **Reviewer Quality:** Measures reviewer calibration, consistency and subject-matter expertise. **Dataset Path:** Moves approved annotations into versioned test data with traceable provenance. |
| 10 | **Red Teaming** | Runs recurring adversarial testing for misuse, prompt injection, excessive autonomy, data exposure and business abuse scenarios. | Risk Analyst / Compliance / Security; Agent Builder / Developer; Platform Admin / AI Operations; Business Owner / LOB Sponsor | **Threat Basis:** Connects testing to current AI threats, business abuse cases and previous incidents. **Closure Evidence:** Tracks the owner, remediation, retest results and remaining risk for each finding. |
| 11 | **Component, Tool & MCP Evaluation** | Tests models, prompts, retrieval pipelines, tools, skills and MCP servers independently before they are included in an agent or application. | Agent Builder / Developer; Risk Analyst / Compliance / Security; Platform Admin / AI Operations | **Contract Tests:** Validates inputs, outputs, permissions, error handling and side effects. **Compatibility Matrix:** Records supported agent frameworks, protocol versions and operating environments. |

> The `g.png` screenshot is cut off after row 11. The spider map also scores a 12th axis, **Salesforce Integration**, which is not described on any captured slide.

---

## How the Service Operates (`h.png`)

| Stage | Evaluation-as-a-Service Activity |
|---|---|
| 1. Onboard | Register the application, intended business outcomes, risks, evaluation owner, datasets, scoring methods and thresholds. |
| 2. Build | Create versioned offline tests, trajectory tests, simulations and component-level evaluations. |
| 3. Qualify | Compare results with approved thresholds and prevent release when unacceptable regression is detected. |
| 4. Select | Use ModelAdvisor to compare approved models and recommend the best configuration for the specific application. |
| 5. Monitor | Evaluate production traffic and trigger review when quality, safety, behavior or business requirements change. |
| 6. Improve | Convert production findings, human feedback, incidents and red-team results into permanent evaluation cases. |

---

## Executive Takeaway (`i.png`)

Evaluation-as-a-Service creates a consistent evidence base for AI investment, model selection, release approval and production assurance. It allows the enterprise to scale AI adoption without leaving each application team to define its own quality and safety standards.

---

## Evaluation Framework Capability Comparison — Small Multiples (`spider-map.png`)

Scale: 0 to 3, read by eye from the radar charts (1 = weak, 2 = partial, 3 = strong).

| Function | Braintrust | AWS AgentCore Evaluations | Langfuse | Arize AX |
|---|---|---|---|---|
| Evaluation Suites | 3 | 3 | 3 | 3 |
| Regression Gates in CI | 3 | 2 | 3 | 2 |
| Trajectory Evaluation | 2 | 3 | 1 | 3 |
| Judge Governance Suite | 2 | 2 | 2 | 3 |
| App Eval Onboarding & Lifecycle | 2 | 2 | 2 | 2 |
| Model Benchmarking / ModelAdvisor | 2 | 2 | 2 | 2 |
| Online / Production Scoring | 3 | 3 | 3 | 3 |
| Simulation | 2 | 3 | 1 | 1 |
| Human Annotation Workflow | 3 | 1 | 3 | 3 |
| Red Teaming | 2 | 2 | 1 | 1 |
| Component, Tool & MCP Eval | 2 | 2 | 2 | 3 |
| Salesforce Integration | 1 | 1 | 1 | 1 |
| **Total (out of 36)** | **27** | **26** | **24** | **27** |

**What the chart shows**

- No tool scores 3 on **App Eval Onboarding & Lifecycle**, **ModelAdvisor**, **Red Teaming** or **Salesforce Integration**. Llumino has to provide these itself, whichever tool is chosen.
- Braintrust and AgentCore cover different things. Braintrust is stronger on CI gates and human annotation. AgentCore is stronger on trajectory evaluation and simulation, but weak on human annotation.
- Arize AX is the only tool rated 3 on Judge Governance and on Component/MCP Eval.
