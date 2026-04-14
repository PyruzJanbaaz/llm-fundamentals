# 03 — Transformer Architecture

> Tags: `#Transformer` `#Attention` `#DeepLearning` `#LLM`

## Introduction
The Transformer is the core architecture behind all modern LLMs.  
It replaced older sequence models such as RNNs and LSTMs by introducing a new idea:  
**self‑attention**, a mechanism that allows the model to look at all tokens at once and learn how they relate to each other.

This shift from sequential processing to parallel relational processing is what made large‑scale language modeling possible.

The Transformer is not just a neural network — it is a **scaling engine** that enables billion‑parameter models.

| Feature | Benefit |
| --- | --- |
| Parallel token processing | Faster training and inference |
| Self-attention | Global context and long-range dependencies |
| Layer stacking | Deep hierarchical understanding |
| Residual + normalization | Stable training at scale |

## Why the Transformer Was a Breakthrough
Before Transformers, models processed text one token at a time.  
This caused several problems:

| Problem | Transformer solution |
| --- | --- |
| Long-range dependencies were hard to learn | Attention connects any token to any other token |
| Training was slow and sequential | Parallel processing across tokens |
| Parallelization was limited | GPU-friendly matrix operations |
| Context understanding was shallow | Global token interaction and multi-head views |

This architecture is the foundation of GPT, Claude, Gemini, LLaMA, and all modern LLMs.

---

## The Core Components of a Transformer
A Transformer layer consists of four major components:

1. **Multi‑Head Self‑Attention**  
2. **Feed‑Forward Network (FFN)**  
3. **Residual Connections**  
4. **Layer Normalization**

These components are stacked dozens or hundreds of times to form a full model.

---

## Self‑Attention: The Heart of the Transformer
Self‑attention answers a simple question:

> “For this token, which other tokens matter?”

Each token is transformed into three vectors:

- **Query (Q)** — what this token is looking for  
- **Key (K)** — what this token offers  
- **Value (V)** — the information this token carries  

Attention scores are computed by comparing Q and K.  
These scores determine how much each token influences the others.

```python
import numpy as np

Q = np.random.randn(seq_len, d)
K = np.random.randn(seq_len, d)
V = np.random.randn(seq_len, d)

scores = Q @ K.T / np.sqrt(d)
weights = softmax(scores)
output = weights @ V
```

Self‑attention enables the model to:

- capture long‑range dependencies  
- understand sentence structure  
- resolve references  
- extract global meaning  

This is the mechanism that gives LLMs their deep contextual understanding.

---

## Multi‑Head Attention: Multiple Perspectives
One attention mechanism is not enough.  
Language contains many types of relationships:

- subject → verb  
- noun → adjective  
- entity → description  
- cause → effect  
- condition → result  

Multi‑Head Attention allows the model to learn **multiple types of relationships in parallel**.  
Each head focuses on a different pattern.  
When combined, they form a rich representation of the input.

---

## Positional Encoding: Understanding Order
Self‑attention alone does not understand order.  
If you shuffle the tokens, the attention mechanism cannot detect the difference.

Positional Encoding solves this by adding numerical patterns to token embeddings that represent:

- position  
- distance  
- sequence structure  

Without positional encoding, the Transformer would be order‑blind.

---

## Feed‑Forward Networks: Deepening the Representation
After attention, each token passes through a small neural network called a **Feed‑Forward Network (FFN)**.

The FFN:

- adds non‑linearity  
- refines semantic features  
- increases model capacity  
- transforms attention outputs into deeper representations  

Attention discovers relationships;  
the FFN strengthens and transforms them.

---

## Residual Connections and Layer Normalization
Transformers are deep networks.  
To stabilize training, each sub‑layer (attention and FFN) is wrapped with:

- **Residual connections** — preserve information and help gradients flow  
- **Layer Normalization** — stabilize activations and prevent divergence  

These components make it possible to train extremely deep and large models.

---

## Encoder vs Decoder
The original Transformer architecture (from *Attention Is All You Need*) has two parts:

| Component | Purpose | Example models |
| --- | --- | --- |
| **Encoder** | Encodes input sequences for understanding | BERT, RoBERTa |
| **Decoder** | Generates output tokens autoregressively | GPT, GPT-4 |
| **Encoder-Decoder** | Transforms input to output with cross-attention | T5, BART |

Modern LLMs like GPT use **only the decoder** with causal masking.  
Models like BERT use **only the encoder**.

---

## Causal Masking: Enforcing Autoregressive Behavior
LLMs must not look at future tokens when predicting the next one.  
Causal masking ensures that:

- token *t* can only attend to tokens ≤ *t*  
- the model behaves autoregressively  
- generation is consistent and valid  

This is essential for next‑token prediction.

---

## Stacking Layers: Depth Creates Intelligence
A single Transformer layer can only learn simple relationships.  
Stacking many layers allows the model to learn:

- syntax  
- semantics  
- reasoning patterns  
- world knowledge  
- abstract structure  

Depth is what transforms attention into intelligence‑like behavior.

---

## Why the Transformer Scales
The Transformer scales because:

| Scaling factor | Why it matters |
| --- | --- |
| Parallel attention | All tokens process simultaneously |
| GPU-friendly matrix math | Batch compute scales efficiently |
| Distributed training | Models can span many devices |
| Layer depth | More layers capture richer patterns |
| Model size | Larger models generalize better |

This scalability is what enabled the jump from small models to LLMs with billions of parameters.

---

## Summary
The Transformer architecture is the foundation of all modern LLMs.  
Its key innovations — self‑attention, multi‑head attention, positional encoding, FFNs, and residual connections — enable:

- deep contextual understanding  
- long‑range reasoning  
- scalable training  
- powerful generation  

The Transformer is not just a model architecture.  
It is the engine that made the LLM revolution possible.
