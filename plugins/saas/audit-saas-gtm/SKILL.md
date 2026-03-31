---
name: audit-saas-gtm
description: >
  Conducts a structured go-to-market audit for SaaS products across 8 phases:
  product strategy, AI/ML core, UX design, safety/compliance, architecture,
  GTM operations, payment infrastructure, and discovery/launch readiness.
  Scores 38 areas (0-114) with prioritized action items.
  Works for both AI-SaaS and non-AI SaaS (AI phases marked N/A automatically).
  Use when: 'audit my SaaS', 'GTM audit', 'launch readiness check',
  'review my SaaS spec', 'go-to-market review', 'SaaS readiness assessment',
  'product audit', 'spec review', 'PRD audit', 'is my SaaS ready to launch',
  or when user uploads a PRD, product specification, or concept document.
---

<objective>
Perform a systematic go-to-market audit of a SaaS product or specification.
The audit evaluates 38 areas across 8 phases using a 0-3 scoring system,
produces a structured report with gap analysis and prioritized recommendations,
and adapts to the product's maturity stage (pre-MVP, MVP, scaling).

Supports both AI-SaaS (full 8-phase audit, 114 points max) and non-AI SaaS
(AI-specific phases marked N/A, score recalculated proportionally).
</objective>

<quick_start>
1. Read `references/audit-checklist.md` — the master checklist for all 38 areas
2. Gather context from the user (what, phase, depth, focus)
3. Route to the appropriate workflow:
   - **Deep Audit**: All 8 phases, full scoring, detailed report
   - **Quick Check**: P0 items only, compact report, top gaps
4. Present the audit report and offer follow-up actions
</quick_start>

<essential_principles>

**Scoring schema** — rate each of the 38 areas on a 4-level scale:

| Score | Label | Meaning |
|-------|-------|---------|
| 🔴 0 | Missing | Not addressed or mentioned |
| 🟠 1 | Incomplete | Mentioned but lacking depth or specifics |
| 🟡 2 | Solid | Addressed with concrete details, but gaps in best practices |
| 🟢 3 | Gold Standard | Fully addressed, meets or exceeds best practices |

**Scoring rules:**
- **Do not guess**: If the spec does not mention something, it is 🔴 0 — not 🟡 2 because it was "probably intended"
- **Context matters**: An MVP does not need SOC-2, but needs a security roadmap statement
- **Demand evidence**: Metrics without targets = 🟠 1, with targets = 🟡 2, with targets + measurement method = 🟢 3
- **Phase-appropriate**: Pre-MVP specs are measured against pre-MVP standards, not enterprise standards
- **"Mentioned" ≠ "Addressed"**: "We will be GDPR-compliant" is 🟠 1. Concrete measures (legal basis, DPA, DPIA) earn 🟡 2+
- **AI-specific thinking**: "User can edit" is standard UX, not HITL. HITL requires systematic confidence-based routing and escalation

**Score aggregation:**
- **Area score** (0-3): Based on P0/P1/P2 item fulfillment
- **Phase score** (0.0-3.0): Arithmetic mean of area scores in that phase
- **Total score**: Sum of all area scores (0-114 for AI-SaaS, proportional for non-AI)

**Non-AI SaaS handling:**
When the product has no AI/ML components, mark these as N/A (NOT 🔴 0):
Phase 2 (all), areas 4.1-4.4, area 5.2, area 7.2.
Remaining: ~24 areas, max 72 points. Recalculate maturity percentage accordingly.

**Phase weighting by product maturity:**

| Stage | Weight Focus |
|-------|-------------|
| Pre-MVP | Phase 1 (40%), Phase 2 (30%), rest 10% each |
| MVP | Phases 1-3 at 20% each, 4-5 at 15%, 6-8 at 10% |
| Scaling | All phases ~12.5% equally, enterprise standards |

</essential_principles>

<intake>
Before starting the audit, clarify with the user:

1. **What is being audited?** — PRD, concept document, feature spec, live product?
2. **What stage is the product in?** — Ideation, pre-MVP, MVP, scaling?
3. **What audit depth?** — Quick Check (P0 only) or Deep Audit (all 8 phases)?
4. **Any specific focus areas?** — e.g. only compliance, only architecture?
5. **Audience for the audit?** — Technical team, stakeholders, investors?

