# 15 — Data Engineering for AI Systems

> Tags: `#DataEngineering` `#RAG` `#VectorDB` `#AIInfrastructure`

## Introduction
LLM systems are only as good as the data they operate on.  
Whether you are building RAG, agents, search, or fine-tuning pipelines, **data engineering is the backbone** of every AI system.

Traditional software engineering focuses on logic.  
AI engineering focuses on **data quality, structure, and retrieval**.

This chapter explains how to design data pipelines that make LLM systems:

- accurate
- reliable
- scalable
- efficient
- production-ready

---

# Part 1 — Why Data Engineering Matters in AI

LLMs do not “know” your domain.  
They rely on:

- retrieved documents
- embeddings
- chunking
- metadata
- indexing
- context assembly

If your data pipeline is weak, your entire AI system collapses.

| Symptom | What it indicates |
| --- | --- |
| Irrelevant retrieval | Bad index or weak embeddings |
| Hallucinations | Missing grounding or poor context |
| Missing context | Incomplete ingestion or chunking |
| Noisy chunks | Poor cleaning or chunk strategy |
| Slow vector search | Inefficient index or hardware |
| Inconsistent answers | Unstable retrieval or prompt logic |

Data engineering is not optional — it is the foundation of AI system design.

---

# Part 2 — Document Ingestion Pipeline

A robust ingestion pipeline includes four stages.

| Stage | What it does | Example outputs |
| --- | --- | --- |
| Document collection | Gather source files and feeds | PDFs, HTML, KBs, emails, logs |
| Normalization | Convert content into clean text | stripped HTML, fixed encoding |
| Cleaning | Remove junk and duplicate content | no boilerplate, no menus |
| Metadata extraction | Add searchable labels and references | title, author, date, tags |

Metadata is as important as the text itself.

Example ingestion flow:

```python
for doc in source_files:
    text = extract_text(doc)
    text = normalize_text(text)
    text = remove_noise(text)
    metadata = extract_metadata(doc)
    save_document(text, metadata)
```

---

# Part 3 — Chunking Strategies

Chunking is one of the most important decisions in RAG.

| Approach | Pros | Cons |
| --- | --- | --- |
| Fixed-size chunking | Simple and fast | May split meaning across chunks |
| Semantic chunking | Keeps ideas intact | More expensive to compute |
| Overlapping chunks | Preserves context | Increases storage and redundancy |
| Hierarchical chunking | Flexible retrieval at multiple scales | Complex to implement |

Chunk size affects retrieval precision, recall, context usage, hallucination rate, and grounding quality.

Example semantic chunking pipeline:

```python
sections = split_by_heading(document)
chunks = []
for section in sections:
    chunks.extend(split_by_semantic_boundaries(section))
```

---

# Part 4 — Embedding Pipelines

Embedding pipelines convert chunks into vectors.

| Decision | Why it matters |
| --- | --- |
| Embedding model | Domain-specific models can improve relevance |
| Batch size | Larger batches reduce overhead |
| Parallelization | Distribute work across workers |
| Incremental updates | Recompute only changed documents |
| Versioning | Track model and data changes |

Embedding pipelines must be reproducible and scalable.

Example incremental embedding update:

```python
for chunk in changed_chunks:
    vector = embed(chunk.text)
    upsert_vector(chunk.id, vector, chunk.metadata)
```

---

# Part 5 — Indexing & Vector Databases

A vector database stores embeddings for fast retrieval.

| Index type | Best use case |
| --- | --- |
| Flat index | Small datasets or exact search |
| HNSW | Fast approximate search at scale |
| IVF | Large datasets with clustering |
| Hybrid index | Combine vector and keyword search |

Best practices:

- store metadata
- store chunk text
- store source references
- use filters to narrow search
- monitor index size
- reindex periodically

Example index insertion:

```python
index.add(
    ids=[chunk.id],
    vectors=[vector],
    metadata=[chunk.metadata]
)
```

---

# Part 6 — Retrieval Optimization

Retrieval quality determines answer quality.

| Technique | What it improves |
| --- | --- |
| Hybrid retrieval | Relevance and recall together |
| Re-ranking | Better ordering of top results |
| Multi-vector retrieval | Captures different aspects of chunks |
| Query expansion | Improves recall for broad queries |
| Metadata filtering | Narrow by type, date, or source |
| Context assembly | Choose top-k and max-token limits |

Retrieval is a system, not a single step.

Example retrieval workflow:

```python
results = vector_search(query_vector, top_k=20)
results = rerank_with_cross_encoder(query, results)
context = assemble_context(results, max_tokens=1200)
```

---

# Part 7 — Data Quality Metrics

Evaluate your data pipeline with measurable signals.

| Metric | What it measures |
| --- | --- |
| Coverage | How much relevant content is included |
| Noise | Amount of irrelevant or junk text |
| Redundancy | Duplicate or repeated content |
| Chunk quality | Semantic coherence of chunks |
| Metadata quality | Accuracy and completeness of labels |
| Retrieval precision | Relevance of returned chunks |
| Retrieval recall | Coverage of all relevant chunks |

Data quality directly affects hallucination rate and downstream reliability.

---

# Part 8 — Data Governance & Versioning

AI systems require strong governance.

| Requirement | Why it matters |
| --- | --- |
| Dataset versioning | Reproduce results and roll back changes |
| Embedding versioning | Know which model produced the vectors |
| Index versioning | Validate retrieval behavior over time |
| Document lineage | Track source and update provenance |
| Audit logs | Diagnose errors and compliance issues |

Without governance, debugging becomes impossible.

---

# Part 9 — Scaling Data Pipelines

Large datasets require scalable infrastructure.

| Challenge | Scaling strategy |
| --- | --- |
| Embedding large corpora | Batch and distributed embedding |
| Index growth | Sharding and partitioning |
| Update frequency | Incremental ingestion and reindexing |
| Ingestion load | Background jobs and stream processing |

Data pipelines must scale with the system.

---

# Summary
Data engineering is the foundation of AI systems.  
It includes:

- ingestion
- cleaning
- chunking
- embedding
- indexing
- retrieval
- metadata
- governance

A strong data pipeline makes LLM systems:

- accurate
- grounded
- scalable
- reliable
- production-ready

Data engineering is not a support function —  
it is a core skill of every AI Engineer.
