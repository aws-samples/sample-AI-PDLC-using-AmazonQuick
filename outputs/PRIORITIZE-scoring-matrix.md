# PRIORITIZE — Scoring Matrix
## AnyCompany Clinical Consultation Workflow

**Date:** 2026-07-08  
**Participant:** Jordan Lee (JL), Senior Solutions Architect, AWS  
**Source:** AI-PDLC Discovery Agent (Pain Point Board)  
**Space:** AnyCompanyClinicalConsultation-v1

---

## Use Cases Evaluated

| # | Use Case | Type | Pain Points Addressed | Description |
|---|----------|------|----------------------|-------------|
| UC1 | Unified Clinical Workflow | Application | #1, #5, #6, #7 | Single-pane experience consolidating consultation, guidelines, and order placement — eliminating system fragmentation |
| UC2 | Real-time AI-Guided Clinical Decision Support | Agentic | #2, #3 | AnyCompany Insight AI delivers explainable, real-time guideline recommendations at point of care with transparent reasoning |
| UC3 | Event-Driven Integration Modernization | Application | #4 | Replace monolithic integration layer with event-driven microservices architecture for resilience and extensibility |

---

## Scoring Framework 1: APPLICATION Use Cases (UC1, UC3)

| Criterion | Weight | What to Assess |
|-----------|--------|----------------|
| Business Value | 25% | Revenue impact, cost savings, user satisfaction |
| Technical Feasibility | 20% | Technology maturity, team expertise, infrastructure readiness |
| User Impact | 20% | Number of users affected, frequency of use, workflow criticality |
| Development Effort | 15% | Complexity, team size needed, timeline (inverse — lower effort = higher score) |
| Integration Complexity | 10% | Number of systems, API availability, data migration (inverse) |
| Strategic Alignment | 10% | Company priorities, market needs, competitive positioning |

---

## Scoring Framework 2: AGENTIC Use Cases (UC2)

| Criterion | Weight | What to Assess |
|-----------|--------|----------------|
| Business Value | 25% | Revenue impact, cost savings, user satisfaction improvement |
| LLM Capability Match | 20% | Can current LLMs handle this well? Reasoning complexity? Output quality? |
| Tool Availability | 15% | Are required tools/APIs available? Ease of integration? Data accessible? |
| User Acceptance | 15% | Will users trust an agent for this? Change management complexity? |
| Time to Market | 15% | Development effort, dependencies, resource availability |
| Strategic Alignment | 10% | Company priorities, market differentiation, competitive advantage |

---

## Scored Matrix

### UC1: Unified Clinical Workflow (Application)

| Criterion | Weight | Score (0-10) | Weighted | Rationale |
|-----------|--------|:---:|:---:|-----------|
| Business Value | 25% | 8 | 2.00 | Addresses 4 pain points (most coverage), directly reduces churn risk ($2-5M at stake), impacts 2,000-5,000 daily consultations |
| Technical Feasibility | 20% | 7 | 1.40 | UI consolidation is well-understood; existing AnyCompanyEMR + AnyCompany Insight AI systems provide data sources; requires frontend re-architecture but no novel tech |
| User Impact | 20% | 9 | 1.80 | Every physician (200-500) uses the workflow daily; eliminates system-switching that causes cognitive burden and errors |
| Development Effort | 15% | 6 | 0.90 | Moderate-high effort: consolidating 4 systems into single pane requires significant frontend + integration work |
| Integration Complexity | 10% | 5 | 0.50 | Must integrate with AnyCompanyEMR, AnyCompany Insight AI, and order systems simultaneously; multi-system data flow |
| Strategic Alignment | 10% | 8 | 0.80 | Core physician experience improvement; competitive necessity (competitors addressing this); US+EU market retention |
| **TOTAL** | | | **7.40** | |

### UC2: Real-time AI-Guided Clinical Decision Support (Agentic)

| Criterion | Weight | Score (0-10) | Weighted | Rationale |
|-----------|--------|:---:|:---:|-----------|
| Business Value | 25% | 9 | 2.25 | Addresses the two highest-severity pain points (9/10 each); directly prevents missed diagnoses and delayed care; GDPR/EU AI Act compliance |
| LLM Capability Match | 20% | 8 | 1.60 | Current LLMs excel at clinical guideline retrieval and explanation generation; AnyCompany Insight AI already exists as foundation; explainability achievable with chain-of-thought |
| Tool Availability | 15% | 7 | 1.05 | AnyCompany Insight AI platform exists; clinical guideline databases available; FHIR APIs for EMR data; some custom tooling needed for real-time inference pipeline |
| User Acceptance | 15% | 7 | 1.05 | Physicians want AI assistance but demand explainability (per feedback: "physicians have final control"); trust barrier is moderate |
| Time to Market | 15% | 6 | 0.90 | Requires real-time inference pipeline, explainability layer, regulatory validation (EU AI Act conformity); 6-12 month timeline |
| Strategic Alignment | 10% | 9 | 0.90 | Highest differentiation opportunity; EU AI Act compliance is market requirement not yet met by competitors |
| **TOTAL** | | | **7.75** | |

### UC3: Event-Driven Integration Modernization (Application)

