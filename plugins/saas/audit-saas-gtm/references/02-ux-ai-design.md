<overview>
Stable reference for UX and AI interaction design. Consolidates principles from
Google PAIR, Microsoft HAX, Apple HIG, Anthropic, and Nielsen Norman Group.
</overview>

<five_core_principles>

**1. AI as advantage, not replacement**
- AI delivers drafts, recommendations, analyses — final decision with user
- Draft-based interfaces with edit controls instead of black-box automation
- Accept/Adjust/Reject as core interaction pattern

**2. Confidence signaling is mandatory**
- Every AI output with reliability indicators: confidence scores, sources, reasoning panels
- Interface explicitly signals how certain the system is
- Never present every output as fact

**3. Explain appropriately**
- Too much explanation = noise
- Collapsible reasoning panels or citation links
- Right level depends on user context

**4. Oversight without friction**
- Review/oversight necessary but not as extra work
- Inline review tools, contextual reasoning displays
- Verification as natural workflow part

**5. Normalize variability**
- Frame AI results as suggestions, not facts
- Language and UI set expectations (outputs vary)
- Regeneration controls and version history as core features
</five_core_principles>

<framework_synthesis>

| Principle | Google PAIR | Microsoft HAX | Apple HIG | Anthropic |
|-----------|------------|---------------|-----------|-----------|
| Transparency | 5 patterns | G1, G2, G11 | Clarity | Constitutional AI |
| User control | 5 patterns | G7-G9, G17 | Corrections | Steerable AI |
| Trust calibration | 7 patterns | G2, G10 | Criticality | Safety-first |
| Error handling | 3 patterns | G10, G12 | Mistakes | Self-correction |
| Feedback | ✓ | G15 | Input patterns | RLHF |
| Privacy | ✓ | G6 | Always protect | Data governance |

**B2B priorities:** Oversight workflows, audit trails, confidence indicators, role-based controls.
a16z: For B2B, move away from chat UX — chat separates user from workflow.

**Consumer priorities:** Simplicity, progressive disclosure, retention-focused design.

**Both:** Transparency, graceful degradation, HITL as non-negotiable.
</framework_synthesis>

<progressive_disclosure>

**Principles:**
- Reveal complexity stepwise
- Simple features first, advanced on demand
- Maximum 2-3 levels (more = frustration)
- Clear indication for accessing more options
- Tailoring by user segment

**Three-layer model:**
- Layer 1 (Index): Lightweight metadata
- Layer 2 (Details): Full content when relevant
- Layer 3 (Deep Dive): Supporting material for deep analysis
</progressive_disclosure>

<graceful_degradation>

**Mandatory elements:**
- Plain-text error messages, not technical codes
- Confirmation that user work is saved
- 2-3 recovery options (retry, wait, offline mode)
- Warm colors (amber/yellow) instead of aggressive red
- Functioning base features when AI fails
- User context preserved across failures
</graceful_degradation>

<trust_calibration>

**Core problem:** Nature (2024): Over-trust in AI causes serious safety issues.
More human-like systems increase trust beyond actual capabilities.

**Before interaction (most effective):**
- Explicit expectations about capabilities and limitations
- Show performance statistics
- Tool language ("Analysis suggests") not human language ("I think")
- Exact confidence percentages, not vague hedging

**Critical insight (FAccT 2024):** First impressions form persistent trust —
trust established in early phases influences assessment regardless of later
AI accuracy. → Calibrate correctly from the start.
</trust_calibration>

<apple_hig_dimensions>

| Dimension | Spectrum | Design Implication |
|-----------|---------|-------------------|
| Criticality | Complementary → Critical | Higher criticality = higher reliability needed |
| Data sensitivity | Low → High (health) | Higher privacy requirements |
| Proactivity | User-initiated → Unsolicited | Proactive requires higher precision |
| Visibility | Invisible → Visible | Visible requires explainability |
| Dynamics | Static → Continuously learning | Dynamic requires notification |
</apple_hig_dimensions>
