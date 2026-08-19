# PROTOTYPE SPECIFICATION — AnyCompany ClinicalFlow™

**Date:** 2026-07-08  
**Participant:** Jordan Lee (JL), Senior Solutions Architect, AWS  
**Space:** AnyCompanyClinicalConsultation-v1  
**Mode:** Deep Discovery  
**Source:** AI-PDLC Pipeline (Discovery → Prioritize → Working Backwards → Spec Generator)  
**Use Cases:** UC1 (Unified Clinical Workflow) + UC2 (Real-time AI-Guided CDS) — combined Phase 1

---

## 1. Product Overview

### 1.1 Product Name
**AnyCompany ClinicalFlow™**

### 1.2 Vision Statement
A unified clinical workspace with embedded real-time, explainable AI-guided clinical decision support — enabling physicians to complete consultations faster with higher guideline adherence and fewer errors, all within a single pane.

### 1.3 Problem Statement
Physicians currently navigate 4 disconnected systems (UI Portal, Integration Layer, AnyCompanyEMR, AnyCompany Insight AI) during clinical consultations. This fragmentation causes:
- Cognitive overload from constant system-switching
- Missed clinical guidelines (no real-time evaluation at point of care)
- Opaque AI reasoning (AnyCompany Insight AI provides recommendations without explainability)
- Error-prone manual data entry and cross-system reconciliation
- Time-consuming consultations (14+ minutes avg vs. target 8 minutes)

### 1.4 Solution
A single-pane clinical workspace that consolidates patient context, real-time AI guideline recommendations (with chain-of-thought explainability), and inline order placement — with the physician always maintaining final control.

### 1.5 MVP Scope
- **In Scope:** Single specialty (Internal Medicine), US-first launch, core consultation workflow, real-time AI recommendations with explainability, inline order placement
- **Out of Scope (Phase 2+):** Multi-specialty expansion, EU launch (GDPR/EU AI Act certification), offline mode, advanced analytics dashboard, patient-facing features

---

## 2. Target Users & Personas

### 2.1 Primary Persona: Dr. Martinez (Specialist Physician)
- **Role:** Internal Medicine specialist
- **Context:** 10-15 consultations/day, desktop workstation in office
- **Pain:** Switches between 4 systems per consultation; misses guidelines; spends 14+ min/consultation
- **Goal:** Complete consultations in ≤8 min with AI assistance while maintaining full clinical authority
- **Trust Level:** Skeptical of AI — needs transparent reasoning to trust recommendations

### 2.2 Secondary Persona: Dr. Patel (General Practitioner)
- **Role:** GP with broader case variety
- **Context:** 20-25 consultations/day, faster pace
- **Pain:** Less specialty depth — relies more on guidelines but can't look them up fast enough
- **Goal:** Quick guideline validation during high-volume consultation days

### 2.3 Tertiary Persona: Nurse Williams (Clinical Support)
- **Role:** Nurse supporting physician consultations
- **Context:** Assists with order preparation, patient triage
- **Pain:** Re-enters data from physician verbal orders into separate CPOE system
- **Goal:** See physician orders inline and prepare/verify without system-switching

---

## 3. Device & Platform Requirements

### 3.1 Primary Platform
- **Device:** Desktop workstation (physician office / nurse station)
- **Resolution:** 1920×1080 (primary), support down to 1366×768 (common hospital monitors)
- **Browsers:** Chrome 100+, Edge 100+ (Windows desktops typical in clinical settings)
- **OS:** Windows 10/11 (primary), macOS (secondary)

### 3.2 Secondary Platform
- **Device:** Tablet (iPad Pro / Surface Pro) for bedside rounds
- **Resolution:** 1024×768 minimum
- **Interaction:** Touch-optimized (≥44px touch targets)

### 3.3 Offline Capability
- Not required for MVP (real-time AI inference requires connectivity)
- Graceful degradation: if AnyCompany Insight API unavailable, show cached guidelines with "potentially outdated" flag

---

## 4. Brand & Design Guidelines

### 4.1 Color Palette
| Token | Value | Usage |
|-------|-------|-------|
| Primary | #1E3A5F (Deep Blue) | Headers, navigation, primary actions |
| Secondary | #4A90D9 (Medical Blue) | Interactive elements, links |
| Success/Confirm | #2E7D32 (Clinical Green) | Confirm actions, positive indicators |
| Warning | #F57C00 (Amber) | Low confidence, needs attention |
| Danger | #C62828 (Red) | Errors, critical alerts, high-risk indicators |
| Background | #F5F7FA (Light Gray) | Page background |
| Surface | #FFFFFF (White) | Cards, panels |
| Text Primary | #1A1A1A | Body text |
| Text Secondary | #5A6872 | Labels, metadata |

