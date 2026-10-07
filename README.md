# Semantic YouTube Video Retrieval

A semantic video search pipeline that converts YouTube videos into timestamped transcript segments and retrieves the most relevant portions of a video using natural-language queries.

The system uses OpenAI Whisper for speech transcription, Sentence Transformers for semantic embeddings, and FAISS for vector similarity search.

## Problem

Searching within long-form videos is difficult because users usually need to scrub through the video manually or rely on exact keyword matching.

This project explores whether semantic embeddings can be used to retrieve relevant video segments even when the user's query does not exactly match the words used by the speaker.

## Pipeline

YouTube Video
    ↓
Audio Extraction using FFmpeg
    ↓
Speech-to-Text using OpenAI Whisper
    ↓
Timestamp-Based Transcript Chunking
    ↓
Sentence Transformer Embeddings
    ↓
FAISS Vector Index
    ↓
Natural-Language Query
    ↓
Top-K Relevant Video Segments
    ↓
Timestamped YouTube Links

## Features

- Downloads and processes YouTube videos
- Extracts audio using FFmpeg
- Transcribes speech using OpenAI Whisper
- Preserves transcript timestamps
- Groups transcript segments into semantic chunks
- Generates vector embeddings using Sentence Transformers
- Indexes embeddings using FAISS
- Retrieves Top-K semantically relevant video segments
- Returns transcript text, similarity score, and video timestamp
- Generates clickable YouTube links that jump directly to the retrieved segment

## Retrieval Example

Query:

"What does the speaker say about people using AI?"

Retrieved result:

Time: 1137s - 1151s

Transcript:
"The people who leverage AI will be more effective than those who don't leverage AI."

The returned YouTube link opens the video directly at the retrieved timestamp.

## Evaluation

To evaluate retrieval quality, I created a manually labeled benchmark of 20 questions with corresponding ground-truth timestamp ranges.

Retrieval performance was measured using Recall@3:

Recall@3 = Number of queries where the correct segment appeared in the Top 3 results / Total queries

### Experiments

| Configuration | Recall@3 |
|---|---:|
| 15-second chunks + MiniLM | 70% |
| 30-second chunks + MiniLM | 80% |
| 30-second chunks + MPNet | 85% |

Increasing the chunk size from 15 seconds to 30 seconds improved retrieval performance by preserving more semantic context.

Replacing `all-MiniLM-L6-v2` with the stronger `all-mpnet-base-v2` embedding model further improved Recall@3 from 80% to 85%.

The final configuration uses:

- 30-second transcript chunks
- `all-mpnet-base-v2`
- FAISS `IndexFlatIP`
- normalized embeddings for cosine-style similarity search

## Technologies

- Python
- OpenAI Whisper
- FFmpeg
- PyDub
- Sentence Transformers
- FAISS
- Hugging Face
- Google Colab

## Installation

```bash
pip install openai-whisper
pip install sentence-transformers
pip install faiss-cpu
pip install pydub
