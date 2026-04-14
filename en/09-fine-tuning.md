# 09 — Fine‑Tuning

> Tags: `#FineTuning` `#LLM` `#LoRA` `#QLoRA` `#DomainAdaptation`

## Introduction
Fine‑tuning is the process of taking a pretrained LLM and training it further on a smaller, domain‑specific dataset.  
While pretraining teaches the model general language understanding, fine‑tuning teaches it:

- new skills  
- new behaviors  
- domain‑specific knowledge  
- task‑specific reasoning patterns  

Fine‑tuning transforms a general LLM into a specialized one.

| What fine-tuning changes | Why it matters |
| --- | --- |
| Model weights | Adjusts behavior and reasoning |
| Vocabulary and phrasing | Improves domain relevance |
| Task-specific patterns | Boosts accuracy on target tasks |
| Style and tone | Matches brand or team voice |

---

## Why Fine‑Tuning Exists
Pretrained LLMs are powerful, but they have limitations.

| Limitation | Fine-tuning benefit |
| --- | --- |
| Domain-specific terms | Learns specialized vocabulary |
| Specialized workflows | Adapts to custom processes |
| Brand tone or style | Matches desired voice |
| Technical hallucinations | Improves factual behavior |
| New skills | Teaches task-specific capabilities |

Fine‑tuning solves these problems by adapting the model to:

- a specific domain (finance, automotive, legal, medical)  
- a specific task (classification, extraction, reasoning)  
- a specific behavior (tone, style, persona)  

Fine‑tuning = adding new capabilities the base model does not have.

---

## Fine‑Tuning vs RAG vs Prompting
These three techniques solve different problems.

| Technique | What it changes | Ideal use case |
| --- | --- | --- |
| **Prompting** | No model change | Quick experimentation and flexible instructions |
| **RAG** | External knowledge source | Grounding answers with up-to-date or private data |
| **Fine‑Tuning** | Model behavior and weights | Specialized tasks, behavior, or domain knowledge |

In production systems, companies often use **all three together**.

---

## Types of Fine‑Tuning

| Method | What it changes | Pros | Cons | Common use cases |
| --- | --- | --- | --- | --- |
| **Full Fine‑Tuning** | All model parameters | Maximum control, highest performance | Expensive, large hardware, catastrophic forgetting risk | Large-scale enterprise models |
| **LoRA** | Small trainable matrices | Cheap, fast, efficient, preserves base model | Slightly lower capacity than full tuning | Most common commercial fine-tuning |
| **QLoRA** | Quantized LoRA | Runs on consumer GPUs, low memory | Still experimental for some models | Democratized low-cost fine-tuning |
| **Instruction Fine‑Tuning** | Behavior on tasks | Improves instruction following | Requires quality paired data | Chat assistants, agents, workflow models |
| **Domain Fine‑Tuning** | Domain knowledge | Better accuracy in specialized fields | Requires domain dataset | Medical, legal, finance, automotive |
| **Task‑Specific Fine‑Tuning** | Specific outputs | High task performance | Narrow scope | Classification, summarization, code generation |

---

## What Fine‑Tuning Can Teach
Fine‑tuning can add:

| Capability | Benefit |
| --- | --- |
| New vocabulary | Handles specialized terminology |
| New reasoning patterns | Improves task-specific logic |
| New workflows | Follows custom procedures |
| New domain knowledge | Increases accuracy in the field |
| New styles and tones | Matches brand or persona |
| Tool-usage behaviors | Uses APIs and internal tools effectively |

Fine‑tuning cannot:

| Limitation | Why it remains true |
| --- | --- |
| Fix fundamental model limitations | The base model architecture still applies |
| Replace missing training data | It cannot invent unseen facts reliably |
| Give the model real-time knowledge | It still lacks live access |
| Eliminate hallucinations entirely | Fine-tuning reduces but does not remove risk |

---

## Data for Fine‑Tuning
The quality of fine‑tuning depends on the dataset.

| Dataset quality | What it means |
| --- | --- |
| Clean | No typos, formatting errors, or noise |
| Consistent | Uniform labels, style, and instruction format |
| High-quality | Accurate, relevant, and well-written examples |
| Task-aligned | Matches the target use case closely |
| Diverse | Covers multiple edge cases and scenarios |
| Contradiction-free | Avoids conflicting examples |

Bad data → bad model.

Example dataset format:

```json
{
  "prompt": "Translate the following sentence into formal English.",
  "input": "Hey, can you send me the report?",
  "output": "Could you please provide the report?"
}
```

---

## Fine‑Tuning Pipeline (Engineering View)

| Stage | Description | Example output |
| --- | --- | --- |
| **Data Collection** | Gather domain documents, examples, transcripts | Raw text, CSV, JSONL |
| **Data Cleaning** | Remove noise, duplicates, contradictions | Cleaned dataset |
| **Data Formatting** | Convert to instruction-response pairs | Prompt-output examples |
| **Training** | Apply LoRA / QLoRA / full fine-tuning | Trained adapter or model weights |
| **Evaluation** | Measure accuracy, hallucination rate, robustness | Test reports, metrics |
| **Deployment** | Merge adapters, quantize, optimize | Production-ready model |

Fine‑tuning is a full ML lifecycle, not a single step.

Example training loop:

```python
for batch in dataset:
    outputs = model(batch.inputs)
    loss = loss_fn(outputs, batch.targets)
    loss.backward()
    optimizer.step()
    optimizer.zero_grad()
```

---

## When NOT to Fine‑Tune
Fine‑tuning is powerful, but often unnecessary.

| Situation | Better approach |
| --- | --- |
| Prompting alone solves the problem | Use prompt engineering |
| RAG can provide the needed knowledge | Use retrieval and grounding |
| The task is simple | Use zero-shot or few-shot prompts |
| The dataset is small or low-quality | Improve data quality first |
| The goal is to reduce hallucinations | Use RAG or guardrails |
| The model only needs formatting or style changes | Use prompt templates or post-processing |

Fine‑tuning should be used when it provides **clear, measurable value**.

---

## Fine‑Tuning in Enterprise Systems
Enterprises fine‑tune models for:

- customer support  
- legal document analysis  
- financial compliance  
- automotive diagnostics  
- insurance claims  
- medical triage  
- internal knowledge workflows  

Fine‑tuning allows companies to build **domain‑expert AI systems**.

---

## Risks and Challenges
Fine‑tuning introduces risks:

- catastrophic forgetting  
- overfitting  
- bias amplification  
- hallucination drift  
- poor generalization  
- data leakage  
- high compute cost  

Mitigation requires:

- careful dataset design  
- evaluation frameworks  
- guardrails  
- hybrid RAG + fine‑tuning architectures  

---

## Summary
Fine‑tuning adapts a pretrained LLM to new tasks, domains, and behaviors.  
It enables:

- new skills  
- new reasoning patterns  
- domain expertise  
- workflow alignment  

Fine‑tuning is not a replacement for prompting or RAG —  
it is a complementary technique that, when used correctly, creates powerful, specialized AI systems.
