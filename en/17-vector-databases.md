# 17 — Vector Databases

## Introduction
Vector databases are the backbone of modern AI systems.  
They enable:

- semantic search  
- retrieval‑augmented generation (RAG)  
- long‑term memory for agents  
- personalization  
- recommendation systems  
- similarity search  
- clustering and classification  

A vector database stores embeddings — high‑dimensional vectors that represent meaning — and retrieves the most relevant items efficiently.

Without vector databases, LLM systems cannot scale beyond the context window.

---

# Part 1 — Why Vector Databases Exist

Traditional databases store:

- strings  
- numbers  
- structured data  

But LLMs operate on **vectors**, not text.

Vector DBs solve three fundamental problems:

### **1. Semantic Search**
Find items based on meaning, not keywords.

### **2. High‑Dimensional Indexing**
Efficiently search millions or billions of vectors.

### **3. Fast Approximate Nearest Neighbor (ANN) Search**
Return the closest vectors in milliseconds.

Vector DBs are essential for RAG, agents, and memory systems.

---

# Part 2 — Core Concepts

## **1. Embeddings**
Vectors representing text, images, audio, or code.

## **2. Vector Space**
A high‑dimensional space (e.g., 768D, 1024D, 4096D).

## **3. Similarity Metrics**
Common metrics:
- cosine similarity  
- dot product  
- Euclidean distance  

## **4. ANN Search**
Approximate Nearest Neighbor search trades perfect accuracy for speed.

---

# Part 3 — Index Types

Vector DBs use specialized index structures.

## **1. Flat Index (Brute Force)**
- exact search  
- slow for large datasets  
- used for small collections  

## **2. HNSW (Hierarchical Navigable Small World)**
Most popular ANN index.

Benefits:
- extremely fast  
- high recall  
- scalable  

Used by:
- Pinecone  
- Weaviate  
- Milvus  
- FAISS  

## **3. IVF (Inverted File Index)**
Cluster‑based indexing.

Benefits:
- good for large datasets  
- supports quantization  

## **4. PQ / OPQ (Product Quantization)**
Compress vectors to reduce memory.

Used for:
- billion‑scale datasets  
- low‑memory environments  

---

# Part 4 — Metadata & Hybrid Search

Vector search alone is not enough.

Metadata enables:
- filtering  
- faceted search  
- structured constraints  
- domain‑specific retrieval  

Hybrid search combines:
- vector search  
- keyword search (BM25)  
- metadata filters  

This dramatically improves retrieval quality.

---

# Part 5 — Vector Database Architecture

A production‑grade vector DB includes:

### **1. Storage Layer**
Stores:
- vectors  
- metadata  
- raw text  

### **2. Index Layer**
HNSW, IVF, PQ, etc.

### **3. Query Layer**
Handles:
- similarity search  
- filtering  
- hybrid search  

### **4. Ingestion Pipeline**
Handles:
- embedding  
- chunking  
- metadata extraction  
- indexing  

### **5. Replication & Sharding**
For scaling and reliability.

### **6. Monitoring**
Tracks:
- latency  
- recall  
- index size  
- memory usage  

---

# Part 6 — Retrieval Quality

Retrieval quality determines RAG quality.

## Key Metrics

### **1. Recall**
Did we retrieve all relevant chunks?

### **2. Precision**
Are retrieved chunks relevant?

### **3. MRR (Mean Reciprocal Rank)**
Measures ranking quality.

### **4. Groundedness**
Does the answer rely on retrieved context?

### **5. Diversity**
Avoid retrieving near‑duplicate chunks.

---

# Part 7 — Scaling Vector Databases

Large datasets require:

### **1. Sharding**
Split vectors across nodes.

### **2. Replication**
Increase availability and read throughput.

### **3. Distributed Indexing**
Parallelize index building.

### **4. Incremental Updates**
Re‑index only changed documents.

### **5. Vector Compression**
Use PQ/OPQ to reduce memory.

### **6. Cold vs Hot Storage**
Hot = frequently accessed  
Cold = archived  

---

# Part 8 — Vector Drift & Versioning

Embeddings change when:
- the embedding model changes  
- chunking changes  
- cleaning improves  
- metadata updates  

This causes **vector drift**.

Mitigation:
- version embeddings  
- version indexes  
- maintain migration pipelines  
- re‑embed periodically  

---

# Part 9 — Choosing a Vector Database

Popular options:

- **FAISS** (local, fast, flexible)  
- **Pinecone** (managed, scalable)  
- **Weaviate** (hybrid search, metadata‑rich)  
- **Milvus** (open‑source, distributed)  
- **Chroma** (simple, local‑first)  

Selection criteria:
- dataset size  
- latency requirements  
- metadata needs  
- hybrid search  
- cost  
- scaling requirements  

---

# Part 10 — Best Practices

### **1. Use metadata aggressively**
Improves precision dramatically.

### **2. Use hybrid search**
Vector + keyword > vector alone.

### **3. Use re‑ranking**
Cross‑encoders improve retrieval quality.

### **4. Avoid huge chunks**
Large chunks → low precision.

### **5. Monitor recall**
Retrieval failures = hallucinations.

### **6. Version everything**
Embeddings, indexes, metadata.

### **7. Rebuild indexes periodically**
Indexes degrade over time.

---

# Summary
Vector databases are the foundation of modern AI systems.  
They enable:

- semantic search  
- RAG  
- memory  
- agents  
- personalization  
- large‑scale retrieval  

A strong vector database architecture makes LLM systems:

- accurate  
- scalable  
- fast  
- reliable  
- production‑ready  

Vector DBs are not optional —  
they are essential for real AI engineering.
