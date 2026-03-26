# Cognitive Contact Intelligence: A Privacy-First Local AI Framework

## Project Title

**Cognitive Contact Intelligence: A Privacy-First Local AI Framework for Automated Business Card Digitization, Structured Data Extraction, and Semantic Contact Search**

---

## Description

### Scope

This project addresses the challenge of digitizing physical business card collections at scale. Organizations and professionals accumulate hundreds of visiting cards over time, yet extracting and organizing that contact information remains a tedious manual process. This framework automates the entire pipeline -- from raw card image to searchable, structured contact database -- while keeping all data strictly local and private.

The system handles:

- Batch processing of large card image collections (tested with 348 cards)
- Multi-engine OCR with automatic quality-based engine selection
- Intelligent parsing of unstructured OCR text into structured contact fields
- Persistent storage with duplicate detection and idempotent processing
- Semantic and keyword-based search across the entire contact corpus
- Interactive web-based interface for browsing, editing, and exporting data
- Optional natural language querying through local LLM integration

### Goal

To build a **complete, production-ready, fully offline** AI application that transforms unstructured visiting card images into a structured, searchable contact database -- without sending any data to external servers or requiring any API keys.

The project demonstrates that meaningful AI applications can run entirely on consumer hardware, preserving data privacy while delivering capabilities traditionally associated with cloud-based SaaS tools.

### Problem Statement

- Manual transcription of business cards is slow, error-prone, and does not scale
- Cloud-based card scanning apps require uploading personal contact data to third-party servers
- Single-engine OCR solutions produce inconsistent results across varying card designs, fonts, and languages
- Extracted text lacks structure -- raw OCR output is not immediately useful without parsing
- Traditional keyword search fails to surface relevant contacts when users describe what they need in natural language

### Solution

The framework implements a multi-layered AI pipeline:

1. **Image Preprocessing Pipeline** -- OpenCV-based enhancement (contrast adjustment, noise reduction, binarization) prepares card images for optimal OCR accuracy

2. **Three-Engine OCR Cascade** -- Rather than relying on a single OCR engine, the system runs up to three engines in sequence (PaddleOCR, Tesseract, EasyOCR) and selects the best result using a composite scoring function based on text length and confidence. This cascade approach significantly improves extraction reliability across diverse card designs

3. **Heuristic Contact Parser** -- Regex patterns and rule-based heuristics extract structured fields (name, company, designation, phone, email, address, website) from raw OCR text. The parser handles multi-line entries, varied formatting conventions, and partial data gracefully

4. **Persistent SQLite Storage** -- All extracted data persists in a local SQLite database with idempotent insertion (re-processing adds zero duplicates). The schema stores both structured fields and raw OCR text for maximum flexibility

5. **Hybrid Semantic Search** -- FAISS vector indexing with SentenceTransformer embeddings enables natural language search (e.g., "marketing consultants in Lahore"). A 70/30 semantic-to-keyword blending strategy combines the precision of keyword matching with the flexibility of semantic understanding

6. **Interactive Streamlit Web Interface** -- Three-page application for processing cards, browsing/editing the contact database, and performing AI-powered search with contact card rendering

7. **Optional LLM Integration** -- Ollama-based local LLM (Llama3/Mistral) provides conversational summarization of search results, with graceful degradation when unavailable

### Impact

- **Privacy Preserved** -- Zero data leaves the user's machine. No cloud uploads, no API keys, no telemetry
- **Time Savings** -- Automates what would take hours of manual data entry into a single batch operation
- **Accuracy** -- The three-engine cascade achieves significantly higher extraction rates than any single OCR engine alone
- **Accessibility** -- Streamlit interface makes the tool usable by non-technical users
- **Portability** -- SQLite database and CSV export allow easy integration with other tools and workflows
- **Cost** -- Runs on consumer hardware with no recurring subscription fees

### Use Cases

- **Professional Networking** -- Digitize business cards collected at conferences, trade shows, and meetings
- **Sales Teams** -- Quickly build searchable lead databases from collected cards
- **Administrative Staff** -- Maintain organized digital contact records for an organization
- **Event Organizers** -- Process attendee cards in bulk after events
- **Recruitment** -- Digitize candidate contact information from career fairs
- **Personal Organization** -- Convert years of accumulated visiting cards into a searchable digital archive

### Future Scope

- **Multi-language OCR Enhancement** -- Expand parsing rules for non-English cards (Arabic, Urdu, Chinese, etc.)
- **GPU Acceleration** -- Leverage CUDA-enabled PaddleOCR for significantly faster processing
- **Mobile Capture Integration** -- Accept photos taken directly from mobile cameras with perspective correction
- **vCard/VCF Export** -- Export contacts in standard vCard format for direct import into phone contacts and email clients
- **Cloud Deployment Option** -- Dockerized deployment for teams that choose to run the system on a shared server
- **Active Learning** -- Use corrections made in the UI to improve parsing accuracy over time
- **Batch PDF Support** -- Process multi-page scanned PDFs containing multiple cards per page
- **API Endpoint** -- REST API layer for integration with CRM systems and other enterprise tools
- **Dashboard Analytics** -- Visual analytics on contact distribution by company, city, industry, etc.
- **Deduplication Intelligence** -- Fuzzy matching to detect and merge near-duplicate contacts across different card versions

---

## Technology Stack

### Core Languages and Frameworks

| Component | Technology | Purpose |
|-----------|-----------|---------|
| Language | Python 3.10+ | Primary development language |
| Web Framework | Streamlit | Interactive web UI with real-time updates |
| Package Manager | uv | Fast Python dependency management |

### OCR and Image Processing

