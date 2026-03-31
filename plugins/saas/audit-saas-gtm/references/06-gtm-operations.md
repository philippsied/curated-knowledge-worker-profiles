<overview>
Stable reference for go-to-market operations: rollout phases, feature flags,
feedback loops, model versioning, acceptance criteria for AI features,
and PRD structure guidance.
</overview>

<rollout_phases>

**Phase 0: Model Validation (pre-alpha)**
Offline eval against test sets, benchmark vs. baselines, ethics/bias review,
data pipeline E2E validated, cost projections confirmed.
Gate: all AI metrics above minimum.

**Phase 1: Internal Alpha (dogfooding)**
Internal team only, feature flag: internal_only.
Monitor: latency, accuracy, edge cases, cost.
Gate: no critical bugs, team confidence in UX.

**Phase 2: Closed Beta**
X% of users or specific segment, feature flag: 5-10%.
A/B test: control vs. treatment.
Gate: statistical significance, no guardrail regressions.

**Phase 3: Open Beta**
25-50% of users. Segmentation analysis: performance consistent across groups.
Gate: confidence in results at scale.

**Phase 4: General Availability**
50% → 75% → 100% over 3-7 days. Continuous monitoring, retrain triggers active.
Beta switch: users can switch back, opt-out tracked.
</rollout_phases>

<feature_flags>

| Flag | Purpose | Default |
|------|---------|---------|
| [feature]_enabled | Master kill switch | false |
| [feature]_percentage | Rollout % | 0 |
| [feature]_model_version | Model version control | v1.0 |
| [feature]_fallback | Fallback mode | true |
</feature_flags>

<rollback>

**Triggers:**

| Trigger | Type | Action |
|---------|------|--------|
| Error rate > X% | Automatic | Feature flag disable |
| AI latency P95 > X ms | Automatic | Fallback model/cache |
| Safety violation | Automatic | Kill switch |
| PII exposure > 0.1% | Automatic | Kill switch + incident |
| User satisfaction &lt; X | Manual | Pause + investigate |
| Cost > $X/day | Manual | Reduce rollout % |

**SLA:** Detect + Decide + Disable ≤ 10 minutes.
</rollback>

<acceptance_criteria>

**Given/When/Then format (AI-extended):**
```gherkin
Scenario: AI-specific behavior
  Given user provides input to AI feature
  When AI generates response
  Then response meets quality threshold
  And confidence indicator is displayed
  And user can provide feedback via thumbs up/down
```

**Definition of Done (AI-extended):**
- Code reviewed
- Unit + integration tests (≥80% coverage)
- AI evaluation suite passing (all metrics above minimum)
- No critical/high bugs
- Accessibility (WCAG 2.1 AA)
- Documentation updated
- Product owner reviewed
- Staging deployed and smoke-tested
</acceptance_criteria>

<feedback_loops>

**Detect → Diagnose → Decide → Deploy:**
Auto-eval on prod logs → trace replay → gate metrics → ship with observability.

**Data collection:**
- Log: inputs, outputs, timestamps, latency, tokens, feedback
- PII anonymize before storage
- 0.5-1% production sample for weekly eval
- Implicit signals: accept/reject, conversation abandonment

**Improvement cadence:**

| Activity | Frequency |
|----------|----------|
| In-the-wild eval review | Weekly |
| Human review sample outputs | Monthly |
| Bias/fairness audit | Quarterly |
| Drift detection | Automated/continuous |
| Retraining trigger review | Monthly |
</feedback_loops>

<model_versioning>

| Artifact | Convention |
|----------|-----------|
| Model weights | Semantic versioning |
| Prompts | Semantic versioning |
| Training data | Date-stamped snapshots |
| Eval datasets | Version-tagged |
| Config | Tied to deployment |
</model_versioning>

<prd_structure>

**Three-phase structure (cross-industry consensus):**
1. Problem alignment — problem, personas, hypothesis, metrics
2. Solution alignment — scope, stories, flows, risks, decisions
3. Launch readiness — AI/ML specs, architecture, compliance, rollout

**AI-specific PRD sections:**
Model strategy and evaluation matrix, prompt engineering specifications,
AI safety and guardrails (input/processing/output), hallucination handling,
feedback loops and continuous improvement, model versioning,
EU AI Act classification, responsible AI checklist.

**Metrics framework (three levels):**
1. Primary metrics (must move): baseline, MVP target, stretch target, measurement
2. Secondary metrics (should move): baseline, target, measurement
3. Guardrail metrics (must not regress): current, minimum, alert threshold
</prd_structure>
