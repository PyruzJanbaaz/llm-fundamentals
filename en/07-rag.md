# 07 — Retrieval‑Augmented Generation (RAG)

> Tags: `#RAG` `#Retrieval` `#Embeddings` `#Grounding` `#LLM`

## Introduction
Large Language Models are powerful, but they have a fundamental limitation: they cannot access information outside their context window or training data.

RAG (Retrieval‑Augmented Generation) solves this problem by combining retrieval, embeddings, and grounded generation.

| Component | Role |
| --- | --- |
| Retrieval | Finds relevant external knowledge |
| Embeddings | Converts text and queries into semantic vectors |
| Generation | Produces grounded answers using retrieved context |
| Grounding | Constrains the model to real data |

RAG transforms an LLM from a closed‑book model into an **open‑book reasoning system**.

---

## Why RAG Exists
LLMs have several constraints that limit real-world use.

| Constraint | How RAG solves it |
| --- | --- |
| Cannot store all knowledge | Retrieves external data on demand |
| Cannot update after training | Uses current documents and knowledge bases |
| Hallucinations when unsure | Grounds answers in retrieved context |
| No access to private data | Keeps domain data external |
| Context window limits | Injects only relevant chunks into the prompt |

RAG addresses these by giving the model access to **external knowledge** at inference time.

RAG = LLM + Retrieval + Embeddings + Grounding

---

## How RAG Works (High‑Level Overview)
A RAG pipeline has four main steps:

| Step | Description |
| --- | --- |
| **Chunking** | Split documents into small, meaningful pieces |
| **Embedding** | Convert each chunk into a semantic vector |
| **Retrieval** | Match the query vector to the closest document vectors |
| **Generation** | Use the retrieved chunks to produce a grounded answer |

This allows the model to answer questions using **real data**, not guesses.

Example retrieval pipeline:

```python
query = "What is the refund policy?"
query_vec = embed_model.encode(query)
results = vector_db.search(query_vec, top_k=5)
context = "\n\n".join([item.text for item in results])
prompt = f"Use only the context below to answer the question.\n\nContext:\n{context}\n\nQuestion:\n{query}"
answer = llm.generate(prompt)
```

---

## Step 1 — Chunking
Documents are too large to embed as a whole.  
Chunking splits them into manageable pieces (e.g., 200–500 tokens).

Good chunking:

- preserves meaning  
- avoids cutting sentences in half  
- keeps related ideas together  
- improves retrieval accuracy  

Chunking is a critical design decision in RAG systems.

---

## Step 2 — Embedding the Chunks
Each chunk is converted into an embedding vector.  
These vectors represent the semantic meaning of the text.

The embedding model determines:

- retrieval quality  
- semantic accuracy  
- robustness to paraphrasing  
- multilingual performance  

Better embeddings → better RAG.

---

## Step 3 — Retrieval
When the user asks a question:

1. The question is embedded  
2. The system searches for the closest document vectors  
3. The top‑k most relevant chunks are selected  

Retrieval methods include:

- vector search (FAISS, Pinecone, Chroma)  
- hybrid search (vector + keyword)  
- re‑ranking (cross‑encoders)  

Retrieval quality directly affects answer quality.

---

## Step 4 — Generation with Grounding
The retrieved chunks are injected into the LLM’s context window.  
The model is instructed to:

- use only the provided information  
- avoid hallucinating  
- cite sources if needed  
- answer concisely and accurately  

This turns the LLM into a **grounded reasoning engine**.

---

## Why RAG Reduces Hallucinations
Hallucinations happen when the model:

- lacks information  
- is forced to guess  
- relies on statistical patterns instead of facts  

RAG reduces hallucinations by:

- providing real data  
- constraining the model to retrieved context  
- grounding answers in external knowledge  

RAG does not eliminate hallucinations,  
but it significantly reduces them.

---

## RAG Architecture (Engineering View)

A typical RAG system includes:

| Component | Role |
| --- | --- |
| **Document Ingestion Pipeline** | Chunking, cleaning, metadata extraction |
| **Embedding Service** | Converts text into semantic vectors |
| **Vector Database** | Stores embeddings for fast retrieval |
| **Retriever** | Finds relevant chunks for a query |
| **Reranker (optional)** | Improves retrieval precision |
| **LLM Generator** | Produces grounded answers |
| **Guardrails** | Prevent hallucinations and enforce constraints |

RAG is not a single model — it is a **system architecture**.

---

## RAG vs Fine‑Tuning
RAG and fine‑tuning solve different problems.

| Use case | RAG | Fine-Tuning |
| --- | --- | --- |
| Changing knowledge | Excellent | Poor |
| Large data sources | Excellent | Expensive |
| Minimizing hallucinations | Good | Helpful with training |
| Private data | Keeps data external | Requires data in training set |
| New skills or behavior | Limited | Ideal |
| Style or tone adaptation | Harder | Ideal |

In practice, many production systems use **RAG + fine‑tuning** together.

---

## RAG in Enterprise Systems
RAG is essential for:

- internal knowledge bases  
- customer support  
- legal and compliance systems  
- financial analysis  
- medical and scientific retrieval  
- automotive diagnostics  
- software documentation search  
- enterprise chatbots  

RAG allows LLMs to operate safely in domains where accuracy is critical.

---

## Advanced RAG Techniques

| Technique | What it does | When to use it |
| --- | --- | --- |
| **Hybrid Retrieval** | Combines vector and keyword search | When precision and recall both matter |
| **Cross-Encoder Re-Ranking** | Re-ranks retrieved chunks for relevance | When retrieval quality is critical |
| **Multi-Vector Retrieval** | Stores multiple embeddings per chunk | When text has rich semantic meanings |
| **Query Expansion** | Rewrites the query for better matches | When queries are short or vague |
| **Context Compression** | Summarizes retrieved chunks | When context window is tight |
| **Hierarchical RAG** | Retrieves at document, section, and chunk levels | When documents are very large |

These techniques significantly improve RAG performance.

---

## Summary
RAG transforms LLMs into powerful, grounded reasoning systems by combining:

- retrieval  
- embeddings  
- vector search  
- context injection  
- generation  

RAG solves the fundamental limitations of LLMs:

- limited context  
- outdated knowledge  
- hallucinations  
- lack of domain‑specific data  

RAG is not optional —  
it is a core architecture for real‑world AI systems.