### 4.2 Typography
- **Font Family:** Inter (primary), system sans-serif (fallback)
- **Body Text:** 16px minimum (clinical readability requirement)
- **Headers:** H1: 28px, H2: 22px, H3: 18px
- **AI Recommendation Text:** 16px, slightly elevated line-height (1.6) for readability
- **Confidence Score:** 14px monospace with color coding

### 4.3 Design Principles
1. **Clinical Clarity** — Every element serves a clinical purpose. No decorative UI.
2. **Glanceable** — Critical information (AI confidence, patient alerts) visible without interaction
3. **Physician Authority** — AI recommendations are *suggestions*, never directives. Visual hierarchy always prioritizes physician workspace over AI panel.
4. **Progressive Disclosure** — Show confidence scores upfront; detailed reasoning available on-demand (expand)
5. **High Contrast** — Designed for bright hospital lighting environments
6. **No Color-Only Encoding** — Icons + labels + color for all status indicators (accessibility)

### 4.4 Accessibility
- WCAG AA compliance minimum
- High contrast mode support
- Minimum 16px body text
- Touch targets ≥44px
- No reliance on color alone for critical information
- Screen reader compatible (ARIA labels)
- Keyboard navigable (Tab/Enter for all primary actions)

### 4.5 Design Inspirations
- A leading EMR vendor physician view (workflow density, information architecture)
- Ambient clinical documentation tools (AI integration UX, ambient AI pattern)
- Established explainable-AI interfaces (confidence visualization, reasoning display)

---

## 5. Feature Specifications

### 5.1 Patient Context Panel (Left Section)

**Purpose:** Provides complete patient context at a glance without system-switching.

| Element | Description | Source |
|---------|-------------|--------|
| Patient Header | Name, DOB, MRN, allergies badge | AnyCompanyEMR via FHIR |
| Active Conditions | Current diagnoses with ICD-10 codes | AnyCompanyEMR |
| Current Medications | Active prescriptions with dosage | AnyCompanyEMR |
| Recent Labs | Last 5 relevant lab results with trend indicators | AnyCompanyEMR |
| Visit History | Last 3 visits, condensed | AnyCompanyEMR |
| Risk Flags | AI-computed risk indicators (color-coded) | AnyCompany Insight AI |

**Behavior:**
- Auto-populates when consultation opens
- Collapsible sections (patient can expand/collapse)
- Sticky header (always visible even when scrolling)
- Data refreshes every 60 seconds during active consultation

### 5.2 AI Recommendation Panel (Center Section)

**Purpose:** Delivers real-time, explainable AI guideline recommendations at point of care.

| Element | Description | Source |
|---------|-------------|--------|
| Recommendation Card | Actionable recommendation with confidence score | AnyCompany Insight AI |
| Confidence Indicator | HIGH (≥80%, green), MEDIUM (50-79%, amber), LOW (<50%, red) | AnyCompany Insight AI |
| Chain-of-Thought | Expandable reasoning: "Based on [data points], guideline [X] recommends..." | AnyCompany Insight AI |
| Guideline Citation | Link to source guideline (ACC/AHA, NICE, etc.) | Clinical DB |
| Supporting Evidence | Patient-specific data points that triggered this recommendation | AnyCompany Insight AI + EMR |
| Action Buttons | "Accept & Order" / "Modify" / "Override" / "Dismiss" | UI |

**AI Interaction Specifications:**
- Recommendations appear within 2 seconds of consultation opening
- Maximum 3 recommendations visible at once (prioritized by confidence)
- Chain-of-thought explanation available on click/expand (not forced)
- Confidence scores use both color AND text label (accessibility)
- "Why this recommendation?" link expands full reasoning

**Confidence Score Display:**
```
HIGH (≥80%):   ████████░░ 85% — Strong evidence
MEDIUM (50-79%): █████░░░░░ 62% — Review recommended  
LOW (<50%):    ███░░░░░░░ 35% — Limited evidence ⚠️
```

### 5.3 Orders & Actions Panel (Right Section)

**Purpose:** Inline order placement without system-switching.

