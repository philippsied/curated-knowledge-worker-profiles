<overview>
Stable reference for AI/ML core: RAG architecture, agent patterns, model strategy,
prompt engineering, evaluation frameworks, hallucination management, and data flywheel.
N/A for non-AI SaaS products.
</overview>

<rag_architecture>

**Maturity ladder:**

| Level | Suitability | Performance |
|-------|------------|-------------|
| Naive RAG | Prototypes only | 10-40% success in enterprise |
| Advanced RAG | **Production standard** | +15-30% retrieval accuracy |
| Graph RAG | Relationship queries | 99% precision, high complexity |
| Agentic RAG | Few specialized cases | Agent uses retrieval as one tool among many |

**Advanced RAG components (production standard):**
- Hybrid search: vector + BM25 via reciprocal rank fusion → +15-30%
- Cross-encoder reranking: +23.4%, 50-100ms latency
- Query transformation: HyDE → +20-35% for ambiguous queries
- Parent document retrieval: small chunks match, large deliver context

**Data pipeline:**
- Optimal chunk size: 300-500 tokens
- SPLICE method: +27% answer precision
- Semantic chunking: +9% recall vs. fixed-size
</rag_architecture>

<agent_architectures>

**Decision rule:**
- Single agent: workflow is primarily one agent with tools
- Multi-agent: workflow is genuinely multi-role
- "Start simple, scale smart"

**Workflow modules (10 core):**
Ingestion → RAG/Knowledge → Prompt Templates → AI Orchestration ← Safety (Input) →
Evaluation ← Monitoring → Safety (Output) → HITL Review → Output Formatting →
Feedback and Annotation → Monitoring → Prompt (improvement signals)
</agent_architectures>

<model_strategy>

**Three tiers:**
| Tier | Use Case | Examples |
|------|----------|---------|
| Cheap | Routing, simple tasks | GPT-4o-mini, Gemini Flash |
| Mid | Moderate reasoning | Claude Sonnet, GPT-4o |
| Premium | Complex multi-step reasoning | Claude Opus, o3 |

**Model fallback chain:** Primary → secondary → tertiary across providers for resilience.
</model_strategy>

<seven_quality_dimensions>

| Dimension | Target | Measurement |
|-----------|--------|-------------|
| Accuracy | >90% (enterprise) | LLM-as-Judge, fact-checking |
| Faithfulness | >0.85 score | RAGAS, DeepEval |
| Relevance | Context-dependent | Semantic similarity, RAGAS |
| Latency | &lt;2s TTFT, &lt;5s total | P50/P95/P99 monitoring |
| Consistency | &lt;10% variance | SCORE framework, multi-run |
| Safety | >99% compliance | NeMo Guardrails, toxicity classifiers |
| Cost efficiency | Use-case specific | Cost per successful task |
</seven_quality_dimensions>

<hallucination_management>

**Inference-time strategies:**
- RAG (most common, does not fully eliminate)
- Chain-of-thought prompting (logical coherence)
- Self-consistency (multiple paths, majority vote)
- Prompt calibration (can reduce hallucination significantly)
- Abstention training ("I don't know")

**In production:**
- Continuous fact-checking against knowledge bases
- Confidence score display for users
- Span-level verification in RAG pipelines
</hallucination_management>

<testing_nondeterministic>

**Fundamental separation:**
- Deterministic layer (data fetching, prompt assembly, schema validation): test classically
- Non-deterministic layer (model output): test probabilistically

**Approaches:**
- Probabilistic pass rates: N runs, "passes X% of runs"
- Dimensions not values: accuracy, coverage, tone, format — on rubric scale
- LLM-as-Judge: stronger model matches human judgment ~80%. Multiple judges as "jury"

**Four-layer test architecture:**
1. Unit tests: mock LLM responses, test deterministic logic
2. Integration tests: real LLM calls, check format/schema
3. Evaluation tests: curated datasets, automated metrics
4. Shadow testing: new prompt parallel to production, no user risk
</testing_nondeterministic>

<data_flywheel>

**Cycle:** Product → Feedback → Improve model → More users → More feedback → Compound improvement

**6 stages (NVIDIA):**
1. Capture production data
2. Curate training signals
3. Fine-tune candidate models
4. Rigorously evaluate
5. Seamlessly deploy
6. Collect intrinsic + extrinsic feedback

**Retraining triggers:**
- Accuracy below threshold for Y days
- Data drift (PSI > threshold)
- New training data > X records
- Scheduled quarterly refresh
</data_flywheel>

<prompt_management>

**Best practices:**
- Central, versioned (SemVer), tested, deployed as immutable artifacts
- Decoupled from code deployments
- Non-technical users (PMs, domain experts) can edit and test
- Environment management: dev/staging/production with promotion gates
</prompt_management>
