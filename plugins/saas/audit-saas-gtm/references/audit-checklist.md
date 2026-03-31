<table_of_contents>
- Phase 1: Strategic Foundation (3 areas, max 9)
- Phase 2: AI/ML Core (5 areas, max 15) — N/A if non-AI
- Phase 3: UX and AI Interaction Design (5 areas, max 15)
- Phase 4: Safety, Guardrails and Compliance (7 areas, max 21)
- Phase 5: Architecture and Infrastructure (8 areas, max 24)
- Phase 6: Go-to-Market and Operations (4 areas, max 12)
- Phase 7: Payment and Revenue Infrastructure (2 areas, max 6)
- Phase 8: Discovery, Visibility and Launch Readiness (4 areas, max 12)
- Scoring Summary (38 areas, max 114)
</table_of_contents>

<overview>
SaaS GTM Audit Checklist — consolidated from industry best practices (Google PAIR,
Microsoft HAX, Apple HIG, Anthropic, a16z, McKinsey) and operational launch frameworks.

Each item is tagged:
- **P0** = Must-Have (launch blocker)
- **P1** = Should-Have (production readiness standard)
- **P2** = Could-Have (differentiation / gold standard)

For time-sensitive benchmarks and data points, see the dedicated `-2026` reference files.
</overview>

<phase id="1" name="Strategic Foundation">

<area id="1.1" name="Problem and Market Validation">
- [ ] P0 — Clearly defined user problem with evidence (interviews, data, tickets)
- [ ] P0 — Ideal Customer Profile (ICP) specified
- [ ] P0 — Value hypothesis formulated and testable
- [ ] P1 — TAM/SAM/SOM market sizing available
- [ ] P1 — Jobs-to-be-Done or comparable framework applied
- [ ] P1 — Competitive analysis with differentiation strategy
- [ ] P2 — Sean-Ellis test planned or conducted (target: ≥40% "very disappointed")
- [ ] P2 — PMF indicators defined (churn &lt;2%, DAU/MAU >40%, organic share >30%)
</area>

<area id="1.2" name="Pricing and Monetization">
- [ ] P0 — Pricing model defined (freemium, per-user, usage-based, hybrid)
- [ ] P1 — Value-based pricing (not cost-plus)
- [ ] P1 — At least two pricing tiers defined
- [ ] P1 — AI cost per interaction calculated and reflected in pricing (if AI product)
- [ ] P2 — Usage-based elements for AI features integrated
- [ ] P2 — Annual contract option with discount (benchmark: ~28%)
- [ ] P2 — Pricing review cadence defined (at least semi-annually)

For current pricing trends and models, see `pricing-2026.md`.
</area>

<area id="1.3" name="North Star Metric and Success Measurement">
- [ ] P0 — Primary success metrics defined with target values
- [ ] P0 — Guardrail metrics defined (must not regress)
- [ ] P1 — North Star Metric identified (drives revenue, reflects customer value)
- [ ] P1 — Leading vs. lagging indicators distinguished
- [ ] P1 — Measurement methods and tools specified
- [ ] P2 — Metrics differentiated by minimum / target / stretch
- [ ] P2 — Abort conditions defined (automatic rollback on threshold violation)
</area>

</phase>

<phase id="2" name="AI/ML Core" note="Mark as N/A if product has no AI/ML components">

<area id="2.1" name="Model Strategy">
- [ ] P0 — Model choice with rationale (accuracy vs. latency vs. cost)
- [ ] P0 — Fallback model defined
- [ ] P1 — Model evaluation matrix (at least 2-3 models compared)
- [ ] P1 — Multi-model strategy (routing by complexity: cheap → mid → premium)
- [ ] P1 — Vendor lock-in risk assessed with mitigation (abstraction layer)
- [ ] P2 — Model cascading implemented (simple queries → cheap models)
- [ ] P2 — Open-source vs. proprietary trade-off documented
</area>

<area id="2.2" name="RAG and Knowledge Architecture">
- [ ] P0 — RAG approach specified (naive/advanced/graph/agentic) with rationale
- [ ] P0 — Chunking strategy defined (target: 300-500 tokens)
- [ ] P1 — Hybrid search (vector + BM25) planned
- [ ] P1 — Reranking step planned (e.g. cross-encoder)
- [ ] P1 — Vector DB chosen with rationale
- [ ] P2 — Query transformation (e.g. HyDE) for ambiguous queries
- [ ] P2 — Parent document retrieval (small chunks match, large for context)
- [ ] P2 — Retrieval latency target defined (&lt;50ms)
</area>