| Element | Description | Source |
|---------|-------------|--------|
| Order Queue | Pending orders from AI recommendations (accepted) | Local state |
| Quick-Add Order | Manual order entry (medication, lab, referral) | CPOE |
| Order Preview | Auto-populated order details from AI recommendation | AnyCompany Insight AI → CPOE |
| Confirmation | "Review & Submit" with order summary | CPOE |
| Order History | Today's placed orders (collapsible) | CPOE |

**Behavior:**
- "Accept & Order" on AI recommendation auto-populates order form
- Physician reviews, optionally modifies, then confirms
- Order submitted to CPOE system in background
- Confirmation toast with order ID
- Re-authentication required for controlled substance orders

### 5.4 Physician Override Flow

**Trigger:** Physician clicks "Override" on any AI recommendation.

**Flow:**
1. Modal appears: "Why are you overriding this recommendation?"
2. Dropdown options:
   - "Patient-specific contraindication"
   - "Guideline not applicable to this case"
   - "Clinical judgment — different approach preferred"
   - "Patient preference"
   - "Other" (free text)
3. Optional: Free-text explanation (encouraged, not required)
4. Confirm override
5. Override logged to audit trail
6. Feedback sent to AnyCompany Insight AI for model improvement (anonymized)

**Key Principle:** Zero friction. Override should take <5 seconds. Never guilt or question the physician.

---

## 6. Data Architecture

### 6.1 Data Sources

| Source | Protocol | Data | Latency |
|--------|----------|------|---------|
| AnyCompanyEMR | FHIR R4 | Patient demographics, conditions, medications, labs, history | <1s |
| AnyCompany Insight AI | REST API (real-time) | Guideline recommendations, confidence scores, reasoning chains | <2s |
| Clinical Guideline DB | REST API | Guideline text, citations, version metadata | <500ms |
| CPOE System | HL7 FHIR | Order submission, order status, formulary | <1s |

### 6.2 Data Flow

```
[Consultation Opens]
    │
    ├── FHIR R4 → AnyCompanyEMR → Patient Context Panel
    │
    ├── Patient Context → AnyCompany Insight AI API → AI Recommendations
    │       └── Chain-of-thought reasoning generated
    │
    └── Guideline DB → Citation enrichment
    
[Physician Action]
    │
    ├── "Accept & Order" → Order auto-populated → CPOE submission
    ├── "Override" → Override logged → Feedback to AnyCompany Insight AI
    └── "Dismiss" → Logged, no action
```

### 6.3 Integration Layer (Event-Driven — UC3 Foundation)

- AWS EventBridge for async event routing
- API Gateway + Lambda for synchronous FHIR queries
- SQS for order submission queue (reliability)
- CloudWatch for health monitoring and alerting
- All data encrypted in transit (TLS 1.3) and at rest (AES-256)

---

## 7. Authentication & Authorization

### 7.1 Authentication
- SAML 2.0 SSO via the hospital enterprise identity provider
- Session timeout: 30 minutes idle
- Re-authentication required for:
  - Controlled substance orders
  - Override of HIGH-confidence AI recommendations
  - Access to full patient history beyond current consultation

### 7.2 Role-Based Access Control (RBAC)

| Role | Access Level |
|------|-------------|
| Physician | Full: view patient data, see AI recommendations, place orders, override AI |
| Nurse | Partial: view patient data, see pending orders, verify orders (cannot place new orders independently) |
| Admin | Configuration: manage user roles, view usage analytics, no patient data access |
| Auditor | Read-only: view audit trail, override logs, compliance reports |

---

## 8. Key Screens — Layout Specifications

### Screen 1: Consultation Workspace (Primary)

