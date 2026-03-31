<overview>
Stable reference for GEO/AI discovery methodology, competitor intelligence
processes, and launch readiness frameworks. Time-sensitive platform guides,
AI visibility data, and launch tactics are in `platform-guides-2026.md`
and `metrics-2026.md`.
</overview>

<geo_ai_discovery>

**Technical optimizations (stable methodology):**

**Server-side rendering (SSR):**
- AI crawlers may not execute JavaScript — ensure content in initial HTML
- Use Next.js with SSR/SSG or equivalent
- Pre-render critical pages
- Test with JavaScript disabled
- Check `view-source:` shows content
- Server response time &lt;200ms (critical for LLM crawler access)

**Structured data (schema.org):**
```json
{
  "@context": "https://schema.org",
  "@type": "SoftwareApplication",
  "name": "Your Product",
  "applicationCategory": "BusinessApplication",
  "operatingSystem": "Web",
  "offers": {
    "@type": "Offer",
    "price": "49",
    "priceCurrency": "USD"
  }
}
```
Also implement: FAQPage schema, Organization schema, Product schema.

**Core Web Vitals targets:**
| Metric | Good | Needs Improvement | Poor |
|--------|------|-------------------|------|
| LCP | &lt;2.5s | 2.5s-4.0s | >4.0s |
| INP | &lt;200ms | 200ms-500ms | >500ms |
| CLS | &lt;0.1 | 0.1-0.25 | >0.25 |
</geo_ai_discovery>

<content_optimization>

**Authoritative language:**
AI systems favor confident, specific content.
- Avoid: "might be", "could potentially", "may help"
- Prefer: Direct statements with evidence, confident tone

**Answer-first structure:**
1. Lead with the answer (first sentence answers the question)
2. Expand with context (supporting details follow)
3. Provide evidence (data, examples, case studies)
4. Include alternatives (acknowledge other options)

**FAQ optimization:**
- Natural question phrasing
- Complete, standalone answers
- Cover comparison queries ("[Product] vs [Competitor]")
- Address pricing and feature questions
</content_optimization>

<ai_visibility_audit_process>

**Step 1: Query AI assistants**
Test these patterns across ChatGPT, Perplexity, Claude:
- "[category] tools"
- "best [category] for [use case]"
- "[your product] vs [competitor]"
- "[category] alternatives to [leader]"

**Step 2: Analyze results**
For each query: Is product mentioned? Position? Description accuracy? Competitors cited?

**Step 3: Citation sentiment**
Review how AI describes your product: positive/neutral/negative framing,
accurate features, correct pricing, any outdated information.

Use `scripts/competitor-visibility.sh` for automated query generation.
</ai_visibility_audit_process>

<improving_visibility>

**Content strategy:**
1. Comparison pages — "[Product] vs [Competitor]"
2. Category pages — "Best [category] tools"
3. Use case pages — "[Product] for [specific use case]"
4. Integration pages — "[Product] + [popular tool] integration"

**Authority building:**
- Get mentioned in industry publications
- Contribute to relevant communities
- Build quality backlinks
- Maintain active social presence

**Freshness signals:**
- Regular blog posts
- Updated pricing/feature pages
- Recent customer testimonials
- Current year references
</improving_visibility>

<launch_frameworks>

**The "Magic Triangle" strategy:**
1. **Founder content**: Personal stories, behind-the-scenes, authentic not polished
2. **Outreach**: Direct DMs, community engagement, podcast appearances, guest posts
3. **Retargeting**: Capture visitors, sequential messaging, coordinate with content

**Email launch sequence:**
- T-7: Teaser (hint, why it matters, gauge interest)
- T-2: Preview (what, benefits, early access offer, waitlist CTA)
- T-0: Launch day (direct link, value prop, social proof, limited offer)
- T+2: Follow-up (thank supporters, share traction, address questions)

**Launch metrics to track:**
- Day 1: Signups, traffic sources, social mentions, PH ranking
- Week 1: Activation rate, first paying customers, feedback themes, churn signals
- Month 1: MRR trajectory, CAC, retention indicators, word-of-mouth
</launch_frameworks>

<legal_trust_pages>

**Required pages:**
- Privacy policy (GDPR-compliant, mentions AI processing if applicable)
- Terms of service
- Cookie consent mechanism
- Imprint / legal notice (required in EU)

**Recommended pages:**
- Security / trust page: practices overview, certifications, data handling
- Status page: real-time indicators, incident history, uptime metrics
- DPA template for enterprise customers

**Enterprise readiness:**
Be prepared for: security questionnaires, vendor assessments, custom DPA requests,
SSO/SCIM requirements, data residency questions.
</legal_trust_pages>
