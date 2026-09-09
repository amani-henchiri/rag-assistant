# 🤖 RAG Assistant — Document-Grounded Q&A with Qwen

> End-to-end Retrieval-Augmented Generation (RAG) pipeline built during a live workshop

---

## Overview

This project implements a **RAG (Retrieval-Augmented Generation)** assistant that answers questions strictly based on provided documents no hallucination, no invented answers.

Built during the **Objectif IA workshop** organized by [Machine Learnia](https://www.machinelearnia.com/) (September 2026), then completed and deployed independently.

The assistant is grounded on PDF guides from a fictional fitness club chain (**Club Odyssée**) — chosen precisely because the LLM has no prior knowledge of it, making it easy to verify whether answers come from the documents or from the model's imagination.

---

## The 4 Bricks of a Production RAG System

### 1. Document Loading & Chunking
- Download PDFs directly from the web (6 guides, ~25 pages total)
- Split documents into overlapping passages (~500 characters) with sentence-aware cutting
- Each chunk keeps track of its source document

### 2. Semantic Search Engine
- Encode all passages into 768-dimensional vectors using **`paraphrase-multilingual-MiniLM-L12-v2`**
- At query time: encode the question and find the most similar passages via cosine similarity
- Works on **meaning**, not keywords — e.g. "stop my subscription" correctly retrieves passages about "résiliation"

### 3. Constrained Language Model
- Uses **Qwen2.5** (0.5B or 1.5B depending on GPU availability)
- The model receives only the retrieved passages as context
- Strict instruction: answer from documents only, or say *"Je ne sais pas, il faut demander à l'accueil"*
- Demonstrates the difference between a model with and without RAG on the same question
---

## Tech Stack

![Python](https://img.shields.io/badge/Python-3.10-blue?logo=python)
![HuggingFace](https://img.shields.io/badge/HuggingFace-Transformers-yellow?logo=huggingface)
![Gradio](https://img.shields.io/badge/Gradio-4.x-orange)
![SentenceTransformers](https://img.shields.io/badge/SentenceTransformers-3.x-green)

- **LLM:** Qwen2.5-1.5B-Instruct / Qwen2.5-0.5B-Instruct (HuggingFace)
- **Embeddings:** `sentence-transformers/paraphrase-multilingual-MiniLM-L12-v2`
- **PDF parsing:** pypdf
- **Deployment:** Gradio (public URL via `share=True`)
- **Platform:** Google Colab (T4 GPU)

---

## How to Run

### Google Colab (recommended)

1. Open `Objectif_IA_Atelier_Code_03_09_2026.ipynb` in Colab
2. Set runtime to **T4 GPU** if available
3. Run all cells in order
4. Copy the public Gradio URL from the output of the last cell

---

## Key Takeaway

The same model, the same role prompt — but two completely different answers depending on whether documents are provided.

**Without RAG:** the model hallucinated or refused to answer (Club Odyssée doesn't exist in its training data).  
**With RAG:** precise, sourced answers grounded in the actual documents.

That's the entire point of RAG in one experiment.

---

## Project Structure

```
rag-assistant/
├── RAG_Document_Assistant.ipynb   # Full notebook
├── requirements.txt                              # Python dependencies
├── .gitignore                                    # Files excluded from Git
└── README.md                                     # This file
```