<area id="2.3" name="Prompt Engineering">
- [ ] P0 — System prompt versioned (semantic versioning)
- [ ] P1 — Prompt architecture documented (strategy, context injection, output format)
- [ ] P1 — Prompts in dedicated directory, not hardcoded
- [ ] P1 — Temperature and max tokens chosen with rationale
- [ ] P2 — Prompt versioning convention (major/minor/patch) defined
- [ ] P2 — Prompts as immutable artifacts with audit trail
- [ ] P2 — Prompt changes decoupled from code deployments
</area>

<area id="2.4" name="Agent and Orchestration">
- [ ] P1 — Decision single agent vs. multi-agent documented
- [ ] P1 — Orchestration framework chosen (LangGraph, CrewAI, etc.)
- [ ] P1 — State management with checkpointing specified
- [ ] P2 — Tool integration via schema-based function calls
- [ ] P2 — Supervisor pattern for multi-agent scenarios
</area>

<area id="2.5" name="Evaluation and Testing">
- [ ] P0 — Evaluation framework defined (RAGAS, DeepEval, Promptfoo)
- [ ] P0 — Golden dataset available (min 10-20 high-priority examples)
- [ ] P1 — Automated eval integrated in CI/CD pipeline
- [ ] P1 — Quality gate with pass rate threshold (e.g. ≥80%)
- [ ] P1 — Four-layer test architecture (unit, integration, evaluation, shadow)
- [ ] P2 — LLM-as-Judge for non-deterministic outputs
- [ ] P2 — Probabilistic pass rates (N runs, X% success)
- [ ] P2 — Shadow testing (new prompt parallel to production)
</area>

</phase>

<phase id="3" name="UX and AI Interaction Design">

<area id="3.1" name="AI Design Principles" note="For non-AI: evaluate general UX principles">
- [ ] P0 — AI outputs framed as suggestions, not facts
- [ ] P0 — Confidence signaling on AI outputs (scores, sources, reasoning)
- [ ] P0 — User can edit/reject/regenerate AI results
- [ ] P1 — Draft-based interfaces instead of black-box automation
- [ ] P1 — Tool-like language ("Analysis suggests") not anthropomorphization ("I think")
- [ ] P2 — Collapsible reasoning panels for explainability
- [ ] P2 — Version history for AI-generated content
</area>

<area id="3.2" name="Progressive Disclosure">
- [ ] P0 — Simple features first, advanced on demand
- [ ] P1 — Maximum 2-3 disclosure levels
- [ ] P1 — Clear indication how to access more options
- [ ] P2 — Tailoring by user segment (experts see more)
</area>

<area id="3.3" name="Trust Calibration">
- [ ] P0 — Explicit communication of capabilities and limitations
- [ ] P1 — Performance statistics shown before first interaction
- [ ] P1 — Confidence percentages instead of vague hedging
- [ ] P2 — Calibration in early phases (first impressions form persistent trust)
</area>

<area id="3.4" name="Graceful Degradation">
- [ ] P0 — Functioning base features when AI services fail
- [ ] P0 — Plain-text error messages instead of technical codes
- [ ] P1 — 2-3 recovery options (retry, wait, alternative)
- [ ] P1 — User context preserved across failures
- [ ] P1 — Confirmation that user work is saved
- [ ] P2 — Warm colors (amber) instead of aggressive red for AI errors
</area>

<area id="3.5" name="Onboarding and Time-to-Value">
- [ ] P0 — Activation metric defined (first value moment)
- [ ] P0 — Onboarding flow for AI features present
- [ ] P1 — 3-phase onboarding (60s orientation, 1-5min activation, 5min-7d reinforcement)
- [ ] P1 — Personalized onboarding flow by role/segment
- [ ] P2 — "Aha moment" identified and optimized
- [ ] P2 — Three-step tours (72% completion) instead of long flows (16%)
</area>

</phase>

<phase id="4" name="Safety, Guardrails and Compliance" note="For non-AI: only 4.5-4.7 apply">