| Criterion | Weight | Score (0-10) | Weighted | Rationale |
|-----------|--------|:---:|:---:|-----------|
| Business Value | 25% | 6 | 1.50 | Foundational enabler but indirect value; reduces downtime (weekly issues) but alone doesn't solve clinical workflow pain directly |
| Technical Feasibility | 20% | 7 | 1.40 | Event-driven architecture is mature (AWS EventBridge, SNS/SQS, Step Functions); AnyCompany already on AWS; well-documented migration patterns |
| User Impact | 20% | 4 | 0.80 | Physicians don't directly interact with integration layer; impact is through reduced downtime and enabling UC1/UC2 (indirect) |
| Development Effort | 15% | 4 | 0.60 | Major infrastructure overhaul: decomposing monolith, migrating data flows, ensuring backward compatibility during transition |
| Integration Complexity | 10% | 3 | 0.30 | Highest complexity: replacing the central integration layer touches every system; requires careful migration with zero downtime |
| Strategic Alignment | 10% | 7 | 0.70 | Enables future extensibility; AWS-native architecture; but table-stakes rather than differentiator |
| **TOTAL** | | | **5.30** | |

---

## Combined Ranking

| Rank | Use Case | Type | Weighted Score | Recommendation |
|------|----------|------|:---:|----------------|
| 🥇 1 | Real-time AI-Guided CDS | Agentic | **7.75** | Highest business value + strategic differentiation + regulatory compliance driver |
| 🥈 2 | Unified Clinical Workflow | Application | **7.40** | Broadest user impact + most pain points addressed + competitive necessity |
| 🥉 3 | Event-Driven Integration Modernization | Application | **5.30** | Foundational enabler — consider as parallel infrastructure workstream |

---

## Final Decision (USER OVERRIDE)

**Phase 1 Scope: UC1 + UC2 (Combined Product Concept)**

The stakeholder has decided to pursue BOTH UC1 and UC2 together as a unified Phase 1 initiative.

**Rationale for override:**
- UC1 and UC2 are complementary — UC2 delivers the AI intelligence, UC1 delivers the unified surface physicians interact with
- Together they address 6 of 7 identified pain points
- UC1 without UC2 = unified but not intelligent; UC2 without UC1 = intelligent but fragmented
- Combined product provides the full physician experience transformation

**Phase 1:** Unified Clinical Workflow with embedded Real-time AI-Guided CDS (UC1 + UC2)
**Parallel Workstream:** Event-Driven Integration Modernization (UC3) — enables and supports Phase 1

---

## Override/Dissent Log

| Date | Who | Original | Override | Reason |
|------|-----|----------|----------|--------|
| 2026-07-08 | Jordan Lee (JL) | UC2 alone as top priority | Both UC1 + UC2 for Phase 1 | Complementary: UC2 = AI intelligence, UC1 = unified experience. Together address 6/7 pain points. |

---

## Audit Trail

```
📋 AUDIT LOG
Agent: AI-PDLC Prioritize
Phase: Phase 1 — Use Case Intake
Decision: 3 use cases identified from Discovery Pain Point Board
Scored by: Jordan Lee (JL)
Source: Discovery Agent (Pain Point Board — 7 pain points across 3 categories)
Evidence: Pain points clustered into Clinical Workflow (5), AI/ML Transparency (1), Technical Infrastructure (1) → 3 solution areas
Timestamp: 2026-07-08 17:32 IST
---

📋 AUDIT LOG
Agent: AI-PDLC Prioritize
Phase: Phase 2 — Categorization
Decision: UC1 (Application), UC2 (Agentic), UC3 (Application)
Validated by: Jordan Lee (JL)
Evidence: UC2 involves autonomous AI agent workflow (real-time inference, explainability); UC1/UC3 are traditional application builds
Timestamp: 2026-07-08 17:32 IST
---

📋 AUDIT LOG
Agent: AI-PDLC Prioritize
Phase: Phase 3 — Scoring
Decision: All scores confirmed as suggested
Scored by: Jordan Lee (JL)
Source: Deep Discovery context from Pain Point Board + market assumptions
Evidence: UC2=7.75, UC1=7.40, UC3=5.30. Frameworks: Agentic (UC2), Application (UC1, UC3)
Timestamp: 2026-07-08 18:08 IST
---

📋 AUDIT LOG
Agent: AI-PDLC Prioritize
Phase: Phase 5 — Final Decision (Override)
Decision: Pursue BOTH UC1 + UC2 for Phase 1 (override from UC2-only recommendation)
Validated by: Jordan Lee (JL)
Override by: Jordan Lee (JL)
Reason: UC1 and UC2 are complementary; together address 6/7 pain points
Evidence: UC1 provides unified surface, UC2 provides AI intelligence — combined = complete physician experience transformation
Timestamp: 2026-07-08 18:10 IST
---
```

---

## Recommended Next Step

Proceed to the **AI-PDLC Working Backwards** agent to create a PR/FAQ for the combined product concept (UC1 + UC2): a unified clinical workflow with embedded real-time AI-guided clinical decision support. This validates the product concept with stakeholders before generating prototype specifications.
