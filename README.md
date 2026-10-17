# College AI Assistant

A small college chatbot built as one LangGraph orchestrator routing queries to four specialised agents, backed by a shared ChromaDB vector store and a SQLite database.

## Architecture

| Agent                       | Module                              | Responsibility                                                              |
| --------------------------- | ----------------------------------- | --------------------------------------------------------------------------- |
| Rules & Policy Agent       | `agents/rules_agent.py`             | RAG over college regulations, attendance, exam policies, scholarships       |
| Announcements Agent         | `agents/announcements_agent.py`     | Current notices, events, exam schedules, holidays, deadlines (SQLite)       |
| Academic Agent              | `agents/academic_agent.py`          | Syllabus, subjects, faculty, study materials, academic calendar             |
| Student Services Agent      | `agents/student_services_agent.py`  | Fees, library, hostel, placements, certificates, administrative queries     |

Supporting modules:

- `orchestrator.py`: LangGraph routing, multi-agent calls and answer synthesis
- `ingest.py`: PDF extraction (PyMuPDF), chunking, embeddings into ChromaDB
- `db.py`: SQLite schema and connection helper
- `config.py`: settings read from environment variables / `.env`
- `app.py`: Streamlit chatbot UI

## Setup

```bash
python -m venv .venv
.venv\Scripts\activate          # Windows (use source .venv/bin/activate elsewhere)
pip install -r requirements.txt
copy .env.example .env          # then fill in OPENROUTER_API_KEY and the model IDs
```

Put the source PDFs in `data/` (see [data/README.md](data/README.md)), then:

```bash
python ingest.py                # build the ChromaDB index
streamlit run app.py
```

## Status

Repository scaffold only. Agents, orchestrator and ingestion raise `NotImplementedError` until they are built.

Planned demos:

1. Answer a college regulation using PDF-based RAG.
2. Retrieve the latest announcements for a department.
3. Explain a subject or retrieve a semester syllabus.
4. Answer a campus-service question such as library timings.
5. Handle a question that needs two agents.
6. Upload a new announcement through an admin form and make it searchable immediately.
