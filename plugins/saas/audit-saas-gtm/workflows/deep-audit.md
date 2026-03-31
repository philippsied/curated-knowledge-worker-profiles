<required_reading>
Load these references before starting:
1. `references/audit-checklist.md` — Always
2. `references/example-audit.md` — Always (quality anchor)
3. Phase-specific references — Load per phase as you progress
4. Time-sensitive references (`*-2026.md`) — Load when evaluating pricing, metrics, or launch tactics

Before proceeding, verify that `references/audit-checklist.md` and
`templates/audit-report.md` are accessible. If any required file is missing,
inform the user and halt — do not attempt a partial audit.
</required_reading>

<process>

<step number="1" name="Read and understand the specification">
Read the provided specification completely. Build an internal understanding:

- What is the product? Who are the target users?
- What AI/ML capabilities are described (if any)?
- Which sections exist, which are missing?
- What maturity level is recognizable?

**Edge cases to detect before proceeding:**

**No AI/ML product?** If the spec contains no AI/ML components, inform the user
that AI-specific phases are not applicable. Mark Phase 2 fully, areas 4.1-4.4,
5.2, and 7.2 as "N/A". Offer to audit the general SaaS areas. Score on ~24
areas (max 72) and calculate maturity percentage proportionally.

**Incomplete spec?** If the spec is only 1-2 pages and clearly an early-stage
document, recommend Quick Check mode and note that many 🔴 0 ratings are due
to the document's maturity, not necessarily the product's maturity.

**Multi-product spec?** If multiple products/features are described, clarify
with the user which product/feature should be audited.

**Domain sensitivity?** Identify if the product operates in a high-risk domain
(legal, healthcare, finance, HR, education, biometrics). Flag elevated
compliance requirements proactively — e.g., legal document analysis implies
potential EU AI Act high-risk classification.
</step>

<step number="2" name="Phase-by-phase audit">
For each of the 8 phases:

1. Load the corresponding reference file (see reference_index in SKILL.md)
2. Check the specification against the checklist items for this phase
3. Score each area using the scoring schema from essential_principles
4. Document gaps and derive action recommendations

**Area scoring logic:**
- Are all P0 items fulfilled? If no → maximum 🟠 1
- Are all P0 + majority P1 items fulfilled? → at least 🟡 2
- Are P0 + P1 + majority P2 fulfilled? → 🟢 3
- Is nothing addressed? → 🔴 0

**Phase score**: Arithmetic mean of all area scores in that phase.
Example Phase 2 (5 areas): (3 + 3 + 2 + 2 + 0) / 5 = 2.0

Work through phases sequentially: 1 → 2 → 3 → 4 → 5 → 6 → 7 → 8.

For time-sensitive evaluations (pricing models, market data, AI visibility stats),
load the corresponding `-2026` reference files and apply current benchmarks.
</step>

<step number="3" name="Create audit report">
Create the report using `templates/audit-report.md` as structure.

The report has 4 parts:

**Part 1: Executive Summary**
- Total score (sum of all area scores)
- Maturity rating (Beginner / Developing / Advanced / Gold Standard)
- Top-3 strengths
- Top-3 critical gaps
- Overall assessment in 3-5 sentences

**Part 2: Phase Detail Scores**
For each phase, a table (note: markdown below is the generated report format):
```
## Phase X: [Name]
Phase Score: X.X / 3.0

| Area | Score | Assessment |
|------|-------|------------|
| [Area 1] | 🟢 3 | [Brief rationale] |
| [Area 2] | 🟠 1 | [Brief rationale] |
```

**Part 3: Gap Analysis**
Grouped by criticality:
- 🔴 Critical (score 0): [What is missing] → [What gold standard requires]
- 🟠 High (score 1): [What is incomplete] → [What should be added]
- 🟡 Medium (score 2): [What is solid but improvable] → [Specific suggestion]

**Part 4: Prioritized Action Items**
Ordered by impact × effort:

| # | Action | Phase | Impact | Effort | Priority |
|---|--------|-------|--------|--------|----------|
| 1 | [Concrete action] | [Phase] | High | Low | 🔴 Immediate |

Priority rules:
- 🔴 Immediate: Compliance blockers, security risks, missing core AI specs
- 🟠 Short-term (2-4 weeks): Architecture decisions, metrics definition, HITL design
- 🟡 Mid-term (1-3 months): Optimizations, extended testing strategies
- ⚪ Backlog: Nice-to-haves, long-term improvements
</step>

<step number="4" name="Present results">
Save the audit report as a markdown file (.md) in the outputs directory.

After presenting the report:
1. Ask if the user wants to deep-dive into specific phases
2. Offer to provide concrete text suggestions/additions for critical gaps
3. Offer to create a prioritized task list (e.g. for Todoist, Asana)
</step>

</process>

<success_criteria>
Deep audit is complete when:
- [ ] All 8 phases evaluated (or N/A marked for non-AI)
- [ ] Every area has a score with evidence-based rationale
- [ ] No guessing — missing = 🔴 0
- [ ] Domain sensitivity flagged if applicable
- [ ] Gap analysis covers all 🔴 and 🟠 items
- [ ] Action items are concrete and directly actionable
- [ ] Report saved as markdown file
</success_criteria>
