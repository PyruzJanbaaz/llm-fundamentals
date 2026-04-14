# 19 — Monitoring & Observability (Deep Dive)

## Introduction
LLM systems are dynamic, probabilistic, and unpredictable.
They can fail in ways traditional software does not.

| Failure mode | Why it matters |
|---|---|
| hallucinations | wrong answers without errors |
| retrieval failures | missing or irrelevant context |
| prompt drift | reduced accuracy over time |
| agent loops | runaway behavior and wasted compute |
| cost explosions | unexpected spending |
| latency spikes | poor user experience |
| safety violations | harm, policy risk |

Observability is not just logging.
It is the ability to understand **why** the system behaves the way it does.

---

# Part 1 — Why Observability Matters in LLM Systems

LLM observability is essential because:

| Reason | Impact |
|---|---|
| Non-deterministic output | same input can produce different answers |
| Silent failures | bad results occur without exceptions |
| Multi-component pipelines | retrieval, ranking, generation, safety, routing, agents, memory all interact |
| Cost volatility | token usage and model choice change cost dynamically |
| Safety risk | harmful outputs must be detected and blocked |

A production AI system needs observability to spot issues early and fix them quickly.

---

# Part 2 — What to Monitor

| Category | Key signals | Why it matters |
|---|---|---|
| Latency | total, model, retrieval, agent tool, network | diagnose performance bottlenecks |
| Token usage | input, output, total, cost/request, cost/user | control spending and chargeback |
| Retrieval quality | recall, precision, grounding score, irrelevant/missing chunks | ensure answers are grounded |
| Hallucinations | unsupported claims, citation mismatches, grounding violations | measure correctness |
| Safety | blocked prompts, unsafe outputs, jailbreak attempts | reduce risk and abuse |
| Agent behavior | tool calls, tool failures, loop detection, plan depth | prevent runaway agents |
| Routing | selected model, fallback frequency, confidence score | optimize cost and quality |
| System health | memory, CPU/GPU, queue depth, error rates | maintain reliability |

Observability is multi-dimensional; every signal should map to a concrete action.

---

# Part 3 — Logging for LLM Systems

Structured logs should capture the full request lifecycle.

| Log element | Example contents | Use case |
|---|---|---|
| Prompts | raw prompt, user message, system instructions | reproduce failure |
| Responses | model output, token counts | evaluate quality and cost |
| Retrieved chunks | chunk text, metadata, score | audit grounding sources |
| Model config | temperature, top-p, max tokens | compare configuration impact |
| Routing decisions | model chosen, fallback, confidence | optimize routing logic |
| Agent actions | tool name, args, result | debug tool execution |
| Safety filters | input/output filter status | investigate policy events |
| Cost metrics | token usage, estimated cost | monitor spend |

### Example structured log
```json
{
  "request_id": "req-123",
  "user_id": "user-42",
  "prompt": "Summarize the article...",
  "model": "medium-13b",
  "response": "...",
  "tokens": {"input": 450, "output": 120, "total": 570},
  "cost": 0.045,
  "routing": {"selected": "medium-13b", "fallback": false, "confidence": 0.92},
  "safety": {"input_filtered": false, "output_filtered": false}
}
```

Logs must be queryable, searchable, and easy to aggregate.

---

# Part 4 — Tracing

Tracing connects request flow across components.
A good trace includes:

| Trace span | What it captures |
|---|---|
| request | API call, request metadata |
| retrieval | index queries, chunk hits, scores |
| ranking | candidate ranking steps |
| generation | model response time, token counts |
| agent | tool calls, action sequence |
| safety | filter decisions, policy checks |
| routing | model selection and fallback logic |

### Example trace pseudocode
```python
with tracer.start_span("request") as request_span:
    request_span.set_attribute("user_id", user_id)
    with tracer.start_span("retrieval"):
        retrieve_documents(query)
    with tracer.start_span("generation"):
        generate_answer(prompt)
```

Tracing is essential for debugging complex errors and latency issues.

---

# Part 5 — Metrics & Dashboards

Dashboards should surface the most important operational signals.