```
┌─────────────────────────────────────────────────────────────────┐
│  HEADER: AnyCompany ClinicalFlow™  |  Dr. Martinez  |  ⚙️  🔔  │
├──────────────┬───────────────────────────┬──────────────────────┤
│              │                           │                      │
│  PATIENT     │   AI RECOMMENDATIONS      │   ORDERS & ACTIONS   │
│  CONTEXT     │                           │                      │
│              │  ┌─────────────────────┐  │  ┌────────────────┐  │
│  [Name/MRN]  │  │ Recommendation 1    │  │  │ Pending Orders │  │
│  [Allergies] │  │ Confidence: 85% ██▓ │  │  │ • Anticoag..  │  │
│              │  │ "Consider adding..." │  │  │ • Lab order.. │  │
│  ─────────── │  │ [Accept] [Override]  │  │  │                │  │
│  Conditions  │  └─────────────────────┘  │  ├────────────────┤  │
│  • Afib      │                           │  │ [+ Quick Add]  │  │
│  • HTN       │  ┌─────────────────────┐  │  │                │  │
│  • T2DM      │  │ Recommendation 2    │  │  ├────────────────┤  │
│              │  │ Confidence: 62% ██░ │  │  │ Today's Orders │  │
│  ─────────── │  │ "Review lipid..."   │  │  │ ✓ Metformin   │  │
│  Medications │  │ [Accept] [Override]  │  │  │ ✓ ECG order   │  │
│  • Metformin │  └─────────────────────┘  │  │                │  │
│  • Lisinopri │                           │  │                │  │
│              │  ┌─────────────────────┐  │  │                │  │
│  ─────────── │  │ 💡 "Why these?"     │  │  │                │  │
│  Recent Labs │  │ [Expand reasoning]  │  │  │                │  │
│  • HbA1c 7.2 │  └─────────────────────┘  │  │                │  │
│  • LDL 142  │                           │  │                │  │
│              │                           │  │                │  │
├──────────────┴───────────────────────────┴──────────────────────┤
│  FOOTER: Consultation Timer: 04:32  |  Auto-saved  |  [Complete]│
└─────────────────────────────────────────────────────────────────┘
```

**Column Widths:** Patient Context (25%) | AI Recommendations (45%) | Orders (30%)

### Screen 2: AI Recommendation Detail (Expanded Explainability)

```
┌─────────────────────────────────────────────────────────────────┐
│  ← Back to Consultation                                         │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  RECOMMENDATION: Consider adding anticoagulant therapy           │
│  Confidence: 85% ████████░░ HIGH                                │
│                                                                  │
│  ─────────────── REASONING CHAIN ──────────────────────────     │
│                                                                  │
│  Step 1: Patient has confirmed Atrial Fibrillation (I48.91)     │
│  Step 2: CHA₂DS₂-VASc Score calculated: 4                      │
│      • Age 72 (+1) • Hypertension (+1) • Diabetes (+1)          │
│      • Prior stroke (+2, if applicable)                          │
│  Step 3: ACC/AHA 2023 Guideline 4.3.2 recommends:              │
│      "Oral anticoagulation is recommended for patients with      │
│       AF and CHA₂DS₂-VASc score ≥2 in men or ≥3 in women"      │
│  Step 4: No documented contraindications found in patient record │
│                                                                  │
│  ─────────────── GUIDELINE SOURCE ─────────────────────────     │
│  📄 ACC/AHA/ACCP/HRS 2023 Guideline for Management of AF       │
│     Section 4.3.2, Recommendation Class I, Level of Evidence A  │
│     [View Full Guideline ↗]                                      │
│                                                                  │
│  ─────────────── PATIENT DATA USED ────────────────────────     │
│  • Diagnosis: Atrial Fibrillation (confirmed 2025-03-15)        │
│  • Age: 72 | Sex: Male                                           │
│  • Comorbidities: HTN, T2DM                                     │
│  • Current anticoagulation: None                                 │
│  • Bleeding risk (HAS-BLED): 2 (moderate)                       │
│                                                                  │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐          │
│  │ Accept & Order│  │   Modify     │  │   Override   │          │
│  └──────────────┘  └──────────────┘  └──────────────┘          │
└─────────────────────────────────────────────────────────────────┘
```

### Screen 3: Order Confirmation

```
┌─────────────────────────────────────────────────────────────────┐
│  ORDER CONFIRMATION                                              │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  Patient: John Smith (MRN: 12345678)                            │
│  Ordering Physician: Dr. Martinez                                │
│                                                                  │
│  ─────────────── ORDER DETAILS ─────────────────────────────    │
│                                                                  │
│  Medication: Apixaban (Eliquis) 5mg                             │
│  Route: Oral                                                     │
│  Frequency: BID (twice daily)                                    │
│  Duration: Ongoing                                               │
│  Refills: 3                                                      │
│                                                                  │
│  ─────────────── AI SOURCE ─────────────────────────────────    │
│  Generated from: AI Recommendation #1 (Confidence: 85%)         │
│  Guideline: ACC/AHA 2023 §4.3.2                                 │
│                                                                  │
│  ─────────────── SAFETY CHECKS ─────────────────────────────   │
│  ✅ No drug interactions detected                                │
│  ✅ No allergy conflicts                                         │
│  ⚠️ Monitor: Renal function (eGFR) — adjust dose if <25         │
│                                                                  │
│  ┌──────────────────────┐  ┌──────────────────────┐            │
│  │   Confirm & Submit    │  │   Edit Before Submit  │            │
│  └──────────────────────┘  └──────────────────────┘            │
└─────────────────────────────────────────────────────────────────┘
```

