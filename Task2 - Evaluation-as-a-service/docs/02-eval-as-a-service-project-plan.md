# Llumino Evaluation-as-a-Service — Project Plan
### Task 2: Gap Analysis (Standard Set vs Golden Sets) and EaaS Rollout

**Dome – Llumino AI Platform Team – October 12, 2026**

> Structure follows the PoC Project Plan template in `pic/`. Capability definitions and tool scores come from [01-eval-as-a-service-capabilities.md](01-eval-as-a-service-capabilities.md). The tool choice comes from [Task 1](../../Task1%20-%20Comparison/docs/01-online-eval-framework-comparison.md). Where a person's name is not yet known, the role is given instead. Costs are planning estimates, and the assumptions behind them are listed in section 5.

## Table of Contents

1. Project Overview
   1.1 Executive Summary
   1.2 Project Sponsor(s) / Stakeholder(s) / Project Team
   1.3 Project Success Criteria
   1.4 Assumptions
2. Scope of Work — Project Plan
3. Solution Architecture
4. Summary of Milestones & Deliverables
   4.1 Expected Cost Breakdown by Service
5. Resources & Cost Estimates
6. Acceptance

---

## 1 Project Overview

### 1.1 Executive Summary

Llumino will run evaluation as a governed, recurring service, not as a test each team runs once before launch. Every AI application gets a versioned evaluation suite that supports release gates, production monitoring and model selection through ModelAdvisor.

Task 2 answers two questions:

1. **Standard set vs golden sets.** Which evaluations should be a shared, enterprise-wide **standard set** (safety, policy, PII, prompt injection, trajectory hygiene)? Which should be **application-specific golden sets** owned by each business line?
2. **Gap analysis.** Of the 12 capabilities in D6 — Evaluation & Quality, which does the tool chosen in Task 1 (Braintrust or AgentCore) cover, and which does Llumino have to build?

High-level deliverables:
- A gap matrix scoring all 12 capabilities for the chosen tool, with a build/buy/configure decision for each.
- A standard-set catalogue and a golden-set template (registration package).
- Onboarding of two pilot applications end to end through all six stages (Onboard → Improve).
- A ModelAdvisor evidence feed and a regression-gate pattern for CI.

### 1.2 Project Sponsor(s) / Stakeholder(s) / Project Team

**Executive Sponsor**

| Name | Title | Description | Contact |
|---|---|---|---|
| Head of AI Platform | Executive Sponsor | Owner of capability D6 — Evaluation & Quality; final sign-off | Dome internal directory |

**Project Stakeholders**

| Name | Title | Stakeholder for | Contact |
|---|---|---|---|
| Lina | Lead, Braintrust evaluation team | Braintrust demo and adoption | Dome internal directory |
| Enterprise Architecture Lead | Architect | AgentCore Evaluations exploration | Dome internal directory |
| AI Risk & Compliance Lead | Risk / Compliance | Standard set, red teaming, judge governance | Dome internal directory |
| Pilot Application Owners (×2) | LOB Sponsors | Golden sets, human review, acceptance of pilot results | Dome internal directory |

**Project Team**

| Name | Title | Role | Contact |
|---|---|---|---|
| Birender | AI Engineer | Gap analysis owner (Task 2) | Dome internal directory |
| Llumino Engagement Manager | Project Manager | Plan, status, risks, stakeholder updates | Dome internal directory |

**Project Escalation Contacts**

| Name | Title | Role | Contact |
|---|---|---|---|
| Head of AI Platform | Executive Sponsor | Scope, priority and funding decisions | Dome internal directory |
| AI Risk & Compliance Lead | Risk / Compliance | Policy, data-classification and red-team issues | Dome internal directory |

### 1.3 Project Success Criteria

| Category | Metric | Description / Target |
|---|---|---|
| Coverage | Capability gap matrix | All 12 D6 capabilities scored 0–3 for the chosen tool, each with a build / buy / configure decision and an owner |
| Coverage | Standard set | ≥ 90% of standard-set checks (safety, PII, prompt injection, policy, trajectory) run automatically on both pilot apps |
| Golden Data | Golden-set quality | Each pilot app has ≥ 100 versioned cases, including edge cases, policy failures and known incidents |
| Release Gates | Regression gate | A CI gate blocks at least one seeded regression per pilot app and returns the failing cases and the rule that triggered |
| Judge Governance | Judge calibration | LLM-judge agreement with human reviewers ≥ 80% on a 50-case calibration set; judge prompt and model version are pinned |
| Production | Online scoring | Sampled production scoring is live for both pilots, with high-risk and failed sessions prioritised and PII redacted |
| ModelAdvisor | Evidence feed | At least one reproducible model comparison (quality, safety, latency, cost) per pilot, recorded with versions and the decision owner |
| Adoption | Builder acceptance | ≥ 80% of pilot builders say the failure evidence is actionable (addresses the low adoption seen in Task 1) |
| Improve loop | Feedback to cases | ≥ 1 production finding or human annotation per pilot turned into a permanent evaluation case |