| Component | Technology | Purpose |
|-----------|-----------|---------|
| Primary OCR | PaddleOCR | High-accuracy OCR engine (primary in cascade) |
| Fallback OCR | Tesseract (pytesseract) | Widely-deployed open-source OCR (second in cascade) |
| Fallback OCR | EasyOCR | Deep-learning-based OCR (third in cascade) |
| Image Processing | OpenCV | Preprocessing pipeline (contrast, thresholding, noise reduction) |
| Image Handling | Pillow (PIL) | Image loading, format conversion, thumbnail generation |

### Data Storage and Search

| Component | Technology | Purpose |
|-----------|-----------|---------|
| Database | SQLite | Local relational storage with CRUD operations |
| Vector Search | FAISS (faiss-cpu) | Facebook AI Similarity Search for nearest-neighbor retrieval |
| Embeddings | SentenceTransformers | Text-to-vector encoding using `all-MiniLM-L6-v2` model |
| Data Analysis | Pandas | Tabular data handling, CSV export, dataframe rendering |
| Numerical | NumPy | Array operations for embeddings and scoring |
| ML Utilities | scikit-learn | Cosine similarity computation for keyword search |

### Optional Components

| Component | Technology | Purpose |
|-----------|-----------|---------|
| Local LLM | Ollama (Llama3 / Mistral) | Conversational AI for natural language contact queries |
| HTTP Client | Requests | Communication with local Ollama server |
| Progress Bars | tqdm | CLI progress indicators during batch processing |
| Environment | python-dotenv | Environment variable management |

### Deep Learning Runtime

| Component | Technology | Purpose |
|-----------|-----------|---------|
| Tensor Runtime | PyTorch (< 2.6.0) | Required by PaddleOCR, EasyOCR, and SentenceTransformers |
| Paddle Runtime | PaddlePaddle (< 3.0) | Backend for PaddleOCR inference |

### Infrastructure and Deployment

| Component | Technology | Purpose |
|-----------|-----------|---------|
| Tunneling | Cloudflare Tunnel (cloudflared) | Expose local app to the internet for sharing |
| Version Control | Git | Source code management |
| Configuration | TOML (`.streamlit/config.toml`) | Streamlit theme and server settings |

---

## Deliverables

### Application Components

1. **Streamlit Web Application** (`app.py`) -- Three-page interactive interface:
   - Card Processing page with image preview and batch OCR execution
   - Contact Database page with pagination, filtering, inline editing, and CSV export
   - AI Search page with semantic search, contact card rendering, and optional LLM chat

2. **CLI Batch Processor** (`process_cards.py`) -- Standalone command-line script for processing card images without the web UI, suitable for automated pipelines

3. **Image Scanner Module** (`modules/image_scanner.py`) -- Discovers and catalogs card images from the assets directory

4. **OCR Engine Module** (`modules/ocr_engine.py`) -- Implements the three-engine cascade with confidence-based scoring and automatic fallback

5. **Contact Parser Module** (`modules/contact_parser.py`) -- Regex and heuristic-based extraction of structured contact fields from raw OCR text

6. **Database Manager Module** (`modules/database_manager.py`) -- Full SQLite CRUD operations including initialization, insertion, search, update, deletion, statistics, and export

7. **Semantic Search Module** (`modules/semantic_search.py`) -- FAISS index construction, embedding generation, and hybrid search implementation

8. **Chatbot Interface Module** (`modules/chatbot_interface.py`) -- Ollama LLM integration with availability detection and graceful degradation

9. **Image Processing Utilities** (`utils/image_processing.py`) -- OpenCV preprocessing pipeline and display-optimized resizing

### Data Outputs

10. **SQLite Contact Database** (`database/contacts.db`) -- Persistent structured storage of all extracted contacts

11. **FAISS Vector Index** (`database/contacts.faiss`) -- Pre-built similarity search index for instant semantic queries

12. **CSV Export** -- On-demand export of the full contact database

### Documentation

13. **Project Documentation** (`project.md`) -- Project scope, goals, technology stack, and deliverables (this document)

14. **README** (`README.md`) -- Setup instructions, usage guide, and quick start

15. **Tutorial Lessons** (`lessons.md`) -- Comprehensive 22-lesson step-by-step tutorial covering every module from setup to deployment

16. **Developer Guide** (`CLAUDE.md`) -- Architecture reference, data flow, schema documentation, and development commands

---

## Architecture Summary

```
Card Images (PNG)
       |
       v
 Image Scanner ── discovers files in assets/
       |
       v
 Image Preprocessor ── OpenCV enhancement pipeline
       |
       v
 OCR Cascade ── PaddleOCR -> Tesseract -> EasyOCR
       |           (selects best result by confidence)
       v
 Contact Parser ── regex + heuristics -> structured fields
       |
       v
 SQLite Database ── persistent storage (idempotent writes)
       |
       v
 FAISS Indexer ── builds vector embeddings for all contacts
       |
       v
 Streamlit UI ── browse, search, edit, export
       |
       v
 (Optional) Ollama LLM ── natural language summaries
```

---

## Key Design Decisions

- **Cascade over single-engine** -- No single OCR engine handles all card designs well. The cascade approach maximizes extraction success across varying fonts, layouts, and print quality
- **Local-first architecture** -- Privacy-sensitive contact data never leaves the machine. All AI inference runs locally
- **Hybrid search** -- Pure semantic search can miss exact matches; pure keyword search can miss conceptual matches. The 70/30 blend captures both
- **Idempotent processing** -- Database uniqueness constraints allow safe re-runs without duplicates
- **Graceful degradation** -- Each optional component (Tesseract, EasyOCR, Ollama) degrades gracefully if unavailable
- **Module independence** -- Each module can be tested and used independently of the full application

---

*This project was developed as a demonstration of applied AI for document intelligence, combining computer vision, natural language processing, and information retrieval in a privacy-respecting, fully offline system.*
