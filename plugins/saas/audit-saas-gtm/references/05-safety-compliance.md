<overview>
Stable reference for safety, guardrails, HITL patterns, compliance (EU AI Act, GDPR),
and responsible AI. Regulatory timelines are included as they are legally fixed dates,
not market estimates.
</overview>

<guardrails>

**Input guards:**

| Guard | Implementation |
|-------|---------------|
| Prompt injection detection | Dedicated classifier + input sanitization |
| PII detection/redaction | NER model / regex / Presidio |
| Content filtering | Toxic/harmful classifier or rule-based |
| Topic boundaries | Allowed/blocked topic lists |
| Input length validation | Max tokens |

**Output guards:**

| Guard | Monitoring |
|-------|-----------|
| Toxicity/harm filtering | Block rate, false positive rate |
| Factual grounding check | Groundedness rate |
| Format validation | Parse failure rate |
| PII leakage detection | PII exposure rate |
| Confidence thresholding | Suppression rate |

**Guardrail testing:**
- Adversarial test suite on every prompt/model change
- Red-teaming before beta / quarterly
- Bypass rate target: &lt;X%
</guardrails>

<hitl_patterns>

**5 patterns:**

| Pattern | Description | Ideal For |
|---------|------------|-----------|
| Interrupt/Resume | Agent pauses, human gives input | Safety-critical decisions |
| Human-as-Tool | Agent sees human as callable tool on uncertainty | Filling knowledge gaps |
| Confidence-Routing | Below threshold → automatically to human | High-volume + occasional edge cases |
| Parallel Feedback | AI does not pause, feedback async | High-throughput workflows |
| Elicitation Middleware | Formalized as tool, traceable, auditable | HITL as first-class citizen |

**Tiered thresholds:**

| Confidence | Action |
|-----------|--------|
| >90% (High) | Auto-approve |
| 70-90% (Medium) | Spot-check |
| &lt;70% (Low) | Mandatory review |

**Circuit breaker:** If error rate exceeds threshold → automatic manual review.

**When HITL is mandatory:**
- Irreversible actions (data deletion, financial transactions)
- Low-confidence outputs
- Ethical/ambiguous situations
- Novel/out-of-distribution inputs
</hitl_patterns>

<progressive_automation>

**Crawl → Walk → Run:**

1. **Crawl (AI as copilot):** AI handles basic requests, human reviews every output.
   Focus: build confidence, establish baseline metrics.
2. **Walk (extended autonomy):** AI deployed to customers, escalation protocols active.
   Continuous learning: patterns, knowledge gaps, feedback loops.
3. **Run (true autonomy):** AI executes independently. Human: strategic oversight + exceptions.
   Only after demonstrated competence on previous levels.

**Principle of Least Autonomy (PoLA):** First deployment should feel
"almost uselessly conservative". Gradual subtask handover contingent on success metrics.

**Readiness indicators:** Declining error rate, declining override rate, stable CSAT, edge case success.
</progressive_automation>

<eu_ai_act>

**Timeline (legally fixed):**

| Date | Obligation |
|------|-----------|
| Feb 2025 | Prohibited practices, AI literacy |
| Aug 2025 | GPAI model obligations |
| Aug 2026 | Full applicability high-risk |

**Penalties:** Up to €35M or 7% global revenue.

**All AI systems (since Feb 2025):**
- AI literacy: team trained
- Transparency: users know they interact with AI
- AI system registered in internal inventory

**GPAI models (since Aug 2025):**
- Technical documentation
- Training data summary
- Copyright compliance
- Complaint mechanism

**High-risk (from Aug 2026):**
- Risk management system
- Data governance
- Human oversight mechanisms
- Accuracy, robustness, cybersecurity
- Logging and record-keeping
- Quality management system
- Conformity assessment

**High-risk domains:** Recruiting, credit scoring, insurance, law enforcement,
education, public services, healthcare, biometrics.
</eu_ai_act>

<gdpr>

**Obligations:**
- DPA per Art. 28 with AI providers (9 mandatory elements)
- DPIA per Art. 35 for high-risk processing
- Purpose limitation and data minimization
- Right to explanation for automated decisions (Art. 22)

**PII handling:**
- Anonymization before LLM submission
- Pseudonymization with fake identifiers
- Right to erasure: context-based processing (RAM flush after request)

**AI-specific DPA clauses:**
- No training on customer data
- Subprocessor chain fully disclosed
- Audit rights for AI-specific compliance
- No "function creep" — data not for unintended AI purposes
</gdpr>

<responsible_ai>

| Principle | Requirement |
|-----------|------------|
| Fairness | Bias detection defined, tests across demographic groups |
| Transparency | Explainability documented, AI outputs labeled as AI-generated |
| Accountability | Clear ownership, escalation paths |
| Safety | Adversarial robustness, red-teaming |
| Privacy | Data minimization, anonymization |
| Human oversight | HITL for high-stakes, override mechanism |
| Environmental | Compute costs estimated, efficiency optimizations |
</responsible_ai>

<security>

**Prompt injection = OWASP #1 risk:**
- Privilege control (least privilege)
- Strict separation of untrusted content / system instructions
- Input and output validation
- HITL for privileged operations
- Regular adversarial penetration testing

**Baseline security:**
- Encryption: AES-256 at rest, TLS 1.3 in transit
- Audit logging: auth, admin, data access, AI interactions
- RBAC with permission matrix
- SAST + DAST in CI/CD
- SOC-2 roadmap (60%+ enterprise customers require it)
</security>
