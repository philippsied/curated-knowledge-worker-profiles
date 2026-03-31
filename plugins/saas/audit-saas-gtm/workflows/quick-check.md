<required_reading>
Load only: `references/audit-checklist.md`
</required_reading>

<process>

<step number="1" name="Read specification and detect edge cases">
Read the provided specification. Identify:
- Is this an AI-SaaS or non-AI SaaS? (determines which P0 items apply)
- What product stage? (adjusts expectations)
- Any domain sensitivity? (elevates compliance P0s)

Apply same edge case detection as deep audit (non-AI, incomplete spec, multi-product).
</step>

<step number="2" name="P0-only evaluation">
Check **only P0 items** from the audit checklist across all 8 phases.

For each P0 item:
- PASS: Item is addressed with sufficient detail
- FAIL: Item is missing or mentioned without substance
- N/A: Not applicable (e.g., AI items for non-AI product)

Count: Total P0 items, passed, failed, N/A.
</step>

<step number="3" name="Create compact report">
Structure:

**Executive Summary** (3-5 sentences)
- Product stage assessment
- P0 pass rate (X/Y passed)
- Overall readiness assessment

**Top-10 Gaps** (most critical P0 failures)
For each:
- What is missing
- Why it matters
- Concrete next step (single actionable instruction)

**Top-5 Priority Actions**
| # | Action | Phase | Why Critical |
|---|--------|-------|-------------|
| 1 | [Action] | [Phase] | [Reason] |

**Offer Deep Audit** as follow-up for comprehensive evaluation.
</step>

</process>

<success_criteria>
Quick check is complete when:
- [ ] All P0 items evaluated as PASS/FAIL/N/A
- [ ] Top-10 gaps identified with concrete next steps
- [ ] Top-5 actions prioritized
- [ ] Report is concise (under 2 pages)
- [ ] Deep audit offered as follow-up
</success_criteria>