### 1.4 Assumptions

- Task 1 settles the main tool (Braintrust or AgentCore) by the end of Week 2 (October 23, 2026). Until then the gap matrix is scored for both.
- Pilot app teams provide sandbox access, trace data and an owner who can approve golden cases.
- Risk / Compliance provides the policy list for the standard set and signs off on red-team scenarios.
- LLM API access for judge models is provisioned for the team in Week 1.
- Production trace data is redacted according to its data classification before it is scored.
- Salesforce Integration requirements are confirmed by the Salesforce platform owner by Week 4. This axis is on the comparison chart but is not described on any slide.

---

## 2 Scope of Work — Project Plan

The team will build the Llumino evaluation service layer in a non-production environment and onboard two pilot applications.

**In scope**

- **Gap analysis:** score the 12 capabilities for the chosen tool and decide what Llumino builds. The starting point, taken from the spider map, is below.

| Capability | Best tool score | Llumino action |
|---|---|---|
| Evaluation Suites | 3 (all) | Configure in tool; Llumino owns the registry |
| Regression Gates in CI | 3 (Braintrust, Langfuse) | Configure; add risk-tiered thresholds |
| Trajectory Evaluation | 3 (AgentCore, Arize) | Configure if AgentCore; partial build if Braintrust |
| Judge Governance Suite | 3 (Arize only) | **Build** calibration and drift control |
| App Eval Onboarding & Lifecycle | 2 (all) | **Build**: registration package, lineage, freshness triggers |
| Model Benchmarking / ModelAdvisor | 2 (all) | **Build**: controlled comparison profile and scorecard |
| Online / Production Scoring | 3 (all) | Configure sampling and privacy policy |
| Simulation | 3 (AgentCore only) | Configure if AgentCore; build if Braintrust |
| Human Annotation Workflow | 3 (Braintrust, Langfuse, Arize) | Configure if Braintrust; build if AgentCore |
| Red Teaming | 2 (all) | **Build**: recurring adversarial suite and closure tracking |
| Component, Tool & MCP Eval | 3 (Arize only) | **Build** contract tests |
| Salesforce Integration | 1 (all) | **Build** connector (requirements due Week 4) |

- **Standard set vs golden sets:** a split rule. Checks that apply to every app go in the **standard set**, owned by Llumino and versioned centrally. Checks about business outcomes go in **golden sets**, owned by each app and registered with Llumino.
- **Pilot onboarding:** two applications taken through Onboard → Build → Qualify → Select → Monitor → Improve.

**Out of scope:** production rollout to all applications, vendor contract negotiation, and the Task 3 agent-identity work.

High-level deliverables:
- Gap matrix and build backlog
- Standard-set catalogue v1 and golden-set registration template
- CI regression-gate pattern and ModelAdvisor evidence schema
- Pilot results report and a rollout recommendation

---

## 3 Solution Architecture

The design pairs a central control plane with federated execution, as set out in Task 1.

```
┌───────────────────────── Llumino Control Plane (central) ─────────────────────────┐
│  Suite Registry      Standard Set      Judge Registry      Threshold / Risk Tiers  │
│  (golden sets,       (shared checks)   (versioned judges,  (gate rules per tier)   │
│   lineage)                              calibration)                               │
│                 ModelAdvisor Evidence Store  ◄── results from all runs             │
└───────────────┬───────────────────────────────┬───────────────────────────────────┘
                │ suites, judges, thresholds    │ scores, traces, verdicts
┌───────────────▼───────────────┐   ┌───────────▼──────────────────────────────────┐
│ Offline / CI execution        │   │ Online / production execution                │
│ (Braintrust or AgentCore)     │   │ (sampled traffic, PII redaction)             │
│ • suites • regression gate    │   │ • online scorers • drift / freshness triggers│
│ • trajectory • simulation     │   │ • human review queue → new golden cases      │
└───────────────────────────────┘   └──────────────────────────────────────────────┘
```

The formal architecture diagram will be produced in Week 2, once the Task 1 tool decision is final.

---

## 4 Summary of Milestones & Deliverables

Estimated project start date: **October 12, 2026**. Estimated project end date: **December 18, 2026** (10 weeks).

