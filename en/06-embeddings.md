# 06 — Embeddings

> Tags: `#Embeddings` `#SemanticSearch` `#RAG` `#Vectors`

## Introduction
LLMs cannot understand raw text. They operate on numbers — vectors — not words. Embeddings are the mechanism that transforms text into numerical representations that capture **meaning**, **relationships**, and **semantic structure**.

An embedding is not just a number. It is a **position in a high‑dimensional semantic space** where similar concepts are close together and different concepts are far apart.

Embeddings are the foundation of understanding in modern language models.

| What embeddings do | Why it matters |
| --- | --- |
| Represent meaning numerically | Enables semantic comparison |
| Encode relationships | Supports analogy and reasoning |
| Compress context | Powers RAG and retrieval |
| Connect modalities | Bridges text, images, audio, and code |

---

## What Is an Embedding?
An embedding is a vector — a list of numbers — that represents the meaning of a token, word, sentence, or document.

| Representation | Example |
| --- | --- |
| Token / word | `car`, `run`, `doctor` |
| Sentence | `The car is red.` |
| Document | Article, paragraph, code file |

Example:

```python
embedding = [0.12, -0.88, 0.44, 0.01, ...]
```

These numbers encode semantic information learned during training.

| Semantic pattern | What it means |
| --- | --- |
| car ≈ vehicle | Similar meaning |
| run ≈ running | Morphological relationship |
| doctor ≈ nurse | Related profession |
| car ≠ banana | Distant meanings |

This structure emerges naturally from large-scale training.

---

## Why Embeddings Matter
Embeddings give LLMs the ability to:

| Capability | Benefit |
| --- | --- |
| Measure similarity | Find related text and ideas |
| Understand relationships | Capture analogies and context |
| Cluster concepts | Group similar content automatically |
| Generalize across contexts | Apply knowledge to novel examples |
| Reason over meaning | Support semantic inference |
| Retrieve information | Power RAG and search systems |

Without embeddings, LLMs would be pattern-matching machines with no semantic awareness.

Embeddings are the **semantic backbone** of the entire model.

---

## How Embeddings Are Learned
During training, the model repeatedly predicts the next token. To do this effectively, it must learn:

| Training signal | What it teaches the model |
| --- | --- |
| Co-occurrence | Which words appear together |
| Syntactic role | Which words play similar grammatical roles |
| Context shifts | How meaning changes across usage |
| Global semantics | How concepts relate in a broader space |

These patterns are encoded into embedding vectors.

The model does not memorize definitions. It learns **statistical meaning** from usage.

This is why embeddings capture:

| Emergent property | Example |
| --- | --- |
| Synonyms | `car` ≈ `vehicle` |
| Analogies | king - man ≈ queen - woman |
| Relationships | doctor ≈ nurse |
| Hierarchies | dog ≈ animal |
| Latent structure | Themes and topics |

Example:

```python
import numpy as np

a = np.array([0.1, 0.3, 0.5])
b = np.array([0.2, 0.1, 0.7])
cosine_similarity = np.dot(a, b) / (np.linalg.norm(a) * np.linalg.norm(b))
print(cosine_similarity)
```

Embeddings often exhibit linear relationships such as:

```text
embedding("king") - embedding("man") ≈ embedding("queen") - embedding("woman")
```

This is not programmed — it emerges.

---

## Types of Embeddings

| Type | Purpose | Example |
| --- | --- | --- |
| **Token Embeddings** | Represent each token in the vocabulary | `apple`, `running` |
| **Positional Embeddings** | Encode token order in the sequence | position 1, position 2 |
| **Segment / Sentence Embeddings** | Distinguish different input segments | sentence A vs sentence B |
| **Document Embeddings** | Represent entire text chunks | article, paragraph, report |
| **Multimodal Embeddings** | Embed images, audio, code, and text together | image-text search |

Embeddings unify all modalities into a common numerical representation.

---

## Embedding Space: The Geometry of Meaning
Embeddings live in a high‑dimensional vector space.  
In this space:

- similar meanings cluster together  
- unrelated meanings are distant  
- analogies form geometric patterns  
- topics form regions  
- relationships form directions  

This space is what allows LLMs to:

- search semantically  
- reason implicitly  
- generalize beyond training data  

The embedding space is the model’s internal map of meaning.

---

## Embeddings in RAG Systems
Retrieval‑Augmented Generation (RAG) relies heavily on embeddings.

| Step | What happens | Why it matters |
| --- | --- | --- |
| 1 | Split documents into chunks | Keeps retrieval manageable |
| 2 | Convert each chunk into an embedding | Creates semantic vectors for search |
| 3 | Convert the user query into an embedding | Matches query meaning to documents |
| 4 | Find the closest chunks in embedding space | Retrieves relevant evidence |
| 5 | Feed them to the LLM | Grounds generation in real content |

Better embeddings → better retrieval → better answers.

Embedding quality directly determines RAG performance.

---

## Embeddings in AI Engineering
For AI engineers, embeddings affect:

| Area | Impact |
| --- | --- |
| Search quality | Finds semantically relevant results |
| Retrieval accuracy | Improves RAG and KB queries |
| Clustering / classification | Groups and labels content |
| Recommendations | Matches interests and needs |
| Agent memory | Stores and retrieves state efficiently |
| Context compression | Fits more meaning into limited windows |
| Long-context reasoning | Supports extended document workflows |
| Cost and latency | Depends on embedding dimensionality and search |

Embeddings are not just a model component — they are a **core system design element**.

---

## Summary
Embeddings transform text into meaning.  
They allow LLMs to:

- understand  
- compare  
- reason  
- retrieve  
- generalize  

Embeddings are the invisible structure that makes intelligence‑like behavior possible.  
They are the semantic foundation of all modern language models.
