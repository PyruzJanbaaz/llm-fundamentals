# 13 — System Design for LLM Apps

> Tags: `#SystemDesign` `#LLMApps` `#RAG` `#Agents` `#Observability`

## Introduction
Building an LLM application is not just about calling a model.  
A real production system requires:

- architecture
- pipelines
- retrieval
- safety
- monitoring
- evaluation
- scaling
- cost control

LLM apps are **systems**, not prompts.

This chapter explains how to design end-to-end LLM systems that are reliable, scalable, and production-ready.

---

# Part 1 — The LLM System Stack

| Layer | Purpose |
| --- | --- |
| User Interface | Collect user intent and display responses |
| API Gateway | Route requests, authenticate, throttle |
| Orchestration | Choose model, route to RAG or agents |
| LLM Inference | Generate text and manage model settings |
| RAG Pipeline | Retrieve, rank, and assemble context |
| Agent Framework | Plan actions, execute tools, manage memory |
| Safety & Guardrails | Enforce policies and safe behavior |
| Monitoring & Logging | Collect metrics, traces, and audit logs |
| Evaluation & Feedback | Measure quality and improve over time |

Each layer has its own responsibilities and failure modes.

---

# Part 2 — Core Architectural Patterns

| Pattern | When to use it | Strengths | Limitations |
| --- | --- | --- | --- |
| Direct LLM Call | Simple chat, summarization, rewrite | Fast to build, low engineering overhead | Hallucinations, no external knowledge |
| RAG-First | Domain QA, knowledge-intensive apps | Grounded answers, better factuality | Requires retrieval quality |
| Agentic | Automation, tool workflows, reasoning | Action execution, planning, extended behavior | Harder to control, safety critical |
| Hybrid | Enterprise systems | Best of RAG, agents, safety, routing | Complex architecture |

Example direct call flow:

```python
response = model.generate(prompt)
```

Example RAG flow:

```python
chunks = retriever.search(query)
context = assemble_context(chunks)
prompt = build_prompt(query, context)
response = model.generate(prompt)
```

---

# Part 3 — End-to-End Architecture Blueprint

| Stage | What happens |
| --- | --- |
| Input Layer | UI, API, SDK, or voice input arrives |
| Preprocessing | Normalize text, detect user role, sanitize input |
| Retrieval (RAG) | Chunking, embedding, search, ranking, assembly |
| Orchestration | Select model, route requests, choose agent path |
| LLM Inference | Generate output with temperature and streaming |
| Post-Processing | Grounding checks, formatting, citations |
| Safety Layer | Output filtering, policy enforcement, redaction |
| Monitoring | Logs, metrics, traces, token usage, latency |
| Evaluation | Offline tests, online metrics, audits |
| Feedback Loop | User feedback, error analysis, dataset updates |

This is the architecture used by real AI products.

Example orchestration code:

```python
def handle_request(request):
    if is_rag_query(request):
        context = retrieve_context(request.text)
        prompt = build_rag_prompt(request.text, context)
    else:
        prompt = build_prompt(request.text)

    response = model.generate(prompt)
    response = safety.filter(response)
    log_request(request, response)
    return response
```

---

# Part 4 — Designing Prompts as System Components

Prompts in production must be:

- modular
- versioned
- testable
- parameterized
- monitored

A prompt is not a string — it is a **software artifact**.

| Prompt concern | Production best practice |
| --- | --- |
| Reusability | Use templates with placeholders |
| Versioning | Track prompt changes in source control |
| Testability | Validate with unit tests and examples |
| Parameterization | Inject variables safely |
| Monitoring | Log runtime prompt variants and performance |

Example prompt template:

```python
template = """
Answer the user question using the context below.
Context:
{context}

Question:
{query}
"""
```

---

# Part 5 — Designing RAG Systems

| Decision | Trade-off |
| --- | --- |
| Chunk size | Small = precision, Large = recall |
| Embedding model | Domain-specific or general-purpose |
| Retrieval strategy | Vector, hybrid, or re-ranked search |
| Context assembly | Top-K, max-tokens, semantic grouping |
| Grounding instructions | Force the model to cite retrieved context |

RAG is a **retrieval system**, not just embeddings.

Example retrieval pipeline:

```python
chunks = chunk_documents(documents)
vectors = embed(chunks)
results = vector_search(query, vectors)
context = assemble([r.text for r in results], max_tokens=1200)
```

---

# Part 6 — Designing Agent Systems

Agents require careful engineering.

| Component | Why it matters |
| --- | --- |
| Tool definitions | Define available actions and safety constraints |
| Planning strategies | Decide the next best action |
| Memory systems | Preserve context across turns |
| Safety constraints | Prevent dangerous or unauthorized actions |
| Loop control | Avoid infinite or repeated behaviors |
| State management | Keep consistent application state |

Agents are powerful but must be engineered carefully.

Example simplified agent loop:

```python
while not task.complete:
    action = planner.next_action(state)
    result = executor.run(action)
    state = memory.update(state, result)
```

---

# Part 7 — Safety as a System Property

Safety must be enforced at every layer.

| Layer | Safety focus |
| --- | --- |
| Input | Prompt sanitization, user intent checks |
| Retrieval | Filter harmful or low-quality documents |
| Generation | Model safety tuning and guardrails |
| Output | Content filtering and redaction |
| Tool usage | Permission checks and sandboxing |
| Memory | Prevent sensitive data leaks |

Safety is not optional — it is part of the architecture.

---

# Part 8 — Observability & Monitoring

A production LLM system must track key signals.

| Signal | Why it matters |
| --- | --- |
| Latency | Detect slow responses |
| Token usage | Control cost |
| Cost | Monitor spend across users and endpoints |
| Retrieval quality | Validate RAG effectiveness |
| Hallucination rate | Measure reliability |
| Safety violations | Detect policy breaches |
| Agent tool calls | Audit external actions |
| User behavior | Find edge cases and drift |

Observability is essential for debugging and improvement.

---

# Part 9 — Scaling & Optimization

| Technique | Benefit |
| --- | --- |
| Caching | Avoid repeated inference and retrieval |
| Batching | Increase throughput |
| Model routing | Choose the right model for each task |
| Prompt compression | Fit more context into the window |
| Context optimization | Keep only relevant data |
| Multi-model pipelines | Use specialized models for different tasks |
| Cost monitoring | Detect and prevent unexpected spend |

Scaling is not about bigger models — it is about smarter architecture.

---

# Summary
System design is the heart of AI engineering.  
A real LLM application is not just a model — it is an ecosystem of:

- retrieval
- generation
- agents
- safety
- monitoring
- evaluation
- scaling

Mastering system design transforms a Software Engineer into an **AI Engineer**.
