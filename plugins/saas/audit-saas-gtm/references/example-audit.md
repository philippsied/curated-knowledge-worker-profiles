<overview>
Annotated example of a completed audit report. Based on a fictional MVP spec
for "TaskPilot AI", an AI-powered project management tool. Use as a quality
anchor for tone, detail level, and format.

Annotations in `[Annotation: ...]` explain scoring rationale. These do NOT
belong in a real audit report.
</overview>

<example_report>
<!-- NOTE: This example shows the GENERATED REPORT FORMAT (markdown).
     The skill itself uses pure XML. Markdown headings below are intentional
     as they represent the output the auditor produces. -->

# SaaS GTM Audit Report: TaskPilot AI

**Audit Date:** 2026-03-15
**Product Stage:** MVP
**Audit Mode:** Deep Audit

---

## Part 1: Executive Summary

**Total Score: 46 / 114 (40%) — 🟠 Developing**

**Maturity:** 🟠 Developing — Fundamentals recognizable, but significant gaps
in AI-specific areas (safety, evaluation, HITL), missing compliance preparation,
and no payment or launch infrastructure.

**Top-3 Strengths:**
1. Clear problem definition with 25 discovery interviews and validated ICP
2. Solid architecture decision (modular monolith + separate inference service)
3. Well-defined onboarding with measurable "aha moment" (first AI-generated project summary)

**Top-3 Critical Gaps:**
1. No guardrails specified (neither input nor output) — launch blocker
2. No evaluation/testing strategy for AI outputs
3. EU AI Act classification missing entirely despite EU data processing

**Overall Assessment:** TaskPilot has a strong product vision and solid SaaS
fundamentals but treats AI like a deterministic feature layer. The spec addresses
none of the AI-specific risks (hallucination, prompt injection, confidence calibration)
and has no plan for human-in-the-loop. Payment infrastructure and launch readiness
are not addressed. Before a beta launch, safety, compliance, and payment gaps must
be closed.

---

## Part 2: Phase Detail Scores (abbreviated for example)

### Phase 1: Strategic Foundation — 2.3 / 3.0

| Area | Score | Assessment |
|------|-------|------------|
| 1.1 Problem and Market | 🟢 3 | 25 interviews, ICP defined (mid-market PM teams, 20-200 employees), JTBD applied |
| 1.2 Pricing | 🟡 2 | Hybrid pricing (per-seat + AI credits) with 3 tiers. But: AI cost per interaction not calculated |
| 1.3 North Star Metric | 🟡 2 | "Projects with AI summary per week" as NSM. But: no guardrail metrics, no leading/lagging distinction |

[Annotation: Phase 1 is strongest because the team takes product discovery seriously.
Score 2 for pricing reflects that AI costs are not calculated — a P1 item for AI-SaaS.]

### Phase 4: Safety — 0.4 / 3.0 (weakest)

| Area | Score | Assessment |
|------|-------|------------|
| 4.1 Input Guardrails | 🔴 0 | Not addressed |
| 4.2 Output Guardrails | 🔴 0 | Not addressed |
| 4.3 HITL | 🟠 1 | "User can edit AI output" — that is user control, not systematic HITL |
| 4.5 EU AI Act | 🔴 0 | Not addressed despite EU user data |

[Annotation: "User can edit" is standard UX, not HITL. HITL requires systematic
confidence-based routing and escalation. This distinction is critical.]

### Phase 7: Payment — 0.0 / 3.0

| Area | Score | Assessment |
|------|-------|------------|
| 7.1 Payment Integration | 🔴 0 | Not addressed — no payment provider, no subscription lifecycle |
| 7.2 AI Economics | 🔴 0 | Not addressed — no cost-per-interaction, no margin targets |

[Annotation: Phase 7 is new in this audit framework. Many specs neglect payment
infrastructure entirely. A 🔴 0 here is a launch blocker.]

### Phase 8: Discovery and Launch — 0.5 / 3.0

| Area | Score | Assessment |
|------|-------|------------|
| 8.1 GEO/AI Discovery | 🔴 0 | Not addressed |
| 8.2 Competitor Intelligence | 🔴 0 | Competitive analysis exists (Phase 1) but no AI visibility audit |
| 8.3 Social and Launch | 🟠 1 | "5 pilot customers" mentioned — basic outreach. No launch strategy. |
| 8.4 Legal and Trust Pages | 🟠 1 | "Privacy policy planned" — mentioned but not implemented |

---

## Part 3: Gap Analysis (abbreviated)

### 🔴 Critical
**No guardrails** (4.1 + 4.2): Prompt injection is a real risk for a tool processing
project data. PII leakage from project data is unmitigated.
→ Define at minimum: prompt injection detection, PII redaction (input), toxicity filter +
PII leakage check (output).

**No payment infrastructure** (7.1): Cannot monetize without active payment processing.
→ Set up Stripe (or equivalent) in live mode, configure subscription lifecycle, test webhooks.

### 🟠 High
**AI costs not calculated** (1.2 + 7.2): Hybrid pricing defines AI credits but actual
cost per interaction is unknown. Risk: margin erosion with heavy AI users.
→ Calculate token costs per use case. Define IER target. Set usage caps per tier.

---

## Part 4: Prioritized Actions (abbreviated)

| # | Action | Phase | Impact | Effort | Priority |
|---|--------|-------|--------|--------|----------|
| 1 | Define input/output guardrails | 4 | High | Medium | 🔴 Immediate |
| 2 | Classify under EU AI Act | 4 | High | Low | 🔴 Immediate |
| 3 | Set up payment provider in live mode | 7 | High | Medium | 🔴 Immediate |
| 4 | Create golden dataset + choose eval framework | 2 | High | Medium | 🟠 Short-term |
| 5 | Calculate AI cost per interaction | 1,7 | High | Low | 🟠 Short-term |
| 6 | Prepare privacy policy and terms | 8 | Medium | Low | 🟠 Short-term |

</example_report>

<auditor_notes>

**Key calibration points for auditors:**
- Award 🔴 0 consistently when something is not mentioned. Do not guess.
- "Mentioned" ≠ "Addressed": "We will be GDPR-compliant" = 🟠 1.
  Concrete measures (legal basis, DPA, DPIA) earn 🟡 2+.
- Think AI-specifically: "User can edit" is standard UX, not HITL.
- Always give concrete actions: not "improve guardrails" but
  "implement prompt injection detection + PII redaction as input guards".
- Phase 4 is the most common weak point. Audit thoroughly.
- Phase 7 and 8 are new — many existing specs will score 🔴 0 here.
  Flag as launch blockers with concrete next steps.
</auditor_notes>