<area id="4.1" name="Input Guardrails" note="N/A if non-AI">
- [ ] P0 — Prompt injection detection implemented (OWASP #1 risk)
- [ ] P0 — PII detection and redaction before LLM submission
- [ ] P1 — Content filtering (toxic/harmful)
- [ ] P1 — Input length validation
- [ ] P1 — Topic boundaries (allowed/blocked topics)
- [ ] P2 — Strict separation of untrusted content and system instructions
</area>

<area id="4.2" name="Output Guardrails" note="N/A if non-AI">
- [ ] P0 — Toxicity/harm filtering on outputs
- [ ] P0 — PII leakage detection on outputs
- [ ] P1 — Factual grounding check (RAG cross-reference)
- [ ] P1 — Format validation (JSON schema enforcement)
- [ ] P1 — Confidence thresholding (suppress below threshold)
- [ ] P2 — Guardrail bypass rate target defined (&lt;X%)
</area>

<area id="4.3" name="Human-in-the-Loop" note="N/A if non-AI">
- [ ] P0 — HITL pattern defined (interrupt/resume, confidence-routing, etc.)
- [ ] P0 — Escalation rules specified (confidence &lt; threshold, sensitive topics)
- [ ] P1 — Tiered thresholds (>90% auto-approve, 70-90% spot-check, &lt;70% review)
- [ ] P1 — Audit trail for all HITL decisions
- [ ] P2 — Progressive automation (crawl → walk → run) planned
- [ ] P2 — PoLA (Principle of Least Autonomy) at first deployment
</area>

<area id="4.4" name="Hallucination Management" note="N/A if non-AI">
- [ ] P0 — Hallucination strategy defined (RAG, citations, confidence)
- [ ] P0 — Maximum acceptable hallucination rate set
- [ ] P1 — Source citations for factual claims
- [ ] P1 — "I don't know" capability (abstention training)
- [ ] P1 — User feedback mechanism (thumbs up/down + structured correction)
- [ ] P2 — Multi-strategy: RAG + CoT + self-consistency + low temperature
</area>

<area id="4.5" name="EU AI Act Compliance">
- [ ] P0 — Risk category classified (minimal/limited/high/unacceptable)
- [ ] P0 — AI literacy: team trained on AI capabilities and limitations (since Feb 2025)
- [ ] P0 — Transparency: users informed they interact with AI (since Feb 2025)
- [ ] P1 — GPAI model obligations met (tech docs, training data summary — since Aug 2025)
- [ ] P1 — Timeline for high-risk requirements (from Aug 2026) documented
- [ ] P2 — Risk management system established (for high-risk)
- [ ] P2 — Conformity assessment planned (for high-risk)
</area>

<area id="4.6" name="GDPR / Data Protection">
- [ ] P0 — Legal basis for data processing defined
- [ ] P0 — Data processing agreements with AI providers (Art. 28)
- [ ] P1 — DPIA conducted or planned (Art. 35)
- [ ] P1 — Data minimization verified
- [ ] P1 — Right to explanation for automated decisions (Art. 22)
- [ ] P1 — Privacy policy includes AI processing
- [ ] P2 — AI-specific DPA clauses (no training on customer data, subprocessor chain)
</area>

<area id="4.7" name="Responsible AI">
- [ ] P1 — Fairness: bias detection methodology defined
- [ ] P1 — Transparency: AI outputs labeled as AI-generated
- [ ] P1 — Accountability: clear ownership for AI outcomes
- [ ] P2 — Red-teaming session planned (before beta / quarterly)
- [ ] P2 — Adversarial test suite available
</area>

</phase>

<phase id="5" name="Architecture and Infrastructure">

<area id="5.1" name="System Architecture">
- [ ] P0 — Architecture decision documented (monolith/modular monolith/microservices)
- [ ] P0 — AI inference as separate service planned (if AI product)
- [ ] P1 — Event-driven architecture for async AI workflows
- [ ] P1 — Multi-tenant design specified
- [ ] P2 — Modular monolith + separate inference microservice (recommended start)
</area>

<area id="5.2" name="AI Gateway" note="N/A if non-AI">
- [ ] P1 — AI gateway planned (unified API, routing, failover, observability)
- [ ] P1 — Model fallback chain defined (primary → secondary → tertiary)
- [ ] P2 — Intelligent routing (simple queries → cheap models)
- [ ] P2 — PII masking and prompt injection detection in gateway
</area>

<area id="5.3" name="Streaming and Latency">
- [ ] P0 — Streaming architecture defined (SSE recommended)
- [ ] P1 — TTFT target defined (&lt;250ms consumer, &lt;500ms enterprise)
- [ ] P1 — Backpressure handling for UI updates
- [ ] P2 — Latency budgets for all endpoints (P50/P95/P99)
</area>

<area id="5.4" name="Caching and Cost Optimization">
- [ ] P1 — Provider-level prefix caching activated
- [ ] P1 — AI cost per request/feature/user tracked
- [ ] P2 — Semantic caching for recurring queries
- [ ] P2 — Token-based rate limiting (not just request-based)
- [ ] P2 — Cost-based throttling mapped to dollar costs
</area>

<area id="5.5" name="Error Handling">
- [ ] P0 — Production error pipeline (circuit breaker → degradation → fallback → retry)
- [ ] P0 — Never expose raw API errors to users
- [ ] P1 — Exponential backoff with jitter for retries
- [ ] P1 — Circuit breaker after threshold failures
- [ ] P1 — Graceful degradation priorities (cache → fallback → rules → canned)
</area>

<area id="5.6" name="Monitoring and Observability">
- [ ] P0 — Three pillars: metrics, logs, traces
- [ ] P0 — AI-specific monitoring (inference latency, token usage, confidence distribution)
- [ ] P1 — Alerting by severity tiers (P0-P3)
- [ ] P1 — Cost tracking with budget alerts
- [ ] P1 — Drift detection (PSI, KL divergence)
- [ ] P2 — Quality dashboards with automated alerts
- [ ] P2 — Data flywheel: feedback → training → improvement loop
</area>

<area id="5.7" name="CI/CD and DevOps">
- [ ] P1 — Prompt evaluation in CI/CD pipeline (Promptfoo/DeepEval)
- [ ] P1 — Security scan for prompt vulnerabilities
- [ ] P1 — Quality gate blocks merge on metric degradation
- [ ] P2 — Canary deployments for prompt/model rollouts
- [ ] P2 — GitOps-based prompt management
- [ ] P2 — Branching: prompt/* branches separate from code
</area>

<area id="5.8" name="Security">
- [ ] P0 — Encryption at rest (AES-256) and in transit (TLS 1.3)
- [ ] P0 — Audit logging for auth, admin, data access, AI interactions
- [ ] P1 — RBAC with defined roles and permission matrix
- [ ] P1 — Vulnerability scanning (SAST + DAST) in CI/CD
- [ ] P1 — SOC-2 roadmap defined (60%+ enterprise customers require it)
- [ ] P2 — Penetration testing planned (annually / before major releases)
</area>

</phase>

<phase id="6" name="Go-to-Market and Operations">

<area id="6.1" name="Rollout Strategy">
- [ ] P0 — Phased rollout defined (alpha → beta → GA)
- [ ] P0 — Feature flags for kill switch and rollout control
- [ ] P1 — Rollback triggers and SLA defined (detect+decide+disable ≤10min)
- [ ] P1 — A/B test: control (without AI) vs. treatment (with AI)
- [ ] P2 — Model validation phase (offline eval before alpha)
- [ ] P2 — Communication plan per phase (internal + external)
</area>

<area id="6.2" name="Feedback and Continuous Improvement">
- [ ] P0 — Explicit feedback (thumbs up/down, corrections) implemented
- [ ] P1 — Implicit signals tracked (acceptance, abandonment, edit distance)
- [ ] P1 — Improvement cadence defined (weekly eval, monthly review, quarterly audit)
- [ ] P2 — Data flywheel documented (feedback → training → improvement)
- [ ] P2 — Retraining triggers defined (accuracy drop, drift, new data)
</area>

<area id="6.3" name="Retention and Customer Success">
- [ ] P1 — NRR target defined (>100% for growth)
- [ ] P1 — Churn monitoring and warning signals
- [ ] P1 — Activation rate tracked as leading indicator
- [ ] P2 — Health score with 6-12 data points
- [ ] P2 — Proactive intervention on churn signals
- [ ] P2 — Expansion trigger at 80% of tier limits
</area>

<area id="6.4" name="SaaS Metrics and Anti-Pattern Detection">
- [ ] P0 — Core metrics defined (MRR, churn, NRR, LTV:CAC)
- [ ] P1 — Red-flag thresholds defined (churn >5%, NRR &lt;90%, LTV:CAC &lt;1:1)
- [ ] P1 — Weekly pulse check (DAU/WAU, signups, activation, feature usage)
- [ ] P1 — Premature scaling guard: PMF proven before scaling sales/marketing
- [ ] P1 — Feature bloat guard: prioritization framework (RICE/MoSCoW)
- [ ] P2 — Leaky bucket check: retention curve stabilizes (not falling to zero)
- [ ] P2 — Rule of 40 and burn multiple tracked

For current metric benchmarks, see `metrics-2026.md`.
</area>

</phase>

<phase id="7" name="Payment and Revenue Infrastructure">

<area id="7.1" name="Payment Integration">
- [ ] P0 — Payment provider active in live mode (not test keys)
- [ ] P0 — Subscription lifecycle working (create, upgrade, downgrade, cancel)
- [ ] P0 — Webhook endpoints configured and verified
- [ ] P1 — Customer billing portal accessible
- [ ] P1 — Failed payment retry and dunning flow configured
- [ ] P1 — Tax compliance configured (Stripe Tax or equivalent)
- [ ] P1 — Fraud prevention rules active (Radar or equivalent)
- [ ] P2 — Statement descriptor set and verified
- [ ] P2 — Proration settings configured correctly
</area>

<area id="7.2" name="AI Economics and Margin Protection" note="N/A if non-AI">
- [ ] P0 — AI cost per interaction calculated
- [ ] P0 — Gross margin target defined (benchmark: >50% for AI-first SaaS)
- [ ] P1 — Usage caps / token limits by tier implemented
- [ ] P1 — Cost monitoring dashboard with alerts
- [ ] P1 — Per-user AI cost tracking active
- [ ] P2 — Dynamic pricing adjustment mechanism
- [ ] P2 — Model efficiency optimization roadmap

For current AI economics benchmarks, see `metrics-2026.md`.
</area>

</phase>

<phase id="8" name="Discovery, Visibility and Launch Readiness">

<area id="8.1" name="GEO and AI Discovery">
- [ ] P0 — Website content server-side rendered (SSR) or pre-rendered
- [ ] P0 — Structured data (schema.org) implemented
- [ ] P1 — Core Web Vitals pass (LCP &lt;2.5s, INP &lt;200ms, CLS &lt;0.1)
- [ ] P1 — Authoritative content: answer-first structure, minimal hedging
- [ ] P1 — FAQ pages with natural question phrasing
- [ ] P2 — Comparison pages ("[Product] vs [Competitor]")
- [ ] P2 — Category pages ("Best [category] tools")

For current AI visibility data, see `metrics-2026.md` (GEO section).
</area>

<area id="8.2" name="Competitor Intelligence">
- [ ] P1 — AI visibility audit conducted (queries across ChatGPT, Perplexity, Claude)
- [ ] P1 — Product mentioned in AI discovery results
- [ ] P1 — Citation accuracy verified (features, pricing)
- [ ] P2 — Multi-platform optimization (presence across all 3 major AI platforms)
- [ ] P2 — Sentiment analysis of AI citations

Use `scripts/competitor-visibility.sh` for query generation.
</area>

<area id="8.3" name="Social Media and Launch Strategy">
- [ ] P0 — Launch channels identified by product type
- [ ] P1 — Product Hunt launch prepared (if applicable): assets, supporter strategy
- [ ] P1 — Founder content strategy defined (LinkedIn, X, or relevant platforms)
- [ ] P1 — Community engagement plan (Reddit, HN, Indie Hackers as appropriate)
- [ ] P2 — Email launch sequence prepared (teaser → preview → launch → follow-up)
- [ ] P2 — Retargeting pixels installed

For current platform guides and tactics, see `platform-guides-2026.md`.
</area>

<area id="8.4" name="Legal and Trust Pages">
- [ ] P0 — Privacy policy published and accessible
- [ ] P0 — Terms of service published
- [ ] P1 — Cookie consent mechanism (GDPR-compliant)
- [ ] P1 — Imprint / legal notice (required in EU)
- [ ] P1 — Security / trust page with practices overview
- [ ] P2 — Status page with uptime history
- [ ] P2 — DPA template ready for enterprise customers
</area>

</phase>

<scoring_summary>

| Phase | Areas | Max Score |
|-------|-------|-----------|
| 1. Strategic Foundation | 3 | 9 |
| 2. AI/ML Core | 5 | 15 |
| 3. UX and AI Design | 5 | 15 |
| 4. Safety and Compliance | 7 | 21 |
| 5. Architecture and Infra | 8 | 24 |
| 6. GTM and Operations | 4 | 12 |
| 7. Payment and Revenue | 2 | 6 |
| 8. Discovery and Launch | 4 | 12 |
| **Total** | **38** | **114** |

**Maturity levels** (based on total score):

| Score Range | Maturity | Interpretation |
|-------------|----------|----------------|
| 0-28 (0-25%) | 🔴 Beginner | Fundamental gaps, not production-ready |
| 29-57 (26-50%) | 🟠 Developing | Basics present, significant catch-up needed |
| 58-85 (51-75%) | 🟡 Advanced | Solid foundation, optimization potential in details |
| 86-114 (76-100%) | 🟢 Gold Standard | Best practices comprehensively implemented |

**Non-AI SaaS scoring**: Exclude N/A areas (Phase 2 fully, 4.1-4.4, 5.2, 7.2).
Remaining: ~24 areas, max 72 points. Recalculate percentage accordingly.

</scoring_summary>