Use AskUserQuestion with appropriate options for each.

**Data freshness check:** The `-2026` reference files contain time-sensitive benchmarks
(research date: Q1/2026). If the current date is past Q1/2027, inform the user that
benchmarks may need validation and offer to verify key data points via web search
before applying them in the audit.
</intake>

<routing>

**Decision tree:**
- User explicitly requests "quick check" or "quick scan" → **Quick Check**
- Spec is &lt;2 pages or clearly early-stage (ideation/pre-MVP) → **Quick Check** (recommend, confirm with user)
- User requests "deep audit", "full audit", or "comprehensive review" → **Deep Audit**
- Spec is ≥3 pages with multiple sections → **Deep Audit** (default)
- Unclear → Ask user via AskUserQuestion

**Quick Check** → Load `workflows/quick-check.md`
- Evaluates P0 items only across all 8 phases
- Output: Executive summary + top-10 gaps + top-5 actions
- Offers Deep Audit as follow-up

**Deep Audit** → Load `workflows/deep-audit.md`
- Evaluates all P0/P1/P2 items, scores 38 areas
- Loads phase-specific references on demand
- Output: Full 4-part audit report (executive summary, phase scores, gap analysis, prioritized actions)

**Note:** The audit report is generated as **markdown** (.md file) — not XML.
The skill uses XML internally, but the output for the user follows the template
in `templates/audit-report.md`.

</routing>

<reference_index>

**Always load:**
- `references/audit-checklist.md` — Master checklist, 38 areas, P0/P1/P2 items

**Load by phase (on demand):**
| Reference | Primary Phase | Secondary |
|-----------|--------------|-----------|
| `references/01-product-strategy.md` | Phase 1 | Phase 3 (onboarding), Phase 6 (metrics) |
| `references/02-ux-ai-design.md` | Phase 3 | Phase 4 (trust ↔ safety) |
| `references/03-architecture-infra.md` | Phase 5 | Phase 2 (CI/CD for prompts) |
| `references/04-ai-ml-quality.md` | Phase 2 | Phase 4 (hallucination detection) |
| `references/05-safety-compliance.md` | Phase 4 | Phase 2 (confidence scoring), Phase 5 (security) |
| `references/06-gtm-operations.md` | Phase 6 | Phase 2 (acceptance criteria), Phase 5 (performance) |
| `references/07-payment-revenue.md` | Phase 7 | Phase 1 (pricing) |
| `references/08-discovery-launch.md` | Phase 8 | Phase 6 (launch metrics) |

**Time-sensitive data (updated periodically):**
| Reference | Content | Research Date |
|-----------|---------|---------------|
| `references/pricing-2026.md` | Pricing trends, models, verified benchmarks | Q1/2026 |
| `references/metrics-2026.md` | AI-first SaaS metrics, IER, margins, GEO data | Q1/2026 |
| `references/platform-guides-2026.md` | Product Hunt, social media, platform-specific tactics | Q1/2026 |

**Templates:**
- `templates/audit-report.md` — 4-part report structure (COPY + FILL)

**Scripts:**
- `scripts/tech-audit.sh` — SSL, security headers, Core Web Vitals check
- `scripts/competitor-visibility.sh` — AI visibility query generator

**Example:**
- `references/example-audit.md` — Annotated example of a completed audit report

**Evaluation:**
- `evals/audit-saas-gtm-evals.json` — 4 test specifications for skill validation

</reference_index>

<success_criteria>
The audit is complete when:
- [ ] Context gathered (what, stage, depth, focus, audience)
- [ ] Correct workflow routed (quick check or deep audit)
- [ ] All applicable areas scored with evidence-based rationale
- [ ] Non-AI areas correctly marked N/A (if applicable)
- [ ] Gap analysis grouped by criticality (🔴 critical / 🟠 high / 🟡 medium)
- [ ] Action items prioritized by impact × effort
- [ ] Report delivered as markdown file
- [ ] User offered follow-up options (deep-dive, text suggestions, task list)
</success_criteria>
