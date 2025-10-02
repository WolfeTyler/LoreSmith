# LoreSmith 🔮  
*An AI-powered semantic search assistant for Dungeons & Dragons 5e lore*

---

## 📖 Introduction
**LoreSmith** is an AI-driven semantic search and retrieval system that helps Dungeon Masters (DMs) and players quickly uncover rules, spells, and monsters from the **Dungeons & Dragons 5e System Reference Document (SRD)**.

Traditional keyword searches often fail in fast-paced sessions. LoreSmith solves this by combining **embeddings, hybrid search, reranking, and generative summarization** to provide **fast, accurate, and citation-grounded answers**.

---

## 🎯 Objectives
- Build a multi-layer **RAG pipeline** (embedding, search, generative).  
- Use public datasets of **D&D 5e spells and monsters**.  
- Support natural language queries (e.g., *“Which monsters are the strongest?”*).  
- Provide structured, user-friendly answers (tables, bullet summaries).  
- Showcase both **retrieval evidence** and **final AI-generated synthesis**.  

---

## 🏗️ System Design

LoreSmith is designed around a **three-layer architecture**:

### 1. Embedding Layer
- Embeds chunks of SRD text using **SentenceTransformers** (`all-MiniLM-L6-v2`).  
- Metadata enriched: `doc_type`, `CR`, spell level, resistances, immunities, environments.  
- Stored in **ChromaDB** as the vector index.  

### 2. Search Layer
- **BM25 keyword search** for lexical grounding.  
- **Chroma vector search** for semantic recall.  
- **Fusion + optional reranking** using a cross-encoder.  
- **Structured monster fallback**:
  - Detects monster queries explicitly.
  - Filters by **CR range, resistances, immunities, environments**.  
  - Expands synonyms (e.g., *swamps → swamp, bog, marsh*).  

### 3. Generative Layer
- Retrieved chunks are passed to **OpenAI GPT models**.  
- Prompt engineering enforces *“Answer only from context”*.  
- Produces:
  - Narrative summaries.  
  - Bullet points.  
  - Markdown tables (e.g., CR, resistances, environments).  

---

## ⚙️ Implementation

- **Environment**: Jupyter Notebook (Python 3.12, Anaconda).  
- **Libraries**:  
  - `chromadb` – persistent vector DB  
  - `sentence-transformers` – embeddings  
  - `rank_bm25` – BM25 lexical retrieval  
  - `openai` – LLM generation  
  - `ipywidgets`, `IPython.display` – styled notebook outputs  

- **Datasets**:  
  - [D&D 5e Spells CSV](https://github.com/wjsutton/games_night_viz/blob/main/challenges/9_bonus_challenges/202206_datafamcon_dnd/dnd-spells.csv)  
  - [D&D 5e Monsters CSV](https://github.com/wjsutton/games_night_viz/blob/main/challenges/9_bonus_challenges/202206_datafamcon_dnd/dnd_monsters.csv)  

- **Output Style**:  
  - Two panels per query:  
    1. 🔎 **Search Layer — Top 3** (retrieval evidence).  
    2. ✨ **Generation Layer — Final Answer** (synthesized output).  

---

## 🔍 Example Queries & Outputs

### Query 1: *“Is there a swamps monster?”*  
- **Search Layer**: Retrieved *Nilbog* and *Boggle*.  
- **Generation Layer**: Summarized traits, CR, and swamp environments.  

### Query 2: *“Which monsters are the strongest?”*  
- **Search Layer**: Retrieved *Tarrasque, Tiamat, Demogorgon*.  
- **Generation Layer**: Explained legendary resistances and provided CR table.  

### Query 3: *“What are the most interesting spells?”*  
- **Search Layer**: Retrieved *Find the Path, Prismatic Wall, Identify*.  
- **Generation Layer**: Summarized effects in structured Markdown table.  

*(See `/screenshots/` for Search + Generation panels.)*

---

## 🚧 Challenges
- **Library conflicts**: Needed `tf-keras` to resolve incompatibility between `Transformers` and Keras 3.  
- **Metadata issues**: ChromaDB required flattening list fields (resistances, immunities).  
- **Sparse monster retrieval**: Initially over-retrieved spells. Fixed with intent detection + structured fallbacks.  
- **LLM hallucinations**: Controlled via grounding prompts and explicit instructions.  

---

## 📚 Lessons Learned
- Hybrid retrieval (BM25 + embeddings) outperforms single-method approaches.  
- Metadata-driven filtering (CR, environment, resistances) is crucial for niche datasets.  
- Prompt engineering is essential to prevent LLM hallucinations.  
- Transparent outputs (showing retrieval + synthesis) build trust in generative systems.  

---

## ✅ Conclusion
*LoreSmith* demonstrates how **RAG pipelines** can be applied to a fantasy RPG domain, making the complex **D&D 5e SRD** accessible through natural language. Dungeon Masters and players can now quickly query monsters, spells, and rules with structured, accurate, and context-grounded answers.  

---

## 📎 Appendix
- 📂 **Notebook**: [`LoreSmith.ipynb`](./LoreSmith.ipynb)  
- 🖼️ **Screenshots**: See `/screenshots/` folder for all evaluation queries.  

---

## 🧙 Credits
- Datasets: [Kaggle D&D 5e Data](https://www.kaggle.com/)  
- Embeddings: [SentenceTransformers](https://www.sbert.net/)  
- Vector Store: [ChromaDB](https://www.trychroma.com/)  
- Generation: [OpenAI GPT](https://openai.com/)  

---