### Screen 4: Patient Summary Dashboard

```
┌─────────────────────────────────────────────────────────────────┐
│  PATIENT SUMMARY DASHBOARD                         [Filter ▼]   │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  TODAY'S CONSULTATIONS                                           │
│  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐          │
│  │ Completed │ │ In Progress│ │  Pending │ │ AI Accept│          │
│  │    8      │ │     1     │ │    3     │ │   78%    │          │
│  └──────────┘ └──────────┘ └──────────┘ └──────────┘          │
│                                                                  │
│  ACTIVE CONSULTATIONS                                            │
│  ┌─────────────────────────────────────────────────────────┐    │
│  │ Patient      │ Status    │ AI Recs │ Orders  │ Duration  │    │
│  ├─────────────────────────────────────────────────────────┤    │
│  │ J. Smith     │ ● Active  │ 2 (1✓)  │ 1 pend  │ 04:32    │    │
│  │ M. Johnson   │ ○ Pending │ —       │ —       │ —        │    │
│  │ R. Williams  │ ○ Pending │ —       │ —       │ —        │    │
│  └─────────────────────────────────────────────────────────┘    │
│                                                                  │
│  AI PERFORMANCE (This Week)                                      │
│  • Guideline Adherence: 91% (+3% vs last week)                  │
│  • Avg Consultation Time: 8.2 min (-2.1 min vs baseline)        │
│  • Override Rate: 12% (top reason: patient preference)           │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

## 9. User Flows

### 9.1 Primary Flow: Guided Consultation

```
Start → Open Consultation → Patient Context Auto-loads
  → AI Recommendations Appear (≤2s) → Physician Reviews
  → [Decision Point]
     ├── Accept → Order Auto-populates → Confirm → Submit → ✓
     ├── Modify → Edit order → Confirm → Submit → ✓
     ├── Override → Capture reason → Log → Continue → ✓
     └── Dismiss → Log → Continue
  → [More recommendations?]
     ├── Yes → Loop back to Review
     └── No → Complete Consultation → Summary Saved → ✓
```

### 9.2 Edge Case: AnyCompany Insight AI Timeout

```
Consultation Opens → FHIR data loads ✓ → AnyCompany Insight API call...
  → [Timeout after 5s]
  → Display: "AI recommendations temporarily unavailable"
  → Show cached/static guidelines (if available) with ⚠️ "Potentially outdated"
  → Physician continues manually
  → Background retry every 30s
  → If AnyCompany Insight AI recovers → Recommendations appear with "Delayed" badge
```

### 9.3 Edge Case: Low Confidence Recommendation

```
AI Returns Recommendation with Confidence <50%
  → Display with ⚠️ amber indicator
  → Add label: "Limited evidence — review recommended"
  → "Accept & Order" button disabled (must click "Review Details" first)
  → After reviewing reasoning → "Accept & Order" becomes available
  → Explicit acknowledgment logged: "Physician accepted low-confidence recommendation"
