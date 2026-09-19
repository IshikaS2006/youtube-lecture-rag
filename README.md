# YouTube Lecture RAG

A timestamp-aware RAG system for asking questions about long-form YouTube lectures and getting **grounded answers with clickable video timestamps**.

The current implementation is demonstrated on a DSA lecture playlist, but the architecture can be adapted to other educational and technical video collections.

## Demo

![YT Lecture RAG Demo](docs/screenshots/demo.png)

Ask questions about lectures, get grounded answers, and jump directly to the relevant video timestamp.

## How It Works

```text
YouTube Playlist
      ↓
    yt-dlp
      ↓
Whisper Transcription
      ↓
Timestamped Chunks
      ↓
Embeddings
      ↓
Qdrant Vector DB
      ↓
Semantic Retrieval
      ↓
Groq LLM
      ↓
Answer + Timestamp Citations
      ↓
YouTube Player
```

The key idea is that retrieved information keeps its **original video timestamp**, so users can jump directly to where the concept was explained.

## Features

* 🎥 YouTube lecture ingestion
* 🎙️ Whisper-based transcription
* ⏱️ Timestamp-aware chunking
* 🔎 Semantic search with Qdrant
* 🤖 Grounded LLM answers
* 📍 Clickable timestamp citations
* 🛑 Refusal when relevant context is not found
* 💾 Cached transcripts for efficient re-indexing
* 🌐 FastAPI web interface
* 📊 Retrieval evaluation with a golden set

## Tech Stack

| Component       | Technology                |
| --------------- | ------------------------- |
| Video ingestion | yt-dlp                    |
| Transcription   | faster-whisper            |
| Embeddings      | sentence-transformers     |
| Vector DB       | Qdrant                    |
| LLM             | Groq                      |
| Backend         | FastAPI                   |
| CLI             | Typer                     |
| Frontend        | HTML + YouTube IFrame API |

## Setup

### 1. Clone

```bash
git clone https://github.com/IshikaS2006/youtube-lecture-rag.git
cd youtube-lecture-rag
```

### 2. Install

```bash
uv venv --python 3.11
.venv\Scripts\Activate.ps1
uv sync
```

### 3. Configure `.env`

```env
GROQ_API_KEY=your_groq_api_key
QDRANT_URL=your_qdrant_url
QDRANT_API_KEY=your_qdrant_api_key
```

## Use the Existing Corpus

Rebuild the vector index:

```bash
uv run ytrag reindex
```

Ask a question:

```bash
uv run ytrag ask "memoization aur tabulation ka difference kya hai?"
```

Search the corpus directly:

```bash
uv run ytrag search "binary search"
```

Start the web UI:

```bash
uv run ytrag serve
```

Then open:

```text
http://127.0.0.1:8000
```

## Ingest Your Own Playlist

```bash
uv run ytrag preflight --playlist "<PLAYLIST_URL>"

uv run ytrag ingest --playlist "<PLAYLIST_URL>" --limit 1

uv run ytrag ingest --playlist "<PLAYLIST_URL>"

uv run ytrag reindex
```

Transcripts are cached, so changing chunking or embeddings does not require re-transcribing the videos.

## Evaluation

```bash
uv run ytrag eval --verbose
```

The evaluation checks whether relevant videos and timestamps are retrieved for predefined questions.

## Project Structure

```text
youtube-lecture-rag/
├── api/
├── eval/
├── index/
├── transcripts/
├── ytrag/
├── docs/
│   └── screenshots/
│       └── demo.png
├── .env.example
├── main.py
├── pyproject.toml
└── uv.lock
```

## Why This Project?

Most RAG systems answer:

> **"What is the answer?"**

This system also answers:

> **"Where in the lecture can I find it?"**

That makes long-form video content searchable and directly actionable.
