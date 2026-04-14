# 18 — Model Selection & Routing

## Introduction
No production AI system uses only one model.
Most real systems use **multiple models**, each optimized for:

- cost
- latency
- accuracy
- reasoning
- domain expertise
- safety

Model selection and routing decide **which model** handles **which request**.

This chapter explains how to design multi-model architectures that are fast, cheap, and reliable.

---

# Part 1 — Why Model Selection Matters

Different models are designed for different workloads.

| Model size | Typical use cases | Primary benefit |
|---|---|---|
| Small | classification, extraction, routing, rewriting, simple Q&A | low latency, low cost |
| Medium | reasoning, summarization, multi-turn chat | balanced cost and quality |
| Large | complex reasoning, creativity, agent planning, code generation | highest accuracy and capability |

Using one model for everything causes:

- unnecessary cost
- high latency
- wasted compute
- poor scalability

Routing is the mechanism that avoids those tradeoffs.

---

# Part 2 — Types of Models in a Production System

| Model tier | Parameter range | Best for | When to use |
|---|---|---|---|
| Small | 1B–8B | fast inference, low-cost tasks | classification, extraction, routing, rewriting |
| Medium | 8B–30B | balanced general-purpose work | summarization, reasoning, multi-turn chat |
| Large | 30B–70B+ | complex, ambiguous, high-value tasks | deep reasoning, creativity, code generation |

### Quick model decision guide
- Use small models for predictable, high-volume work.
- Use medium models for moderate complexity and multi-turn understanding.
- Use large models only when quality or reasoning exceeds smaller model capability.

---

# Part 3 — Routing Strategies

Routing decides which model serves a request.

| Strategy | How it works | Pros | Cons |
|---|---|---|---|
| Rule-based | Hard-coded if/else rules | fast, predictable, easy | not adaptive, brittle |
| Classifier-based | Small model labels the request | flexible, more accurate | needs tuning, extra cost |
| LLM-based | LLM chooses the best model | highly adaptive, handles ambiguity | latency, prompt risk |
| Cost-aware | Prefer small/fallback to large | dramatic savings | requires confidence/monitoring |
| Hybrid | Mix of rules, classifiers, cost, safety | robust, enterprise-ready | more complex to build |

## Rule-based routing
```python
if request.type == "classification":
    model = small_model
elif request.reasoning == "high":
    model = large_model
else:
    model = medium_model
```

## Classifier-based routing
```python
route = router_model.predict(request.text)
if route == "summary":
    model = medium_model
elif route == "legal":
    model = domain_expert_model
else:
    model = small_model
```

## LLM-based routing
Use a compact LLM to analyze input and choose a model or pipeline.

Example prompt:
> "Review the user request. Select the smallest model that can satisfy it while meeting quality and safety requirements."

## Cost-aware routing
Use cost signals and confidence to decide when to escalate.

Example flow:
- first attempt with a small model
- if confidence is low or request is complex, call a larger model
- cache results for repeated queries

---

# Part 4 — Confidence Scoring

Confidence helps routing decide when to escalate.

| Method | Description | Use case |
|---|---|---|
| Self-evaluation | Ask the model how confident it is | quick fallback decisions |
| Logit-based | Use token probability scores | low-level confidence signal |
| Cross-model verification | small model + large model verify | avoid hallucinations |
| RAG grounding score | check retrieval relevance | decide whether to escalate |

### Example: confidence-based fallback
```python
response, score = model.generate(input)
if score < 0.75:
    response = large_model.generate(input)
```

Confidence scoring reduces hallucinations and overall cost.

---

# Part 5 — Multi-Model Pipelines

Production systems often chain several models.

| Pipeline | Steps | Purpose |
|---|---|---|
| Classification → Retrieval → Generation | classify → retrieve → generate | structured Q&A and RAG |
| Summarization → Compression → Generation | summarize → compress → generate | improve context efficiency |
| Router → Model A/B/C | route → selected model | dynamic task allocation |
| Safety → Generation → Safety | pre-filter → generate → post-filter | end-to-end safety |

### Example pipeline
```python
input = safety_model.filter(input)
route = router_model.classify(input)
output = selected_model.generate(route, input)
result = safety_model.filter(output)
```

Multi-model pipelines improve quality, safety, and reliability.

---

# Part 6 — Cost Optimization with Routing

Routing is one of the strongest cost-saving levers in AI systems.

| Technique | What it does | Benefit |
|---|---|---|
| Small-model first | try small model, escalate only if needed | lower average cost |
| Token budgeting | reserve large models for long/complex input | better cost control |
| Domain-specific models | use specialized models for verticals | higher accuracy, lower cost |
| Caching | reuse expensive results | eliminate repeated work |

### Cost-aware decision example
```python
if request.domain in ["legal", "finance"]:
    model = domain_model
elif request.complexity > 0.8:
    model = large_model
else:
    model = small_model
```

With good routing, cost reductions of **5× to 20×** are realistic.

---

# Part 7 — Safety-Aware Routing

Routing must incorporate safety checks.

| Trigger | Routed model | Why |
|---|---|---|
| unsafe query | safety model | block or sanitize input |
| ambiguous query | large model | better reasoning |
| high-risk domain | specialized model | reduce risk |
| tool invocation | verified model | maintain trust |

Safety is a core part of routing logic, not an afterthought.

---

# Part 8 — Monitoring & Observability for Routing

Track:

- model usage  
- cost per model  
- latency per model  
- routing decisions  
- fallback frequency  
- confidence scores  
- safety violations  

Routing must be observable to be reliable.

---

# Summary
Model selection and routing are essential for building scalable, cost‑efficient, high‑quality AI systems.  
A real AI system uses:

- small models for simple tasks  
- medium models for reasoning  
- large models for complex tasks  
- routing to choose the right model  
- confidence scoring to reduce hallucinations  
- multi‑model pipelines for quality and safety  

Routing transforms an LLM system from a single‑model demo into a **production‑grade AI architecture**.
