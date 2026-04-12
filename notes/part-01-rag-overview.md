# Part 1: Retrieval-Augmented Generation (RAG)

Notes from introductory slides on RAG: what it is, why it matters, and how a typical system is built.

---

## What is RAG?

**Retrieval-Augmented Generation (RAG)** is a process that improves what a Large Language Model (LLM) says by letting it **look up** an authoritative, external knowledge base **before** it answers.

- It extends an LLM to **specific domains** or **your own data** without retraining the base model.
- **Benefits:** Often cheaper than full fine-tuning, and helps answers stay **relevant**, **accurate**, and **useful** in a given context.

---

## Why is RAG needed? (Limits of a plain LLM)

A standard LLM answers from its **training** (billions of parameters, e.g. GPT-style models). That alone leads to:

| Issue | What goes wrong |
|--------|------------------|
| **Hallucinations** | The model can sound sure while stating things that are wrong. |
| **Stale knowledge** | It only “knows” up to its **training cutoff**; no built-in access to new or live data. |
| **No private data** | It cannot see your **internal** docs (policies, HR, finance, product specs, etc.). |

RAG addresses these by **grounding** generation in retrieved, user-chosen sources.

---

## The RAG architecture

A typical RAG setup has **two main pipelines**.

### 1. Data ingestion pipeline

Prepares your data so the system can **search** it semantically.

| Step | Role |
|------|------|
| **Data sources** | Collect raw content (PDFs, HTML, Excel, SQL exports, etc.). |
| **Parsing & chunking** | Split text into smaller **chunks** that are easy to embed and retrieve. |
| **Embedding model** | Turn each chunk into a **vector** (numeric representation of meaning). Same family of models as for the query later. |
| **Vector database** | Store vectors (and links back to text). This becomes your **retrieval knowledge base**. |

### 2. Retrieval & generation pipeline

Handles each **user question** end-to-end.

| Step | Role |
|------|------|
| **User query** | The user asks a question in natural language. |
| **Query embedding** | Encode the query with the **same** embedding model → query vector. |
| **Retriever (similarity search)** | Find chunks in the vector DB whose vectors are **most similar** to the query vector. |
| **Context + prompt** | Pack the top chunks as **context**, add the user query and system instructions → full prompt for the LLM. |
| **Generation** | The LLM produces an answer **conditioned on** that context—more grounded than pure parametric knowledge. |

**Example product:** Perplexity-style search + answer flows use this general idea.

---

## Quick recap

1. **Ingest:** documents → chunks → embeddings → vector store.  
2. **Answer:** query → embedding → retrieve similar chunks → LLM answers using those chunks as context.

Next parts can go deeper on chunking, embeddings, vector DBs, evaluation, and production pitfalls.
