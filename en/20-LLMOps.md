# 20 — LLMOps (CI/CD for AI Systems)

## Introduction
LLMOps manages the lifecycle of AI systems in production.
It extends DevOps with AI-specific needs for:

- model versioning
- embedding and index versioning
- prompt versioning
- evaluation pipelines
- safety checks
- rollback strategies
- monitoring and drift detection

LLMOps ensures AI systems are reliable, reproducible, and continuously improving.

---

# Part 1 — Why LLMOps Is Needed

| Traditional DevOps | LLMOps challenge | Why it matters |
|---|---|---|
| deterministic code | non-deterministic model outputs | same input can produce different results |
| predictable behavior | behavior changes with data and model | regressions may appear silently |
| code-only deployments | prompt, data, and model deployment | many moving parts must be versioned |

LLMOps adds structure and automation to handle AI-specific complexity.

---

# Part 2 — LLMOps Components

| Domain | What to manage | Example tools |
|---|---|---|
| Model management | versioning, deployment, rollback, A/B tests | model registry, feature flags |
| Data management | dataset versioning, embedding pipelines, metadata governance | data version control, ETL, pipeline schedulers |
| Prompt management | template versioning, testing, review, rollback | git, prompt stores |
| Evaluation | offline and online evaluation, hallucination and safety tests | test harnesses, metrics systems |
| Monitoring | latency, cost, retrieval quality, hallucinations, safety | observability dashboards |
| CI/CD | automated tests, deployments, evaluation gating | pipelines, canary releases |

LLMOps combines DevOps, DataOps, MLOps, and AI-specific workflow automation.

---

# Part 3 — Model Versioning

Models must be versioned like code.

Track:
- architecture
- training and fine-tuning data
- hyperparameters
- tokenizer version
- safety config
- deployment target

| Why version | Benefit |
|---|---|
| reproducibility | rerun experiments and reproduce results |
| rollback | recover from bad model releases |
| auditing | trace decisions to a model version |
| comparison | evaluate new vs. old models |

### Example model metadata
```yaml
model_name: llm-chat-13b
version: 2.1.0
architecture: transformer
training_data: curated-chat-v3
tokenizer: tknr-v1
safety_profile: high
```

---

# Part 4 — Embedding & Vector DB Versioning

Embedding pipelines and vector indexes are part of the model lifecycle.

| Change source | What changes | LLMOps response |
|---|---|---|
| embedding model update | embedding vectors change | re-embed corpus and publish new index |
| chunking or cleaning change | document structure changes | rebuild index and version it |
| metadata or filter change | retrieval behavior changes | validate new index quality |

LLMOps must support:
- embedding versioning
- index versioning
- incremental re-embedding
- index migration
- rollback to previous index

### Embedding pipeline example
```python
for record in dataset:
    embedding = embed_model.encode(record.text)
    index.upsert(id=record.id, vector=embedding, metadata=record.meta)
```

---

# Part 5 — Prompt Versioning

Prompts are code and must be managed as such.

| Prompt lifecycle | Why it matters |
|---|---|
| version control | track prompt changes over time |
| testing | detect prompt regressions early |
| review | ensure prompt quality and safety |
| rollback | recover broken prompt updates |

### Prompt version example
```yaml
prompt_id: summary-v2
template: |
  Summarize the following text in 3 sentences:
  {{user_text}}
version: 2026-04-14
reviewer: ai-engineering-team
```

Prompt drift and prompt decay are real production risks.

---

# Part 6 — Evaluation Pipelines

Automated evaluation protects against regressions.

| Evaluation type | What it tests | Typical outcome |
|---|---|---|
| Offline | datasets, synthetic tests, hallucinations, safety | pass/fail before deployment |
| Online | A/B tests, shadow mode, user feedback | choose best model/config |

### Evaluation pipeline example
```python
results = evaluate(model, test_set)
if results.hallucination_rate > 0.05:
    reject_deployment()
elif results.accuracy < baseline:
    reject_deployment()
else:
    promote_model()
```

Evaluation ensures quality does not regress over time.

---

# Part 7 — CI/CD for AI Systems

A complete LLMOps pipeline includes multiple validation stages.

| Stage | Purpose | Example action |
|---|---|---|
| Data validation | ensure clean inputs | remove duplicates, enforce metadata |
| Embedding pipeline | build/rebuild vector index | batch or incremental embedding |
| Prompt tests | verify formatting and instructions | unit tests for prompt outputs |
| Model tests | check accuracy, grounding, safety | run automated model evaluation |
| Deployment | release safely | canary, shadow, A/B launch |
| Rollback | recover quickly | revert prompt/model/index changes |

### CI/CD flow example
```yaml
steps:
  - run: data_validation.py
  - run: embedding_build.py
  - run: prompt_tests.py
  - run: model_evaluation.py
  - deploy: canary
  - monitor: 30m
```

LLMOps makes AI systems safe to deploy and easy to manage.

---

# Part 8 — Drift Detection

Drift emerges naturally in production AI.

| Drift type | Signal | Response |
|---|---|---|
| Model drift | quality drop, unexpected behavior | retrain or fine-tune model |
| Data drift | changed user input distribution | update prompts or retrain |
| Retrieval drift | reduced grounding or recall | refresh index and validate |
| Prompt drift | prompt effectiveness decreases | revise templates and test |
| Safety drift | more unsafe outputs | tighten safety filters and review policy |

### Drift alert example
```python
if moving_average(grounding_score, 1d) < 0.7:
    alert("retrieval drift detected")
```

LLMOps must detect drift early and automate response paths.

---

# Part 9 — Governance & Compliance

LLMOps enforces policies across AI pipelines.

| Requirement | LLMOps control |
|---|---|
| privacy | data anonymization, access controls |
| retention | dataset and log expiry policies |
| auditability | versioned artifacts and logging |
| explainability | traceable decisions and metadata |
| access control | role-based permissions |

Governance is essential for enterprise AI trust and compliance.

---

# Summary
LLMOps is the foundation of reliable production AI.
It includes:

- model and embedding versioning
- prompt management
- evaluation and CI/CD pipelines
- monitoring and drift detection
- safety and governance controls

LLMOps transforms AI from a prototype into a **reliable, scalable, enterprise-grade system**.
