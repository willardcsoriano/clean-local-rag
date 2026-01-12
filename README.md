# Clean Local RAG 🧹🤖

**Forked from:** [AllAboutAI-YT/easy-local-rag](https://github.com/AllAboutAI-YT/easy-local-rag)

**Maintained by:** [willardcsoriano](https://www.google.com/search?q=https://github.com/willardcsoriano)

This repository provides a 100% local **Retrieval-Augmented Generation (RAG)** system. While based on the original "Easy Local RAG," this fork introduces an optimized pipeline designed to handle "dirty" text extraction from PDFs—solving the fragmented chunking issues that often break standard RAG performance.

## 🌟 Key Improvements in this Fork

### 1. Advanced Text Preprocessing (`myrag.py`)

Standard RAG often fails because it treats every line of a PDF as useful data. My updated script, `myrag.py`, uses logic from the [pdf-to-txt-tool](https://github.com/willardcsoriano/pdf-to-txt-tool) to ensure only "clean" data enters your vector store.

* **Semantic Chunking:** Unlike the original `localrag.py` which often reads line-by-line, `myrag.py` uses a sliding-window chunking method with a `CHUNK_SIZE` of 1200 characters and a `CHUNK_OVERLAP` of 200. This ensures sentences aren't cut in half, preserving context for the AI.
* **Noise Suppression:** Automatically filters out non-printable characters, repeated whitespace, and common PDF "noise" (like calendar patterns and page headers) that confuse vector embeddings.
* **Persistent Vector Store:** Saves your embeddings to `embeddings.json` so you only have to process your documents once.

### 2. Comparison: `localrag.py` vs. `myrag.py`

| Feature | `localrag.py` (Original) | `myrag.py` (Improved) |
| --- | --- | --- |
| **Ingestion** | Reads `vault.txt` line-by-line. | Semantic chunking with overlap. |
| **Data Cleaning** | Minimal / None. | Regex-based noise & control-char removal. |
| **Memory** | Re-embeds everything on every run. | Loads from persistent `embeddings.json`. |
| **Context** | 3 relevant lines. | 7 high-context chunks (`TOP_K = 7`). |

## 🛠️ Requirements & Setup

1. **Install Ollama:** Download and install from [ollama.com](https://ollama.com/).
2. **Pull the Models:**
```bash
ollama pull llama3
ollama pull mxbai-embed-large

```


3. **Install Dependencies:**
```bash
pip install torch ollama openai

```



## 🚀 How to Use

### Step 1: Prepare your Vault

Place your raw text into a file named `vault.txt`. For the best results, use the [pdf-to-txt-tool](https://github.com/willardcsoriano/pdf-to-txt-tool) to convert your PDFs into a clean, RAG-ready format first.

### Step 2: Run the RAG

Run the improved script to build your vector store and start chatting:

```bash
python myrag.py --model llama3

```

*The first run will generate embeddings and save them to `embeddings.json`. Subsequence runs will load almost instantly.*

## 📁 Repository Structure

* `myrag.py`: The improved, high-context RAG script with semantic chunking and persistence.
* `localrag.py`: The original line-by-line RAG script from the upstream repo.
* `vault.txt`: Your knowledge base (put your text here).
* `embeddings.json`: The generated vector database (created automatically).

---

## Credits

* **Original Project:** Kris at [All About AI](https://github.com/AllAboutAI-YT).
* **Optimization & Cleanup:** [Willard Soriano](https://www.google.com/search?q=https://github.com/willardcsoriano).

---