| Dashboard | Key metrics |
|---|---|
| Latency | p50, p90, p95, p99; per model; per endpoint |
| Cost | cost per request, cost per user, cost per feature, spend over time |
| Retrieval | recall, precision, top-k distribution, chunk overlap |
| Hallucination | hallucination rate, grounding violations, unsupported claims |
| Safety | blocked prompts, unsafe outputs, jailbreak rate |
| Agent behavior | tool call frequency, failure rate, loop detection |

### Example monitoring query
```sql
SELECT
  percentile(latency_ms, 95) AS p95,
  avg(cost) AS avg_cost,
  sum(case when unsafe_output then 1 else 0 end) AS unsafe_count
FROM requests
WHERE timestamp > now() - interval '1 hour';
```

Dashboards turn raw data into actionable insight.

---

# Part 6 — Alerting

Alerts should be precise, actionable, and low-noise.

| Condition | Alert example | Recommended action |
|---|---|---|
| latency spike | p95 > 1.5x baseline | investigate model or infra bottleneck |
| cost spike | cost/request increases by 30% | inspect high-cost requests |
| retrieval failure | recall < threshold | review index refresh and prompts |
| hallucination surge | hallucination rate rises | tune prompts or escalate models |
| safety increase | unsafe outputs spike | audit safety filters and blocklists |
| agent loop | agent exceeds max steps | disable agent and inspect plan logic |
| routing failure | fallback rate climbs | validate router confidence and models |

### Alert rule example
```yaml
alert: HighLLMLatency
expr: histogram_quantile(0.95, sum(rate(model_latency_bucket[5m])) by (le, model)) > 1000
for: 5m
labels:
  severity: warning
annotations:
  summary: "LLM p95 latency is high"
```

Alerts are only useful when they lead to a clear remediation.

---

# Part 7 — Observability for RAG Systems

RAG systems need specialized signals.

| RAG signal | What to track | Why it matters |
|---|---|---|
| Retrieval logs | retrieved chunks, similarity scores, metadata filters | verify source selection |
| Retrieval metrics | recall, precision, grounding score | measure grounding quality |
| Chunk quality | chunk length, coherence, duplication | avoid noisy evidence |
| Index health | index size, update latency, drift | keep retrieval fresh |

### RAG observability example
```json
{
  "request_id": "req-123",
  "retrieval": {
    "chunks": [
      {"id": "c1", "score": 0.92, "source": "doc-7"},
      {"id": "c2", "score": 0.87, "source": "doc-3"}
    ],
    "grounding_score": 0.78,
    "recall": 0.85
  }
}
```

RAG observability helps ensure model answers are grounded, relevant, and reliable.

- embedding version mismatches  

RAG observability is essential for reducing hallucinations.

---

# Part 8 — Observability for Agents

Agents require deeper observability.

Track:

### **1. Tool Calls**
- which tool  
- arguments  
- results  
- latency  

### **2. Planning**
- number of steps  
- plan depth  
- plan failures  

### **3. Loop Detection**
- repeated actions  
- repeated reasoning  

### **4. State Management**
- memory reads  
- memory writes  

Agents without observability are impossible to debug.

---

# Part 9 — Observability for Routing

Routing must be monitored:

### **1. Model Usage**
- small vs medium vs large  

### **2. Fallback Rate**
High fallback → routing failure.

### **3. Confidence Scores**
Low confidence → hallucination risk.

### **4. Cost Distribution**
Large models must not dominate cost.

Routing observability = cost control + quality control.

---

# Part 10 — LLM-Specific Failure Modes

Observability must detect:

### **1. Prompt Drift**
Prompts degrade over time.

### **2. Model Drift**
Model updates change behavior.

### **3. Retrieval Drift**
Index changes affect recall.

### **4. Memory Drift**
Outdated memory causes errors.

### **5. Safety Drift**
Safety filters weaken over time.

LLM systems drift — observability catches it.

---

# Summary
Monitoring and observability are essential for building reliable, scalable, and safe LLM systems.  
A complete observability stack includes:

- logs  
- traces  
- metrics  
- dashboards  
- alerts  
- retrieval monitoring  
- agent monitoring  
- routing monitoring  
- cost monitoring  
- safety monitoring  

Observability transforms an LLM system from a black box into a **transparent, debuggable, production‑grade AI platform**.
