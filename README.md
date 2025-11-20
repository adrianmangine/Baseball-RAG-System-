# Baseball-RAG-System-

# MP3 – Baseball RAG: Conversational Baseball Analytics Assistant

This repo contains the materials for **Mini-Project #3: Real-World RAG Implementation** for the AI Mini-Project Series.

The project designs a **Retrieval-Augmented Generation (RAG)** system over open baseball statistics datasets (Lahman, Retrosheet, pybaseball) to let users ask **natural-language questions** about players, teams, and seasons.

---

## 🚀 Project Overview

**Goal:**  
Build a small but realistic RAG design that lets a user ask questions like:

- “Who had the highest WAR among AL pitchers in 2019?”
- “Compare the Yankees’ bullpen ERA in 2018 vs 2020.”
- “Show Mike Trout’s career OPS trend.”

The system uses structured stats and text summaries to generate answers, returning both numbers and short explanations with citations.

---

## 🧭 Domain & Use Case

- **Domain:** Baseball statistics and sports analytics
- **Use Case:** Conversational baseball analytics assistant over MLB stats
- **Users:**
  - Baseball fans and sports nerds
  - Students in sports analytics / data science classes
  - Journalists / bloggers who want quick stat pull + explanation

**Success Criteria (high level):**

- ≥ 80% correct answers on a small test set of queries
- ≤ 5 seconds latency per query
- Every answer includes a source citation (dataset + year/season)

Full details are in [`reports/capb_report.md`](reports/capb_report.md).

---

## 📊 Data & Constraints

**Datasets:**

- **Lahman Baseball Database** – core structured stats (players, teams, seasons)
- **Retrosheet** – play-by-play and game-level event data
- **pybaseball API** – access to modern/advanced stats and potentially live data

**Formats:** CSV, JSON, possibly TXT/MD for narrative summaries.  
**Scale:** ~200 MB total for initial MVP (manageable locally).

**Constraints:**

- Use **free / open-source** tooling and datasets
- Assume **local-only processing** for the MVP
- Latency target: **≤ 5s** per query
- Simple security: basic login / API key if deployed

---

## 🧱 RAG Architecture (MVP)

Minimal pipeline:

1. **Ingestion** – load Lahman + Retrosheet CSVs, optional pybaseball pulls
2. **Chunking** – group data by *player*, *team*, *season* (heading-based chunks)
3. **Embedding** – generate embeddings for textified “stat cards” and summaries
4. **Vector DB** – store embeddings in **Chroma** with metadata (player, year, team)
5. **Retrieval** – hybrid search (metadata filters + vector similarity)
6. **LLM** – hosted GPT-style model to synthesize a natural-language answer
7. **Answer & Citations** – final response with stats + where they came from

Example stack (conceptual):

- **Framework:** LangChain / LlamaIndex (not strictly required, but realistic)
- **Embeddings:** OpenAI `text-embedding-3-small`
- **Vector Store:** Chroma
- **LLM:** Hosted GPT-4 / GPT-5.1
- **Orchestration:** Simple Python script or notebook

Detailed YAML and rationale are in the CAPB report.

---

## ⚙️ Component Choices & Bakeoff

**Vector DB:**

- **Option A:** FAISS
- **Option B:** Chroma ✅ (selected)

Chroma wins for this project due to:

- Easy Python integration
- Built-in persistence and metadata filters
- Good fit for a small, local academic project

**Reranker:**

- **Option A:** None ✅ (selected)
- **Option B:** Cohere Rerank

For this mini-project, the corpus is small enough that a dedicated reranker is not strictly necessary. The project prioritizes simplicity and time constraints over squeezing out extra retrieval quality.

Other alternatives and tradeoffs are documented in `capb_report.md`.

---

## 🧪 Evaluation Plan

- **Test Set:** ~15 questions spanning:
  - Single-player queries (career, per-season)
  - Team comparisons across seasons
  - “Leaders” queries (e.g., highest OPS in a given year)

**Metrics:**

- % Correct Answers
- % Answers with proper citations
- Average latency per query

Example target (baseline):

- 12 / 15 correct
- 12 / 15 with citations
- ~3–4 seconds average latency on a local machine

---

## ⚠️ Risks & Future Work

**Key risks & edge cases:**

- Ambiguous player names (multiple players named “Smith”)
- Missing / incomplete data (very early seasons, pre-1900)
- LLM hallucinations for stats not in the dataset
- Data drift / season updates if live data is included

**Future improvements:**

- Add a reranker for ambiguous or complex queries
- Integrate live Statcast / advanced metrics more deeply
- Expand to minor leagues or college baseball
- Explore GraphRAG-style reasoning over narratives:
  - Example: “How did roster changes affect team performance year to year?”

---

## 📚 Files in This Repo

- `README.md` – You are here.
- `reports/capb_report.md` – The completed CAPB mini-project report.
- `reports/rubric.md` – The project rubric for reference and self-check.

This repo is meant as a **documentation + design artifact** for MP#3, and can be used as a starting point for a full implementation later.
