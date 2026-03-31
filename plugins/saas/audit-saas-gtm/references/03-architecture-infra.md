<overview>
Stable reference for architecture decisions, AI gateway patterns, streaming,
caching, error handling, monitoring, CI/CD, and security. Includes trust
infrastructure (auth, logging, uptime). Tool-specific version numbers may
need periodic review.
</overview>

<architecture_decision>

**Recommended: Modular Monolith + Inference Microservice**

| Variant | Suitability | Trade-off |
|---------|------------|-----------|
| Monolith | MVP, &lt;10 devs | AI inference not independently scalable |
| Microservices | Large teams, independent scaling | Enormous operational complexity |
| **Modular Monolith** | **Recommended start** | 43% faster feature delivery, 22% lower infra costs |

**Decision rule:** Modular monolith for application logic + separate microservice for
AI inference (GPU-bound, latency-sensitive, different scaling). Extract services only
when bottlenecks are proven.

**Event-driven architecture for AI workloads:**
- LLM inference takes seconds to minutes — EDA keeps apps responsive
- AI workloads are bursty — EDA scales horizontally via event queues
- On model failure: event-driven retry, redirect, degrade
</architecture_decision>

<ai_gateway>

**Functions:** Unified API for all models, intelligent routing, automatic failover,
observability, security (PII masking, prompt injection detection), semantic caching,
streaming support.

**Recommendation:** Introduce immediately once LLM usage goes beyond experiments.

**Streaming: SSE as standard**
- De facto standard: unidirectional, HTTP-based, browser reconnection
- Users perceive streaming as 40% faster at identical total time
- WebSockets only for bidirectional (collaborative editors, voice)
- TTFT target: &lt;250ms consumer apps
</ai_gateway>

<caching_strategies>

**Tier 1: Provider-level prefix caching**
- 90% cost savings, 85% latency reduction (Anthropic cache reads)
- Implementation: static content (system prompts, few-shot) at start, dynamic at end

**Tier 2: Semantic caching**
- 31% of LLM queries are semantically similar
- 61-69% hit rate, >97% accuracy
- Ideal for support, FAQ bots, search
- Not for highly personalized or creative tasks

**Combined: >80% savings possible**
</caching_strategies>

<cost_optimization>

**Order by ROI:**
1. Quick wins (15-40%): Prompt optimization + provider caching
2. Medium term (+20-40%): Semantic caching + model cascading
3. Advanced (+30-60%): Quantization, custom engines, hardware

**Rate limiting for AI-SaaS:**
- Token-based, not just request-based
- Hierarchical: user → agent → task level
- Cost-based throttling mapped to dollar costs
- Always expose headers: X-RateLimit-Remaining, Retry-After
</cost_optimization>

<error_handling>

**Production pipeline:**
```
App → Circuit Breaker → Graceful Degradation → Model Fallback → Retry + Backoff → LLM API
```

- Exponential backoff with jitter, 2x extra for 429 (rate limit), max 3 retries/model
- Model fallback chain: primary → secondary → tertiary (cross-provider for resilience)
- Graceful degradation priority: cache → fallback model → rules → canned responses
- Never expose raw API errors to users
</error_handling>

<monitoring>

**Three pillars:** Metrics (Prometheus/Datadog), Logs (ELK/structured JSON), Traces (OpenTelemetry)

**AI-specific monitoring:**
Inference latency (TTFT, tokens/sec, E2E), token usage and cost per request/feature/user,
confidence distribution, guardrail trigger rates, drift detection (PSI, KL divergence),
user feedback sentiment trends.

**Alerting tiers:**
| Severity | Response | Example |
|----------|----------|---------|
| P0 Critical | &lt;15 min | Service down, data loss |
| P1 High | &lt;1 hour | Degraded performance, accuracy drop |
| P2 Medium | &lt;4 hours | Approaching capacity |
| P3 Low | Next business day | Unusual patterns |
</monitoring>

<cicd>

**Pipeline with LLM evaluation:**
PR → Code Quality → Prompt Evaluation → Security Scan → LLM Regression →
Quality Gate (fail if &lt;80%) → Human Review → Merge → Build → Canary → Monitor → Promote/Rollback

**Golden dataset:** Start with 10-20 high-priority examples, expand from production issues.
Temperature=0 for factual tests, semantic similarity for style.

**Branching:** main (production), feature/* (development), prompt/* (separate eval pipelines),
experiment/* (model swaps, hyperparameters).
</cicd>

<trust_infrastructure>

**Authentication checklist:**
- OAuth 2.0/OIDC (not custom auth), MFA, secure session management
- NIST password requirements, rate limiting on auth endpoints
- Account lockout, secure password reset
- SSO for enterprise, API key management, WebAuthn/passkey support

**Audit logging — what to log:**
- Auth events (login, password changes, MFA)
- Data access (reads on sensitive data, exports, API access)
- System events (config changes, permission modifications, billing)
- Format: timestamp (UTC ISO 8601), actor, action, resource, context, outcome
- Storage: immutable, tamper-evident, 1-7 year retention

**Security headers (target A+ on securityheaders.com):**
```
Strict-Transport-Security: max-age=31536000; includeSubDomains; preload
X-Content-Type-Options: nosniff
X-Frame-Options: DENY
Content-Security-Policy: default-src 'self'; ...
Permissions-Policy: geolocation=(), microphone=(), camera=()
```

**Data protection:**
- Encryption at rest (DB, files, backups) + in transit (TLS 1.2+, HSTS)
- Automated daily backups, point-in-time recovery, geographic redundancy
- Regular restore testing
</trust_infrastructure>
