<!-- Research: Q1/2026 — Review and update by Q1/2027 -->
<!-- Sources: a16z, Sacks, Rachitsky, McKinsey, industry benchmarks -->

<stability_notice>
**Research date:** Q1/2026. **Review by:** Q1/2027.
If the current date is past Q1/2027, validate key benchmarks (especially gross margins,
IER targets, GEO visibility stats) via web search before using in audit reports.
Flag to user: "Metric benchmarks are from Q1/2026 and may need updating."
</stability_notice>

<overview>
Time-sensitive SaaS and AI-first metrics, benchmarks, and market data.
Includes AI economics (IER, margins), GEO/AI visibility statistics,
and quantitative performance benchmarks. Last research: Q1/2026.
</overview>

<quantitative_benchmarks research_date="Q1/2026">

| Metric | Danger | Acceptable | Good | Excellent |
|--------|--------|-----------|------|-----------|
| Sean-Ellis | &lt;25% | 25-35% | 40-50% | >50% |
| NPS | &lt;0 | 0-36 | 36-50 | >50 |
| Monthly Churn (B2B) | >5% | 3-5% | 1-3% | &lt;1% |
| NRR | &lt;90% | 90-100% | 100-110% | >120% |
| LTV:CAC | &lt;1:1 | 1:1-3:1 | 3:1-5:1 | >5:1 |
| CAC Payback | >24 mo | 12-18 mo | 6-12 mo | &lt;6 mo |
| DAU/MAU | &lt;10% | 10-20% | 25-40% | >50% |
| Gross Margin | &lt;70% | 70-75% | 75-80% | >85% |
| Rule of 40 | &lt;20% | 20-30% | 30-40% | >40% |
| Burn Multiple | >3x | 2-3x | 1-2x | &lt;1x |

**NRR benchmarks by segment (2024-2025 data):**

| Segment | Median NRR | Median GRR |
|---------|-----------|-----------|
| Enterprise | 118% | >95% |
| Mid-Market | 108% | ~92% |
| SMB | 97% | ~85% |

**Red flags (immediate action required):**
- Monthly churn >5% (46% annual loss)
- NRR &lt;90% (active revenue contraction)
- GRR &lt;85% (product base eroding)
- LTV:CAC &lt;1:1 (value destruction)
- CAC payback >24 months
- Burn multiple >3x
- Trial conversion &lt;5%
- Retention curve falling to zero (no PMF)
</quantitative_benchmarks>

<ai_first_metrics research_date="Q1/2026">

**Gross margin benchmarks:**

| Company Type | Gross Margin | Notes |
|-------------|-------------|-------|
| Traditional B2B SaaS | 80-90% | Long-standing benchmark |
| AI-First B2B SaaS | 50-65% | Due to inference costs |
| Unoptimized AI | ~25% | Experimental pricing |
| Optimized AI | ~60% | Custom models, refined pricing |

**Key statistics:**
- 84% of AI companies see 6%+ gross margin erosion from AI infrastructure
- Compute costs for AI apps run 1-3x software hosting costs
- Custom fine-tuned models deliver 50-70% cost reduction at scale
- LLM costs fall ~10x annually ("LLMflation"): GPT-4-level $20/MTok (2022) → $0.40/MTok (2025)

**2026 reality check:** Costs will not fall as fast as hoped. Cheaper tokens
are being offset by heavier use of reasoning models and rising maintenance.

**Inference Efficiency Ratio (IER)** — proposed internal metric:
```
IER = Revenue per User / Inference Cost per User
```

| IER | Assessment |
|-----|------------|
| &lt;4:1 | Unsustainable — rethink model |
| 4:1-8:1 | Marginal — optimize aggressively |
| 8:1-15:1 | Healthy — standard target |
| >15:1 | Excellent — room for feature expansion |

Target: AI costs &lt;30% of revenue (equivalent to ~3.3:1+ IER).

**AI-adjusted metrics:**
- AI-Adjusted LTV = Traditional LTV - (Cumulative Inference Cost over Lifetime)
- AI-Adjusted CAC = Traditional CAC + (Onboarding AI Costs)
- AI-Adjusted Payback = CAC / (Monthly Revenue - Monthly AI Cost)

**Cost optimization thresholds:**

| Monthly Spend | Recommended Strategy |
|---------------|---------------------|
| &lt;$50K | Stay with API providers |
| $50K-$200K | Implement intelligent routing |
| >$200K | Evaluate custom model development |
</ai_first_metrics>

<geo_ai_visibility research_date="Q1/2026">

**Market impact (verified 2026 data):**
- AI agents now account for ~33% of organic search activity (and climbing)
- ChatGPT refers around 10% of new Vercel signups (up from 1% six months ago)
- Only 12% of companies appear across all three platforms (ChatGPT, Perplexity, Claude)
- Citation patterns vary by up to 300% across platforms
- Multi-platform optimization drives 3.2x more AI-sourced leads
- 30:1 ratio between discovery queries and direct queries in AI assistants

**Platform-specific content length (verified):**

| Platform | Optimal Content Length | Notes |
|----------|----------------------|-------|
| ChatGPT | 2,000+ words | Favors comprehensive guides |
| Claude | 1,500-2,500 words | Prefers balanced analysis, strong hierarchy |
| Perplexity | Quality over length | Searches web, cites every source |
</geo_ai_visibility>

<nfr_benchmarks research_date="Q1/2026">

**Performance:**

| Metric | Target | Maximum |
|--------|--------|---------|
| LCP Landing Page | ≤1.5s | ≤2.5s |
| Dashboard Load | ≤3.0s | ≤5.0s |
| API Read P95 | ≤200ms | ≤500ms |
| API Write P95 | ≤500ms | ≤1000ms |
| AI TTFT | ≤500ms | ≤1000ms |
| AI Throughput | ≥40 tok/s | ≥20 tok/s |

**Availability:**

| Tier | SLA | Downtime/Month |
|------|-----|---------------|
| Internal SLO | 99.95% | 21.9 min |
| Public SLA | 99.9% | 43.8 min |
| AI Features SLO | 99.5% | 3.65 h |

**UX statistics:**
- 71% of users expect personalized experiences
- Streaming perceived as 40% faster (even at identical total time)
- WCAG 2.2 accessibility = 35% greater reach
- Bad UX causes 70% churn
- UX investments yield up to 9,900% ROI
</nfr_benchmarks>
