# Jyotish AI Astrologer

![Python](https://img.shields.io/badge/Python-3.9%2B-3776AB?logo=python&logoColor=white)
![Flask](https://img.shields.io/badge/Flask-Web%20App-000000?logo=flask&logoColor=white)
![OpenAI](https://img.shields.io/badge/OpenAI-AI%20Assistant-412991?logo=openai&logoColor=white)
![Swiss Ephemeris](https://img.shields.io/badge/Swiss%20Ephemeris-pyswisseph-1f6feb)
![Ayanamsa](https://img.shields.io/badge/Ayanamsa-Lahiri-orange)
![Status](https://img.shields.io/badge/Status-In%20Development-yellow)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

A Flask web application for Vedic (sidereal) astrology. It generates birth charts using the Swiss Ephemeris, performs Kundali compatibility matching, produces rule-based predictions with remedies, and includes an AI chat assistant that answers questions using the user's own chart data.

## Features

- **Birth chart generation** - Sidereal positions using the Lahiri ayanamsa for the Sun, Moon, Mercury, Venus, Mars, Jupiter, Saturn, plus Rahu and Ketu (mean node).
- **Zodiac and Nakshatra mapping** - Signs shown in both Sanskrit (Mesha, Vrishabha, ...) and English (Aries, Taurus, ...), with all 27 Nakshatras and their padas.
- **Ascendant and houses** — Ascendant and house cusps computed from birth date, time and location.
- **Panchanga** - Tithi and Nakshatra for the birth moment (Yoga and Karana are currently simplified).
- **Kundali matching** - Compatibility analysis between two people from their birth dates and a shared location.
- **Predictions and remedies** — Prediction signals generated from the chart, with matching remedies.
- **AI Astrologer chat** — Conversational assistant that uses the most recently generated chart, predictions and remedies as context, with per-user conversation history, a clear-history endpoint and suggested questions.
- **JSON API** — Endpoints for horoscope data and AI chat, usable from other clients.

## Tech Stack

| Area | Technology |
|------|------------|
| Backend | Python, Flask |
| Astronomical calculations | Swiss Ephemeris (`pyswisseph`) |
| AI assistant | OpenAI API |
| Sessions | Flask sessions (filesystem), optional Redis |
| Configuration | `python-dotenv` |
| Frontend | Jinja2 templates, CSS |

## Project Structure

```
.
├── app.py                # Flask app, chart calculations and routes
├── astrology_utils.py    # Matching, prediction signals and remedies
├── ai_assistant.py       # AI assistant (VedicAIAssistant) and question suggestions
├── requirements.txt      # Python dependencies
├── templates/            # HTML templates (Jinja2)
└── static/css/           # Stylesheets
```

## Getting Started

### Prerequisites

- Python 3.9 or later
- An OpenAI API key (only required for the AI chat feature)

### Installation

```bash
git clone https://github.com/Chowdri-Furkhan07/astro.git
cd astro

python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate

pip install flask pyswisseph python-dotenv
pip install -r requirements.txt
```

### Configuration

Create a `.env` file in the project root:

```
SECRET_KEY=your-secret-key
OPENAI_API_KEY=your-openai-api-key
```

If `SECRET_KEY` is not set, the app falls back to a development default. Always set your own value in production. If the AI assistant cannot initialise, the rest of the application still runs and the chat endpoint returns a 503 response.

### Run

```bash
python app.py
```

Open `http://localhost:5000` in your browser.

## Routes

### Pages

| Route | Method | Description |
|-------|--------|-------------|
| `/` | GET | Home page with the birth details form |
| `/chart` | POST | Generates the birth chart, predictions and remedies |
| `/compatibility` | GET | Kundali matching form |
| `/match` | POST | Runs compatibility matching and shows results |
| `/predictions/<chart_data>` | GET | Predictions page (placeholder) |
| `/ai-chat` | GET | AI Astrologer chat interface |
| `/about` | GET | About page |

### API

| Route | Method | Description |
|-------|--------|-------------|
| `/api/horoscope` | POST | Returns chart data as JSON |
| `/api/chat` | POST | Sends a message to the AI astrologer |
| `/api/chat/clear` | POST | Clears the current conversation history |
| `/api/chat/suggestions` | GET | Returns suggested questions, personalised if a chart exists |

### Example: `/api/horoscope`

```bash
curl -X POST http://localhost:5000/api/horoscope \
  -H "Content-Type: application/json" \
  -d '{
    "year": 2000,
    "month": 1,
    "day": 1,
    "hour": 10,
    "minute": 30,
    "second": 0,
    "latitude": 12.9716,
    "longitude": 77.5946,
    "timezone": 5.5
  }'
```

`hour`, `minute` and `second` are local time. `timezone` is the offset from UTC in hours and defaults to `5.5` (IST).

### Example: `/api/chat`

```bash
curl -X POST http://localhost:5000/api/chat \
  -H "Content-Type: application/json" \
  -d '{"message": "What does my chart say about career?"}'
```

The assistant uses the chart stored in the session by the last `/chart` request, so generate a chart in the same browser session first for personalised answers.

## Known Limitations

- Yoga and Karana in the Panchanga are simplified placeholders, not full calculations.
- The `/predictions/<chart_data>` page is a placeholder; predictions are currently shown on the chart page.
- Kundali matching takes birth dates and a shared location only; birth time is not used.
- Predictions are generated from rule-based signals and AI responses, and are intended for entertainment and educational purposes only.

## Roadmap

- Complete Yoga and Karana calculations
- Divisional charts and Vimshottari Dasha periods
- Birth time in Kundali matching
- Downloadable chart reports
- Automated tests for the calculation functions

## Disclaimer

This project is for educational and entertainment purposes. It does not provide professional, medical, legal or financial advice.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

## Author

**Chowdri Furkhan**

B.E. in Artificial Intelligence and Machine Learning, Bengaluru, India

[![GitHub](https://img.shields.io/badge/GitHub-Chowdri--Furkhan07-181717?logo=github&logoColor=white)](https://github.com/Chowdri-Furkhan07)

Feedback and suggestions are welcome. Open an issue on this repository to get in touch.
