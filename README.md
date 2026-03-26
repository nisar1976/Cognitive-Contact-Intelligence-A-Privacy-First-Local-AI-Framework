# Cognitive Contact Intelligence

**A Privacy-First Local AI Framework for Business Card Digitization and Semantic Search**

---

## Overview

A fully offline Python application that processes visiting card images, extracts structured contact information using a 3-engine OCR cascade, stores data in SQLite, and provides intelligent semantic search through a Streamlit web interface.

**No API keys. No cloud uploads. Everything runs locally on your machine.**

## Features

- **3-Engine OCR Cascade** -- PaddleOCR (primary), Tesseract (fallback), EasyOCR (fallback) with automatic quality-based selection
- **Structured Field Extraction** -- Automatically parses names, companies, phone numbers, emails, addresses, and websites from raw OCR text
- **Semantic Search** -- FAISS vector similarity search with SentenceTransformers embeddings and hybrid keyword blending
- **Interactive Web UI** -- Streamlit-based interface with card processing, contact database browsing, and AI-powered search
- **Optional LLM Chat** -- Ollama integration for natural language queries over your contact database
- **CSV Export** -- Export your entire contact database for use in other tools
- **Duplicate Detection** -- Idempotent processing ensures no duplicate entries

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Frontend | Streamlit |
| OCR | PaddleOCR, Tesseract, EasyOCR |
| Image Processing | OpenCV, Pillow |
| Database | SQLite |
| Search | FAISS + SentenceTransformers (`all-MiniLM-L6-v2`) |
| LLM (optional) | Ollama (Llama3 / Mistral) |
| Package Manager | uv |
| Language | Python 3.10+ |

## Project Structure

```
project_root/
├── app.py                    # Streamlit web application
├── process_cards.py          # CLI batch processing script
├── pyproject.toml            # Dependencies and project config
├── assets/                   # Input card images (PNG)
├── database/                 # Auto-generated SQLite DB and FAISS index
├── modules/
│   ├── image_scanner.py      # Image discovery
│   ├── ocr_engine.py         # 3-engine OCR cascade
│   ├── contact_parser.py     # Structured field extraction
│   ├── database_manager.py   # SQLite CRUD operations
│   ├── semantic_search.py    # FAISS indexing and search
│   └── chatbot_interface.py  # Optional LLM integration
├── utils/
│   └── image_processing.py   # Image preprocessing pipeline
└── .streamlit/
    └── config.toml           # UI theme and settings
```

## Quick Start

### Prerequisites

- Python 3.10 or higher
- [uv](https://docs.astral.sh/uv/) package manager
- (Optional) [Tesseract OCR](https://github.com/tesseract-ocr/tesseract) installed on your system
- (Optional) [Ollama](https://ollama.ai/) for LLM-powered search

### Installation

```bash
# Clone the repository
git clone <repository-url>
cd all_visiting_cards

# Install dependencies
uv sync
```

### Usage

```bash
# Step 1: Process card images (CLI)
uv run python process_cards.py

# Step 2: Launch the web interface
uv run streamlit run app.py
```

The web UI has three pages:

1. **Process Cards** -- Scan, preview, and process all card images
2. **Contact Database** -- Browse, filter, edit, and export contacts
3. **AI Search** -- Semantic search with optional LLM summaries

### Network Access

To make the app accessible from other devices on your network:

```bash
uv run streamlit run app.py --server.address 0.0.0.0
```

### Public Sharing (via Cloudflare Tunnel)

```bash
cloudflared tunnel --url http://localhost:8501
```

## How It Works

1. **Discover** -- Scans the `assets/` folder for PNG card images
2. **Preprocess** -- Enhances images for better OCR accuracy
3. **Extract** -- Runs the 3-engine OCR cascade, picks the best result
4. **Parse** -- Extracts structured fields (name, phone, email, etc.) using regex and heuristics
5. **Store** -- Saves structured data and raw text to SQLite
6. **Index** -- Builds FAISS vector index for semantic search
7. **Search** -- Hybrid search combining 70% semantic + 30% keyword similarity

## Offline Operation

All AI models are downloaded locally on first run:

- PaddleOCR detection and recognition models
- SentenceTransformers embedding model (`all-MiniLM-L6-v2`)

After initial setup, no internet connection is required for any operation.

## License

This project is for educational and personal use.
