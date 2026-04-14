# 12 — Evaluation & Hallucinations

> Tags: `#Evaluation` `#Hallucinations` `#RAG` `#AgentTesting`

## Introduction
Evaluation is one of the most important — and most overlooked — parts of building LLM systems.  
A model that performs well in a demo may fail in production due to:

- hallucinations
- inconsistent reasoning
- poor retrieval
- weak grounding
- safety violations
- unpredictable user inputs

Evaluation ensures that an LLM system is **reliable, measurable, and production-ready**.

This chapter explains how to evaluate LLMs, detect hallucinations, and measure real-world performance.

---

# Part 1 — Understanding Hallucinations

## What Is a Hallucination?
A hallucination occurs when an LLM produces:

| Hallucination behavior | Example |
| --- | --- |
| Incorrect information | “The capital of Australia is Sydney.” |
| Fabricated facts | Made-up dates, events, or data |
| Unsupported claims | Statements with no evidence |
| Confident wrong answers | “Yes, I did that.” when it did not |
| Invented citations | Fake URLs or document IDs |

Hallucinations are not bugs — they are a natural consequence of next-token prediction.

---

## Why Hallucinations Happen
LLMs hallucinate for several core reasons.

| Cause | What it means |
| --- | --- |
| Predicting text, not truth | The model optimizes likelihood, not factuality |
| Missing context | The model lacks key information |
| Overgeneralization | Patterns are applied too broadly |
| Ambiguous prompts | Vague instructions produce vague answers |
| Retrieval failures | Wrong documents feed the model incorrect context |
| Long context degradation | Information fades over long sequences |

Hallucinations cannot be eliminated — only reduced and controlled.

---

# Part 2 — Types of Hallucinations

| Type | Description | Example |
| --- | --- | --- |
| Factual | Incorrect or false facts | “The moon is made of cheese.” |
| Logical | Broken reasoning | “If A > B and B > C, then C > A.” |
| Retrieval | Invented content not in context | Claims not supported by retrieved passages |
| Citation | Fake sources, URLs, or references | “According to Journal XYZ…” |
| Instruction | Misinterpreted instructions | Follows the wrong task | 
| Agentic | Incorrect tool behavior or tool hallucination | Calls a wrong API or invents a tool result |

---

# Part 3 — Evaluating LLM Systems

Evaluation must be **continuous**, not a one-time checkpoint.

A complete evaluation pipeline includes:

| Evaluation layer | Purpose |
| --- | --- |
| Offline evaluation | Measure model quality on test sets |
| Online evaluation | Observe behavior with real users |
| Hallucination testing | Track unsupported or false outputs |
| Safety testing | Detect harmful or policy-violating outputs |
| Retrieval evaluation | Validate RAG relevance and grounding |
| Agent evaluation | Validate tool use, plans, and outcomes |

---

## Offline Evaluation
Offline evaluation uses static datasets to measure repeatable quality.

| What to measure | Why it matters |
| --- | --- |
| Accuracy | Correct answers on known examples |
| Reasoning quality | Logical, consistent outputs |
| Grounding | Answer support from source text |
| Hallucination rate | Frequency of unsupported claims |
| Consistency | Stable output across similar prompts |
| Style adherence | Meets formatting or tone requirements |
| Instruction following | Completes the requested task |

### Common methods:
- curated test sets
- synthetic test sets
- benchmarks
- golden answers

Offline evaluation is fast and repeatable.

---

## Online Evaluation
Online evaluation measures behavior in production.

| Method | What it reveals |
| --- | --- |
| A/B testing | Which version performs better in the wild |
| User feedback | Real user satisfaction and failure reports |
| Production logs | Actual usage patterns and errors |
| Human review | Qualitative assessment of hard cases |
| Error analysis | Root cause identification |

Online evaluation captures real user behavior, edge cases, unexpected prompts, and long-tail failures.

Offline + online = complete evaluation.

---

# Part 4 — Evaluating RAG Systems

RAG introduces new evaluation dimensions because retrieval and generation interact.

## Key RAG metrics

| Metric | What it measures |
| --- | --- |
| Retrieval precision | Relevance of retrieved chunks |
| Retrieval recall | Coverage of all relevant chunks |
| Grounding score | Degree the answer is supported by context |
| Context utilization | How much retrieved text was actually used |
| Hallucination rate | Unsupported additions beyond the context |

---

## RAG Failure Modes

| Failure mode | What goes wrong | Mitigation |
| --- | --- | --- |
| Irrelevant retrieval | Wrong chunks are returned | Better search, filtering, relevance ranking |
| Missing retrieval | Needed context is not found | Expand recall, improve indexing |
| Over-retrieval | Too much context dilutes focus | Use stricter chunk selection |
| Under-retrieval | Not enough context is available | Add more documents or larger windows |
| Context mismatch | Retrieved text does not match the question | Improve query formulation |
| Context overflow | Window is exceeded, important text dropped | Summarize or compress context |

Evaluating RAG means evaluating the entire pipeline, not just the model.

---

# Part 5 — Evaluating Agents

Agents introduce additional software-like evaluation requirements.

| What to evaluate | Why it matters |
| --- | --- |
| Tool accuracy | Correct tool selection |
| Tool arguments | Correct and safe inputs |
| Planning quality | Appropriate decomposition of tasks |
| Loop stability | Avoid repeated or infinite cycles |
| Safety | Prevent unsafe or unauthorized actions |
| State management | Maintain consistent memory and context |

Agent evaluation is closer to **software testing** than traditional ML evaluation.

Example agent evaluation check:

```python
assert action.tool == "search"
assert action.params["query"] != ""
assert not action.looped_more_than(5)
```

---

# Part 6 — Techniques to Reduce Hallucinations

| Technique | Why it helps |
| --- | --- |
| Better prompting | Reduces ambiguity and guides the model |
| RAG grounding | Anchors answers to trusted data |
| Retrieval re-ranking | Improves relevance of context |
| Context compression | Removes noise from long input |
| Model selection | Use models better suited for factual tasks |
| Fine-tuning | Teach domain-specific accuracy |
| Guardrails | Block unsupported or harmful outputs |
| Self-critique | Have the model validate its own answer |

Example self-check prompt:

```text
Please answer the question, then verify whether your response is supported by the provided context.
Question: ...
Context: ...
```

---

# Part 7 — Metrics for Evaluation

| Metric | What it means |
| --- | --- |
| Accuracy | Correctness of the model’s answers |
| Faithfulness | Alignment with source context |
| Groundedness | Support from retrieved or provided data |
| Consistency | Repeatable results for similar prompts |
| Hallucination rate | Percentage of unsupported claims |

A strong evaluation strategy tracks both model quality and hallucination risk.


### **6. Latency**
Time to respond.

### **7. Cost**
Tokens used.

### **8. Safety Violations**
Number of unsafe outputs.

Evaluation is multi‑dimensional — no single metric is enough.

---

# Summary
Evaluation is essential for building reliable LLM systems.  
It ensures:

- correctness  
- grounding  
- safety  
- consistency  
- production readiness  

Hallucinations cannot be eliminated, but they can be measured, controlled, and reduced through:

- better prompts  
- RAG  
- fine‑tuning  
- guardrails  
- retrieval optimization  
- continuous evaluation  

Evaluation is not optional —  
it is the backbone of AI Engineering.
