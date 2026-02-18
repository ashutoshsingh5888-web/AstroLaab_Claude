# 🌟 Vedic Astrology API

A production-ready REST API for Vedic (Jyotish) astrology powered by [Swiss Ephemeris](https://www.astro.com/swisseph/).

[![Python](https://img.shields.io/badge/Python-3.11+-blue.svg)](https://python.org)
[![FastAPI](https://img.shields.io/badge/FastAPI-0.115-green.svg)](https://fastapi.tiangolo.com)
[![Swiss Ephemeris](https://img.shields.io/badge/Swiss_Ephemeris-Lahiri-orange.svg)](https://www.astro.com/swisseph/)

---

## ✨ Features

| Feature | Endpoint | Description |
|---------|----------|-------------|
| Birth Chart | `POST /api/v1/birth-chart` | Full Kundli with all planets & houses |
| Birth Chart SVG | `POST /api/v1/birth-chart/svg` | North or South Indian style SVG |
| Birth Chart PDF | `POST /api/v1/birth-chart/pdf` | Detailed PDF report |
| Rashi | `POST /api/v1/rashi` | Moon sign (Vedic) |
| Nakshatra | `POST /api/v1/nakshatra` | Birth star, pada, deity |
| Zodiac | `POST /api/v1/zodiac` | Vedic + Western sun sign |
| Dasha | `POST /api/v1/dasha` | Vimshottari dasha timeline |
| Manglik | `POST /api/v1/manglik` | Mangal dosha check |
| Gun Milan | `POST /api/v1/gun-milan` | Kundli matching (36 points) |
| Numerology | `POST /api/v1/numerology` | Life path & name numbers |
| Retrograde | `POST /api/v1/retrograde` | Retrograde planets on a date |
| Retrograde Now | `GET /api/v1/retrograde/current` | Currently retrograde planets |

**Ayanamsa:** Lahiri (Chitrapaksha)  
**House System:** Whole Sign  
**Timezone:** All input/output in IST (UTC+5:30)

---

## 🚀 Quick Start

### 1. Clone and Setup

```bash
git clone https://github.com/yourusername/astrology-api.git
cd astrology-api

# Create virtual environment
python -m venv .venv
source .venv/bin/activate    # Linux/Mac
# .venv\Scripts\activate     # Windows

# Install dependencies
pip install -r requirements.txt
```

### 2. Download Ephemeris Files

```bash
python scripts/download_ephe.py
```

### 3. Configure Environment

```bash
cp .env.example .env
```

Edit `.env`:
```env
# Generate a key:
# python scripts/generate_key.py
API_KEYS=astro-your-generated-key-here

ENVIRONMENT=development
LOG_FORMAT=text
LOG_LEVEL=DEBUG
```

### 4. Run the API

```bash
uvicorn app.main:app --reload --port 8000
```

Visit:
- **API Docs**: http://localhost:8000/docs
- **Health**: http://localhost:8000/health

---

## 🔑 Authentication

All `/api/v1/*` endpoints require an `X-API-Key` header.

```bash
# Generate a key
python scripts/generate_key.py

# Use in requests
curl -H "X-API-Key: astro-your-key-here" http://localhost:8000/api/v1/rashi \
  -H "Content-Type: application/json" \
  -d '{"date":"1990-05-15","time":"14:30:00","latitude":28.6139,"longitude":77.2090}'
```

Multiple keys are supported (comma-separated in `API_KEYS`):
```env
API_KEYS=astro-key1,astro-key2,astro-key3
```

---

## 📡 API Usage Examples

### Birth Chart

```bash
curl -X POST "http://localhost:8000/api/v1/birth-chart" \
  -H "X-API-Key: your-key" \
  -H "Content-Type: application/json" \
  -d '{
    "date": "1990-05-15",
    "time": "14:30:00",
    "latitude": 28.6139,
    "longitude": 77.2090,
    "name": "Arjun Sharma",
    "place": "New Delhi, India"
  }'
```

### Moon Sign (Rashi)

```bash
curl -X POST "http://localhost:8000/api/v1/rashi" \
  -H "X-API-Key: your-key" \
  -H "Content-Type: application/json" \
  -d '{"date":"1990-05-15","time":"14:30:00","latitude":28.6139,"longitude":77.2090}'
```

### North Indian SVG Chart

```bash
curl -X POST "http://localhost:8000/api/v1/birth-chart/svg" \
  -H "X-API-Key: your-key" \
  -H "Content-Type: application/json" \
  -d '{"date":"1990-05-15","time":"14:30:00","latitude":28.6139,"longitude":77.2090,"style":"north"}' \
  -o kundli.svg
```

### PDF Report

```bash
curl -X POST "http://localhost:8000/api/v1/birth-chart/pdf" \
  -H "X-API-Key: your-key" \
  -H "Content-Type: application/json" \
  -d '{"date":"1990-05-15","time":"14:30:00","latitude":28.6139,"longitude":77.2090,"name":"Arjun Sharma"}' \
  -o kundli.pdf
```

### Kundli Matching (Gun Milan)

```bash
curl -X POST "http://localhost:8000/api/v1/gun-milan" \
  -H "X-API-Key: your-key" \
  -H "Content-Type: application/json" \
  -d '{
    "person1": {"date":"1990-05-15","time":"14:30:00","latitude":28.6139,"longitude":77.2090,"name":"Arjun"},
    "person2": {"date":"1992-08-20","time":"09:15:00","latitude":19.0760,"longitude":72.8777,"name":"Priya"}
  }'
```

### Currently Retrograde Planets

```bash
curl -H "X-API-Key: your-key" http://localhost:8000/api/v1/retrograde/current
```

---

## 🌐 Deploy to Render

### Step 1: Push to GitHub

```bash
git init
git add .
git commit -m "Initial commit"
git remote add origin https://github.com/yourusername/astrology-api.git
git push -u origin main
```

### Step 2: Create Render Service

1. Go to [render.com](https://render.com) → **New** → **Web Service**
2. Connect your GitHub repository
3. Render will auto-detect `render.yaml`

### Step 3: Set Environment Variables

In Render Dashboard → Your Service → **Environment**:

| Variable | Value |
|----------|-------|
| `API_KEYS` | `astro-your-key` (generate with `python scripts/generate_key.py`) |
| `ENVIRONMENT` | `production` |
| `LOG_FORMAT` | `json` |
| `LOG_LEVEL` | `INFO` |

> ⚠️ **Never commit your API key to git.** Always set it in Render's environment variables.

### Step 4: Deploy

Render will automatically:
1. Install dependencies from `requirements.txt`
2. Start the API with gunicorn + uvicorn workers
3. Provide a public URL like `https://vedic-astrology-api.onrender.com`

---

## 🧪 Running Tests

```bash
pip install pytest
pytest tests/ -v
```

---

## 🏗️ Project Structure

```
astrology-api/
├── app/
│   ├── main.py              # FastAPI app, middleware, routers
│   ├── core/
│   │   ├── config.py        # Settings (env vars)
│   │   ├── logger.py        # Structured JSON/text logging
│   │   ├── ephemeris.py     # Swiss Ephemeris wrapper
│   │   └── timezone_utils.py # IST ↔ UTC ↔ Julian Day
│   ├── modules/
│   │   ├── dasha.py         # Vimshottari dasha calculator
│   │   ├── gun_milan.py     # 8-kuta matching system
│   │   ├── manglik.py       # Mangal dosha checker
│   │   └── numerology.py    # Pythagorean numerology
│   ├── renderers/
│   │   ├── chart_svg.py     # North & South Indian SVG charts
│   │   └── pdf_report.py    # ReportLab PDF generator
│   ├── middleware/
│   │   └── auth.py          # API key authentication
│   ├── models/
│   │   └── schemas.py       # Pydantic request/response models
│   └── routers/
│       ├── birth_chart.py   # /birth-chart endpoints
│       ├── dasha.py         # /dasha endpoint
│       ├── gun_milan.py     # /gun-milan endpoint
│       ├── health.py        # /health endpoints
│       ├── manglik.py       # /manglik endpoint
│       ├── numerology.py    # /numerology endpoint
│       ├── rashee.py        # /rashi, /nakshatra endpoints
│       ├── retrograde.py    # /retrograde endpoints
│       └── zodiac.py        # /zodiac endpoint
├── ephe/                    # Swiss Ephemeris data files
├── scripts/
│   ├── download_ephe.py     # Download SE1 data files
│   └── generate_key.py      # Generate API keys
├── tests/
│   └── test_core.py         # Unit tests
├── .env.example             # Environment template
├── .gitignore
├── render.yaml              # Render deployment config
├── requirements.txt
└── README.md
```

---

## 📊 Technical Details

### Astronomy Engine
- **Swiss Ephemeris** via `pyswisseph` — the gold standard for astrological calculations
- **Ayanamsa**: Lahiri (Chitrapaksha) — standard for Indian Vedic astrology
- **Coordinate System**: Sidereal (Vedic), with tropical available for Western sun sign
- **House System**: Whole Sign (each sign = one house, starting from ascendant sign)
- **Timezone**: All inputs/outputs in IST (UTC+5:30); internally uses Julian Day Numbers (UTC)

### Vimshottari Dasha
The 120-year planetary period system based on Moon's birth nakshatra:
- 27 nakshatras × 4 padas = 108 padas
- Each nakshatra has a dasha lord (ruling planet)
- Balance at birth = fraction of nakshatra remaining × total dasha years

### Gun Milan (8 Kutas)
Traditional Vedic compatibility scoring based on both parties' Moon nakshatra and sign:
| Score | Compatibility |
|-------|---------------|
| 28–36 | Excellent |
| 21–27 | Good |
| 18–20 | Average |
| 0–17  | Poor |

---

## 📝 License

MIT License — see [LICENSE](LICENSE) for details.

> **Note**: `pyswisseph` is licensed under AGPL-2.0. If you deploy this as a SaaS, review the AGPL implications.

---

## 🙏 Credits

- [Swiss Ephemeris](https://www.astro.com/swisseph/) by Astrodienst AG
- [pyswisseph](https://github.com/astrorigin/pyswisseph) Python binding
- [FastAPI](https://fastapi.tiangolo.com) web framework
- [ReportLab](https://www.reportlab.com) PDF generation
