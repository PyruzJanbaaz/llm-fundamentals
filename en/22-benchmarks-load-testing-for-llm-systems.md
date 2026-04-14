# 22 — Benchmarks & Load Testing for LLM Systems

## Introduction
Benchmarking and load testing validate the performance, reliability, and scalability of LLM systems.
LLM systems differ from traditional software because they are:

- probabilistic
- multi-component
- data-dependent
- model-dependent
- retrieval-dependent

This chapter shows how to benchmark and load test LLM applications in a systematic, repeatable, and production-ready way.

---

# Part 1 — Why Benchmarking Matters

| Why it matters | LLM impact |
|---|---|
| non-deterministic outputs | same input can produce different answers |
| multi-stage pipelines | many failure points (retrieval, ranking, generation, safety, routing, agents) |
| model-dependent performance | small/medium/large models behave differently |
| variable latency and cost | token usage changes per request |
| quality drift | hallucination rate can increase over time |

Benchmarking ensures stability, reliability, and cost control.

---

# Part 2 — What to Benchmark

| Category | Signals | Why it matters |
|---|---|---|
| Latency | p50, p90, p95, p99, cold/warm start, model/retrieval/agent latency | performance visibility |
| Throughput | RPS, concurrent users, queue depth | capacity planning |
| Cost | tokens/request, cost/request, cost/user, cost/feature | budgeting and optimization |
| Quality | accuracy, groundedness, hallucination rate, consistency, instruction following | user trust |
| Retrieval | recall, precision, MRR, grounding score | RAG effectiveness |
| Agent stability | tool success rate, loop detection, plan quality, failure modes | agent reliability |

Benchmarking must cover every layer of the system.

---

# Part 3 — Benchmarking LLM Quality

Quality tests should be automated and repeatable.

| Test type | What it measures | Example |
|---|---|---|
| Accuracy | output matches gold standard | answer correctness |
| Hallucination | unsupported claims | false facts |
| Grounding | relies on retrieval context | RAG-supported answers |
| Consistency | repeatability over runs | same prompt, similar response |
| Instruction following | obeys prompt rules | correct formatting or behavior |
| Safety | avoids unsafe content | no policy violations |

### Example quality benchmark
```python
for prompt, expected in test_set:
    output = model.generate(prompt)
    assert matches(expected, output)
```

---

# Part 4 — Benchmarking RAG Systems

RAG systems require dedicated metrics.

| Metric | What it measures |
|---|---|
| retrieval recall | relevant chunks retrieved |
| retrieval precision | retrieved chunks are relevant |
| re-ranking quality | ordering improves answer quality |
| context utilization | model uses retrieved evidence |
| grounding score | answer is supported by sources |
| chunk quality | chunk coherence and relevance |

### RAG benchmark example
```python
retrieved = retriever.search(query, top_k=10)
score = grounding_score(model, query, retrieved)
```

Better RAG performance usually means lower hallucination.

---

# Part 5 — Benchmarking Agents

Agents behave like software and need special benchmarks.

| Agent metric | Why it matters |
|---|---|
| tool call accuracy | right tool, right args |
| plan quality | efficient and logical steps |
| loop stability | avoids infinite or repeated loops |
| tool latency | slow tools slow agents |
| error recovery | recovers from failures |
| memory use | correct state reads/writes |

### Agent benchmark example
```python
actions = agent.plan(task)
assert is_valid_plan(actions)
result = agent.execute(actions)
assert result.success
```

Agent benchmarks are closer to software testing than ML testing.

---

# Part 6 — Load Testing LLM Systems

Load testing validates system scalability.

| Metric | Meaning |
|---|---|
| RPS | how many requests per second the system supports |
| concurrency | how many simultaneous users are handled |
| queue depth | request backlog under load |
| latency under load | p95/p99 behavior during stress |
| token throughput | tokens processed per second |
| hardware utilization | GPU/CPU/memory saturation |

### Example load test script
```python
for user in range(concurrent_users):
    spawn_request(user_request)
measure_latency()
measure_error_rate()
```

Load testing must simulate real user patterns, not just synthetic traffic.

---

# Part 7 — Load Testing RAG Systems

RAG load testing includes index and retrieval load.

| Area | What to test |
|---|---|
| vector DB latency | search, filter, and re-ranking delays |
| index throughput | query and update rate |
| cache hit rate | how often cached results are used |
| embedding load | batch embedding performance |

### RAG load test example
```python
for query in query_stream:
    results = vector_db.search(query)
    assert results.latency < threshold
```

RAG load testing is essential for enterprise-scale systems.

---

# Part 8 — Load Testing Multi-Model Routing

Routing adds complexity under load.

| Metric | Why it matters |
|---|---|
| model distribution | small/medium/large usage balance |
| fallback rate | how often requests escalate |
| cost under load | expensive models should not dominate |
| latency distribution | routing overhead and bottlenecks |

### Routing test example
```python
route = router.predict(request)
model = select_model(route)
response = model.generate(request)
```

Routing load tests ensure cost efficiency and stability.

---

# Part 9 — Stress Testing

Stress tests reveal hidden failure modes.

| Stress type | Purpose |
|---|---|
| spike testing | sudden traffic surge behavior |
| soak testing | long-duration high load |
| failure injection | simulate model, DB, or network failure |
| chaos testing | random component failures |

### Example chaos scenario
```python
kill_service("vector-db")
send_requests(1000)
check_system_recovery()
```

Stress testing exposes resilience issues before production incidents.

---

# Part 10 — Benchmarking Tools & Automation

A benchmark suite should be:

- automated
- repeatable
- versioned
- integrated into CI/CD
- monitored

Benchmark runs should be triggered by:

- model updates
- prompt updates
- index updates
- routing updates
- safety updates

### Example CI integration
```yaml
jobs:
  benchmark:
    runs-on: ubuntu-latest
    steps:
      - run: python tests/benchmark_latency.py
      - run: python tests/benchmark_quality.py
      - run: python tests/benchmark_rag.py
```

Benchmarking is a core part of LLMOps.

---

# Summary
Benchmarking and load testing are essential for reliable, scalable, and cost-efficient LLM systems.
A complete benchmark suite measures:

- latency
- throughput
- cost
- quality
- hallucination rate
- retrieval quality
- agent stability
- routing performance

Load testing verifies real-world capacity.
Benchmarking ensures quality persists over time.

Together, they transform an LLM prototype into a **production-grade AI platform**.