```

---

## 10. Non-Functional Requirements

### 10.1 Performance

| Metric | Target | Critical Threshold |
|--------|--------|-------------------|
| Page Load (Consultation) | <2s | <4s |
| AI Recommendation Latency | <2s | <5s |
| Order Submission | <1s | <3s |
| Patient Context Load | <1s | <2s |
| FHIR API Response | <500ms | <1s |

### 10.2 Availability
- 99.9% uptime (clinical system — no planned downtime during business hours)
- Graceful degradation if AI unavailable (EMR + manual workflow continues)
- Multi-AZ deployment (AWS)

### 10.3 Security & Compliance
- **HIPAA:** PHI encrypted at rest (AES-256) and in transit (TLS 1.3), audit logging for all access, BAA with AWS
- **GDPR:** Data residency controls (EU data stays in EU region), right to erasure, data processing agreements
- **EU AI Act:** High-risk AI system documentation, transparency reports, human oversight mechanisms (physician override), bias monitoring
- **Audit Trail:** Every AI recommendation, physician action, and override logged with timestamp, user ID, and context

### 10.4 Scalability
- Support 200-500 concurrent physicians
- Handle 2,000-5,000 consultations/day
- Auto-scaling based on consultation volume patterns (peak morning hours)
- CDN for static assets

---

## 11. Design Handoff Brief (for Designer Agent)

### 11.1 Personas
- Primary: Dr. Martinez (specialist physician, skeptical of AI, values efficiency)
- Secondary: Dr. Patel (GP, high-volume, values quick validation)
- Tertiary: Nurse Williams (clinical support, order verification)

### 11.2 Emotional Target
- **Before ClinicalFlow:** Frustrated, cognitively overloaded, time-pressured, distrustful of opaque AI
- **After ClinicalFlow:** Confident, efficient, in-control, informed, trusting (because reasoning is visible)

### 11.3 User Journey Summary
1. **Open Consultation** → Patient context loads instantly (relief: "everything I need is here")
2. **See AI Recommendations** → Confidence scores give quick signal (trust: "I can see why it thinks this")
3. **Review Reasoning** → Chain-of-thought validates clinical judgment (confidence: "this aligns with what I know")
4. **Take Action** → One-click order placement (efficiency: "that was fast")
5. **Complete** → Consultation done in 8 min (satisfaction: "I have more time for my patient")

### 11.4 Brand Context
- Colors: Deep blue primary, clinical green for confirmations, amber for warnings
- Typography: Inter, 16px minimum, clinical clarity
- Aesthetic: Professional healthcare, high-contrast, no decorative elements
- Inspiration: established clinical workflow UX patterns — information density, ambient AI integration, and explainable-AI presentation

### 11.5 Key Screens for Wireframing
1. Consultation Workspace (3-column: patient | AI | orders)
2. AI Recommendation Detail (expanded reasoning)
3. Order Confirmation (safety checks visible)
4. Patient Summary Dashboard (metrics + active consultations)

---

## 12. Success Metrics (from PR/FAQ)

| KPI | Target | Measurement |
|-----|--------|-------------|
| Consultation Time Reduction | ≥25% (14 min → ≤10.5 min) | Avg time from open to complete |
| Guideline Adherence | ≥90% | % consultations with at least one AI recommendation accepted |
| Missed Diagnosis Reduction | ≥30% | Chart review audit vs. baseline |
| Physician NPS | ≥50 | Quarterly survey |
| AI Acceptance Rate | ≥70% | Recommendations accepted / total shown |
| Error Reduction | ≥50% | Transcription/data entry errors vs. baseline |
| Physician Churn | <10% | Annual retention rate |

---

## 13. Timeline

| Phase | Timeline | Deliverable |
|-------|----------|-------------|
| MVP (Internal Medicine, US) | 6 months | Core consultation workflow + AI recommendations + orders |
| Phase 2 (Multi-specialty) | +3 months | Cardiology, Oncology expansion |
| Phase 3 (EU Launch) | +3 months | GDPR/EU AI Act certification + EU deployment |
| Phase 4 (Analytics) | +3 months | Advanced physician dashboard, population health insights |

---

## 14. Assumptions (Labeled)

| # | Assumption | Basis |
|---|-----------|-------|
| 1 | Brand uses blue/white palette with green accents | Healthcare industry standard, professional clinical aesthetic |
| 2 | Desktop-first, responsive to tablet | Clinical workstation is primary physician environment |
| 3 | 1920×1080 primary resolution | Common hospital monitor specification |
| 4 | SAML SSO with hospital IdP | Enterprise healthcare standard auth pattern |
| 5 | WCAG AA accessibility minimum | Healthcare regulatory expectation + clinical environment needs |
| 6 | Established clinical workflow UX patterns as design inspiration | Proven information density and explainability conventions |

---

📋 AUDIT LOG  
Agent: AI-PDLC Spec Generator  
Phase: Full Spec Generation (Headless Mode)  
Decision: Generated complete prototype specification for AnyCompany ClinicalFlow™ (UC1 + UC2 combined)  
Details:  
- 4 key screens specified with layout diagrams  
- 6 data sources integrated via FHIR R4, REST, HL7  
- AI interaction model: real-time suggestions with chain-of-thought explainability  
- 3 user flows documented (primary + 2 edge cases)  
- Design Handoff Brief included for Designer agent  
- 14 assumptions clearly labeled  
Artifact: SPEC-GENERATOR-prototype-spec.md  
Artifact location: AnyCompanyClinicalConsultation-v1  
Validated by: Jordan Lee (JL)  
Evidence: All inputs from Discovery, Prioritize, and Working Backwards stages  
Timestamp: 2026-07-08 18:29 IST  
---
