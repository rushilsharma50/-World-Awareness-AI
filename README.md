# 🌍 World Awareness AI

> **Real-time global event detection and monitoring, powered by a multi-agent AI classification engine.**

[![Python](https://img.shields.io/badge/Python-3.11+-blue?logo=python)](https://python.org)
[![FastAPI](https://img.shields.io/badge/FastAPI-0.110+-green?logo=fastapi)](https://fastapi.tiangolo.com)
[![Gemini](https://img.shields.io/badge/AI-Gemini%202.0%20Flash-orange?logo=google)](https://ai.google.dev)
[![License](https://img.shields.io/badge/License-MIT-lightgrey)](LICENSE)

---

## 🎯 Project Goal

World Awareness AI transforms the global news stream into a structured, real-time intelligence feed. Rather than showing raw headlines, the system automatically **detects, classifies, geocodes, and scores** notable world events — surfacing only what matters across disasters, conflicts, health crises, crime, and more.

The goal is to build a living world map where anyone can instantly see **what is happening, where, and how severe it is** — without manually sifting through thousands of articles.

---

## ✅ What's Implemented

### 🔌 Multi-Source Data Ingestion
| Source | Description |
|---|---|
| **NewsAPI** | Commercial news aggregator with region filtering |
| **GNews** | Google News-powered RSS aggregation |
| **GDELT** | Global Database of Events, Language, and Tone (structured event data with GPS coordinates) |
| **RSS Feeds** | Direct RSS parsing from major international outlets |
| **DuckDuckGo Search** | Supplemental article discovery for niche topics |
| **ReliefWeb** | UN-backed humanitarian disaster & health event feed |
| **GDACS** | Global Disaster Alert and Coordination System for weather & natural disasters |
| **Medical Research RSS** | Specialized feeds for health & outbreak detection |

### 🤖 Multi-Agent AI Classification Engine
- **Triple-Agent Round-Robin**: Rotates between **Google Gemini 2.0 Flash**, **Groq (Llama 3)**, and **OpenRouter (Llama 3.1)** for classification.
- **Graceful Fallback Chain**: If one AI agent hits a rate limit or fails, the next agent in the chain takes over automatically.
- **Keyword Fallback**: When all AI agents are unavailable, a deterministic keyword-matching engine classifies events locally — ensuring zero downtime.
- **Noise Filtering**: A blocklist pre-screens articles for non-event content (contests, tutorials, webinars, etc.) before spending API tokens.

### 🗺️ Geocoding & Location Extraction
- Uses **spaCy NLP** to extract location names from article text.
- AI provides a `primary_location` hint for more accurate geocoding.
- Falls back gracefully when coordinates cannot be resolved.
- Supports `is_global` flag for worldwide events without a specific pin.

### 📊 Event Processing & Deduplication
- **Clustering Engine** (`event_processor.py`): Groups duplicate/similar events together so the same story doesn't appear multiple times.
- **Importance Scoring**: Events are scored 1–5 (minor → catastrophic) and sorted by severity.
- **Impact Notes**: AI generates cross-border impact summaries for events with international consequences.

### 🗂️ Event Categories
`disaster` · `conflict` · `weather` · `accident` · `health` · `medical` · `political` · `environmental` · `tech` · `economy` · `crime`

### 🖥️ Frontend Dashboard
- **Interactive World Map** powered by **Leaflet.js** with marker clustering.
- **Density Heatmap** toggle to visualize event hotspots.
- **Live Event Sidebar** with per-category filter buttons and a severity range slider.
- **Region Selector** covering 40+ countries and regions worldwide.
- **Real-time Stats Bar** showing event counts by category and source.
- **Toast Notifications** and live status updates.
- Dark, premium UI with micro-animations (Inter + JetBrains Mono fonts).

### ⚡ Backend API (FastAPI)
| Endpoint | Description |
|---|---|
| `GET /` | Serves the frontend dashboard |
| `GET /api/events` | Returns classified & clustered events (supports `region`, `category`, `severity` filters) |
| `GET /api/events/stats` | Aggregated statistics by category, severity, and data source |
| `GET /api/regions` | List of all supported regions with labels |

- **In-memory TTL Cache** (`cachetools`) to avoid redundant API calls.
- **Parallel Processing** with `ThreadPoolExecutor` for fast multi-source ingestion.
- CORS-enabled for local and deployed frontend access.

---

## 🛠️ Tech Stack

### Backend
| Technology | Role |
|---|---|
| **Python 3.11+** | Core language |
| **FastAPI** | REST API framework |
| **Uvicorn** | ASGI server |
| **Google Gemini 2.0 Flash** | Primary AI classification model |
| **Groq (Llama 3 8B)** | Secondary AI agent |
| **OpenRouter (Llama 3.1)** | Tertiary AI agent |
| **spaCy** | NLP for entity/location extraction |
| **Geopy** | Geocoding (Nominatim) |
| **feedparser** | RSS feed parsing |
| **duckduckgo-search** | Supplemental article discovery |
| **cachetools** | TTL in-memory cache |
| **python-dotenv** | Environment variable management |
| **requests** | HTTP client for external APIs |

### Frontend
| Technology | Role |
|---|---|
| **HTML5 / CSS3 / Vanilla JS** | Core frontend (no framework) |
| **Leaflet.js** | Interactive map rendering |
| **Leaflet.MarkerCluster** | Event marker clustering on map |
| **Leaflet.heat** | Heatmap density layer |
| **Google Fonts** | Inter & JetBrains Mono typography |

### External Data Sources & APIs
- NewsAPI, GNews API, GDELT, ReliefWeb API, GDACS RSS
- Google Gemini API, Groq API, OpenRouter API

---

## 🚀 Getting Started

### Prerequisites
- Python 3.11+
- API keys for: NewsAPI, GNews, Gemini, Groq, and/or OpenRouter

### Installation

```bash
# 1. Clone the repository
git clone https://github.com/your-username/world-ai.git
cd world-ai

# 2. Create and activate a virtual environment
python -m venv .venv
.venv\Scripts\activate        # Windows
# source .venv/bin/activate   # Linux/macOS

# 3. Install dependencies
pip install -r backend/requirements.txt

# 4. Download spaCy language model
python -m spacy download en_core_web_sm
```

### Configuration

Create a `.env` file inside the `backend/` directory:

```env
NEWSAPI_KEY=your-newsapi-key
GNEWS_API_KEY=your-gnews-key
GEMINI_API_KEY=your-gemini-api-key
GROQ_API_KEY=your-groq-api-key
OPENROUTER_API_KEY=your-openrouter-api-key
```

> **Note:** The system is designed to degrade gracefully. If only some API keys are configured, the remaining agents will be skipped and the keyword fallback will cover classification.

### Running the App

```bash
cd backend
uvicorn main:app --reload --port 8000
```

Open your browser at **[http://localhost:8000](http://localhost:8000)**

---

## 🔮 Future Goals

### Near-Term
- [ ] **WebSocket live updates** — push new events to the frontend in real time without page refresh
- [ ] **Historical timeline** — slider to replay events over the past 24–72 hours
- [ ] **Event detail modal** — click any map pin to see full article text, source links, and AI impact analysis
- [ ] **Persistent event database** — store events in SQLite/PostgreSQL to support history and deduplication across sessions

### Medium-Term
- [ ] **Trend detection** — identify escalating situations by tracking the same event cluster over time
- [ ] **User alerts / subscriptions** — email or browser push notifications for high-severity events in a chosen region
- [ ] **Social media signal integration** — ingest Twitter/X and Reddit signals to detect breaking events before traditional news
- [ ] **Sentiment & confidence scores** — display AI confidence level and media sentiment per event
- [ ] **Multi-language support** — classify and display events from non-English sources

### Long-Term Vision
- [ ] **Predictive risk modeling** — use historical patterns to flag regions at elevated risk before events occur
- [ ] **AR / 3D Globe frontend** — replace the 2D map with an interactive 3D globe visualization
- [ ] **Mobile app** (React Native or Flutter) with offline event cache
- [ ] **Agency / Government API** — provide a secured API tier for professional analysts and humanitarian organizations
- [ ] **Fine-tuned classification model** — replace general-purpose LLMs with a domain-specific event detection model trained on labeled news data

---

## 📁 Project Structure

```
world-ai/
├── backend/
│   ├── main.py               # FastAPI app, routing, caching
│   ├── ai_engine.py          # Multi-agent AI classification pipeline
│   ├── event_processor.py    # Event clustering & deduplication
│   ├── geocoder.py           # NLP location extraction + geocoding
│   ├── config.py             # API keys, categories, region config
│   ├── news_client.py        # NewsAPI client
│   ├── gnews_client.py       # GNews client
│   ├── gdelt_client.py       # GDELT client
│   ├── free_news_client.py   # RSS + DuckDuckGo client
│   ├── specialized_client.py # ReliefWeb + GDACS client
│   └── requirements.txt
├── frontend/
│   ├── index.html            # Dashboard markup
│   ├── script.js             # Map, filters, API calls, live updates
│   └── style.css             # Dark theme, animations, responsive layout
└── README.md
```

---

## 🤝 Contributing

Pull requests are welcome! For major changes, please open an issue first to discuss what you'd like to change.

---

## 📄 License

This project is licensed under the MIT License.