| # | Milestone or Deliverable Title | Description | Completion |
|---|---|---|---|
| 1 | Gap Matrix; Standard-vs-Golden Split Rule | Score all 12 capabilities for the chosen tool. Confirm build / buy / configure for each. Agree which checks are standard and which are golden. | Weeks 1–2 (Oct 23) |
| 2 | Standard Set v1; Golden-Set Registration Template | Catalogue of shared checks (safety, PII, prompt injection, policy, trajectory). Registration package template (owner, outcomes, risk tier, datasets, scorers, thresholds, cadence). | Weeks 3–4 (Nov 6) |
| 3 | Pilot Onboarding & Golden Sets | Onboard two pilot apps. Build ≥ 100 golden cases each. Version datasets, scorers and prompts. | Week 5 (Nov 13) |
| 4 | Regression Gates & Judge Governance | CI gate with risk-tiered thresholds and failure evidence. Judge calibration against human labels, with pinned versions. | Weeks 6–7 (Nov 27) |
| 5 | ModelAdvisor Feed & Online Scoring | Controlled model comparison per pilot. Sampled production scoring with redaction. Freshness and drift triggers. | Week 8 (Dec 4) |
| 6 | Red Teaming & Improve Loop | First recurring adversarial run with closure tracking. Production findings and annotations turned into permanent cases. | Week 9 (Dec 11) |
| 7 | Pilot Report & Rollout Recommendation | Results against the success criteria, the build backlog for the remaining gaps, and the rollout plan. | Week 10 (Dec 18) |

### 4.1 Expected Cost Breakdown by Service

| Service | Purpose | Estimated Cost (10 weeks) |
|---|---|---|
| Evaluation tool | Suites, gates, online scoring | Braintrust: assumed covered by the existing demo during the pilot (Pro is $249/month; Enterprise, needed for hybrid hosting, is custom-priced). AgentCore: pay as you go, not free (see section 5.2 notes) |
| LLM judge API calls | Automated scoring (~125,000 judgments) | ~$1,500 |
| Storage / database | Suite registry, evidence store | ~$200 |
| Compute (CI runners, simulation, red-team runs) | Offline runs | ~$800 |
| **Total infrastructure** | | **~$2,500** |

---

## 5 Resources & Cost Estimates

### 5.1 Resources

| Resource | Allocation |
|---|---|
| Evaluation Lead / Gap Analysis Owner | 100% |
| GenAI Engineer (suites, judges, gates) | 100% |
| Platform / MLOps Engineer (CI, online scoring) | 100% |
| Risk / Compliance Analyst (standard set, red teaming) | 50% |
| Pilot App SMEs (golden sets, human review) | 25% each (×2) |
| Project / Engagement Manager | 50% |
| **Total** | **4.5 FTE = 45 person-weeks** |

### 5.2 Total Fees for Services and Expenses

| Item | Amount |
|---|---|
| Internal effort (45 person-weeks × $2,500 blended loaded rate) | $112,500 |
| Tooling and API spend (section 4.1) | $2,500 |
| Contingency (10%) | $11,500 |
| **Total estimated cost** | **$126,500** |

**How these estimates were worked out**

- **LLM judge calls.** Offline: 2 pilots × about 100 cases × about 5 judges × about 40 runs ≈ 40,000 judgments. Online: 2 pilots × 10% sample of about 2,000 sessions a day × 3 judges × 70 days ≈ 84,000 judgments. At about $0.012 per judgment, the total is about $1,500. The per-judgment rate should be checked against the judge model the team actually uses.
- **Internal effort.** The $2,500 per person-week blended rate is a placeholder. Replace it with Dome's internal rate card.
- **Tooling.** A vendor quote for production rollout is not included. Braintrust Enterprise is custom-priced. AgentCore Evaluations is billed per token for built-in evaluators ($0.0024 per 1K input tokens, $0.012 per 1K output tokens) and $1.50 per 1,000 evaluations for custom evaluators, plus model costs (AWS pricing page, checked 2026-10-05).

---

## 6 Acceptance

The project is accepted when the Head of AI Platform and the AI Risk & Compliance Lead confirm that:

1. The gap matrix is complete and every gap has an owner and a decision.
2. Both pilot apps meet the success criteria in section 1.3.
3. The rollout recommendation and build backlog have been reviewed.

| Name | Role | Signature | Date |
|---|---|---|---|
| Head of AI Platform | Executive Sponsor | | December 18, 2026 |
| AI Risk & Compliance Lead | Risk / Compliance | | December 18, 2026 |
