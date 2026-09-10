# Pakistan Tax Navigator

A Streamlit MVP that helps salaried individuals, freelancers, IT/software exporters and small businesses navigate Pakistan tax-compliance questions.

## Architecture

- `app.py` — main Streamlit workflow and progressive UI
- `data_collection.py` — Step 1/2 segmentation and profile collection
- `processing.py` — RAG query workflow, LLM prompting and confidence scoring
- `rag_engine.py` — local document parsing, chunking, embeddings and FAISS retrieval
- `internet_search.py` — optional current-web fallback using Gemini Google Search grounding
- `ui.py` — styling and reusable UI components
- `ingest.py` — owner-only RAG indexing utility
- `config.py` — configuration, paths, model and thresholds
- `knowledge_base/` — owner-managed source documents only; no public upload feature
- `vectorstore/` — generated FAISS index and metadata; commit these generated files for Streamlit Cloud so the app does not need to rebuild the index on every deployment

## Local setup

1. Create a Python 3.10+ virtual environment.
2. Install dependencies:

```bash
pip install -r requirements.txt
```

3. Add your owner-managed FBR/Pakistan source documents to `knowledge_base/`.
4. Create `.streamlit/secrets.toml`:

```toml
GEMINI_API_KEY = "your-key-here"
```

Never commit this file.

5. Build the index:

```bash
python ingest.py
```

6. Start the app:

```bash
streamlit run app.py
```

## GitHub / Streamlit Community Cloud

Commit the Python files, owner-managed `knowledge_base/`, and generated `vectorstore/` index. If the index becomes large later, move it to external storage rather than rebuilding it for every app session. Keep `.streamlit/secrets.toml` out of GitHub and add the secret in the deployed app's Secrets settings.

## Important product rule

The RAG knowledge base is intentionally not user-editable from the front end. Only the application owner updates source documents and rebuilds the index.
