# 🕉️ ScriptureRAG: Contextual Reflection from the Ashtadasha Mahapuranas

[![Live Application](https://img.shields.io/badge/Live-Streamlit_App-FF4B4B?style=for-the-badge&logo=streamlit)](https://scripturerag.streamlit.app/)
[![Python](https://img.shields.io/badge/Python-3.10%2B-blue?style=for-the-badge&logo=python)](#)
[![Gemini](https://img.shields.io/badge/LLM-Google_Gemini_Flash-8E75B2?style=for-the-badge&logo=google)](#)
[![ChromaDB](https://img.shields.io/badge/Vector_DB-ChromaDB-FD663F?style=for-the-badge)](#)

ScriptureRAG is a specialized Retrieval-Augmented Generation (RAG) system designed to provide philosophically grounded, historically accurate, and exact-cited responses from the 18 Ashtadasha Mahapuranas. It solves the critical issue of general-purpose LLMs hallucinating Sanskrit verses and fabricating scriptural citations.

## 🚀 Key Innovations

*   **Zero-Hallucination Architecture:** Grounded entirely in 10,582 curated passages from 123 unabridged Motilal Banarsidass (MLBD) translation volumes.
*   **Dual-Layer Guardrails:** A <1ms regex layer and a semantic LLM classifier intercept prompt injections, coding queries, and jailbreaks without consuming API budget.
*   **Split-Binary DB Stitching:** Bypasses GitHub's 100 MB file limit by splitting the 236 MB ChromaDB SQLite database into 6 binary files, stitched in-memory (< 1 sec) upon application startup.
*   **Dynamic Length Adaptation:** Automatically scales responses from crisp 5-7 line summaries to detailed 3-part treatises based on natural prompt intent (no UI toggles needed).
*   **RAGAS Mathematical Validation:** Appends an audit badge verifying that generated claims are strictly supported by the retrieved context (Faithfulness F >= 0.85).

## 🧠 End-to-End Architecture

1.  **Input Guardrails:** Filters out non-philosophical/malicious queries.
2.  **Canonical Query Expansion:** Maps colloquial English to classical Sanskrit terminology (e.g., "fear of death" → "Yama, Garuda Purana, Moksha").
3.  **Dense Vector Retrieval:** `BAAI/bge-base-en-v1.5` embeddings retrieve candidates from ChromaDB via HNSW Cosine Similarity.
4.  **Cross-Encoder Re-Ranking:** `ms-marco-MiniLM-L-6-v2` performs token-level cross-attention to refine the Top-6 candidates down to the Top-3 most faithful passages.
5.  **Grounded LLM Generation:** Google Gemini Flash (Temperature = 0.2) synthesizes the response.
6.  **OCR Sanitization & Validation:** Strips historical scanning artifacts and validates exact page/canto citations.

## 🛠️ Technology Stack

| Component | Technology | Description |
| :--- | :--- | :--- |
| **Frontend UI** | Streamlit + Custom CSS | ChatGPT-style dark theme (#212121), HTML5 Web Speech API |
| **Vector DB** | ChromaDB | Persistent local vector store with HNSW indexing |
| **Embeddings** | BAAI/bge-base-en-v1.5 | 768-dimensional dense vector embeddings |
| **Re-ranker** | ms-marco-MiniLM-L-6-v2 | Token-level cross-attention scoring |
| **Generative Engine** | Google Gemini Flash | Low-latency, high-quota grounded generation |

## 💻 Local Installation

```bash
# 1. Clone the repository
git clone [https://github.com/your-username/ScriptureRAG.git](https://github.com/your-username/ScriptureRAG.git)
cd ScriptureRAG

# 2. Create and activate a virtual environment
python -m venv venv
source venv/bin/activate  # On Windows use: venv\Scripts\activate

# 3. Install dependencies
pip install -r requirements.txt

# 4. Set up environment variables
# Create a .env file and add your Google Gemini API key:
# GEMINI_API_KEY="your_api_key_here"

# 5. Run the application
streamlit run app.py
