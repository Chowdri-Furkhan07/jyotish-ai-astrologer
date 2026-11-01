# Jyotish AI Astrologer

A Flask web app for Vedic (Jyotish) astrology. It generates sidereal birth charts, matches two charts for compatibility (Kundali Milan), gives predictions and remedies, and includes an AI chat assistant that answers questions using your own chart.

## Features

- **Birth chart (Kundali):** Sidereal positions using the Lahiri ayanamsa, calculated with Swiss Ephemeris. Ascendant, houses (whole-sign, from the Lagna) and Rahu/Ketu are all sidereal.
- **Kundali matching:** Compatibility scoring between two charts using the traditional Ashtakoota factors (including Bhakoot, Nadi, Tara and Graha Maitri). Birth time is optional and defaults to noon. Vashya and Yoni use simplified rules.
- **Panchanga details:** Yoga and Karana names, plus Manglik and house placements computed from the Lagna.
- **Predictions and remedies:** Chart-based predictions with suggested remedies.
- **AI chat assistant:** Ask questions in plain language; answers draw on your generated chart. Chart context is stored server-side (in memory or Redis), and the browser cookie holds only an opaque ID.
- **Production basics:** Input validation, structured logging, request IDs, security headers, rate limiting and a `/health` endpoint.

## Tech Stack

| Area | Tools |
|---|---|
| Backend | Python, Flask, Gunicorn |
| Astronomy | pyswisseph (Swiss Ephemeris) |
| AI | OpenAI Python SDK (v1 client) |
| State and rate limiting | Redis, Flask-Limiter |
| Config | python-dotenv |
| Tooling | pytest, ruff, GitHub Actions, Docker, Docker Compose |

## Project Structure

```
.
├── app.py              # Local development entry point
├── wsgi.py             # WSGI entry point (gunicorn wsgi:app)
├── vedic_astro/        # Application package (core / services / web)
├── templates/          # HTML templates
├── static/css/         # Stylesheets
├── tests/              # Pytest suite
├── Screenshots/        # App screenshots
├── Dockerfile
├── docker-compose.yml  # Web app + Redis
├── Makefile
├── requirements.txt
└── requirements-dev.txt
```

## Getting Started

### Prerequisites

- Python 3.x
- An OpenAI API key (for the AI chat assistant)
- Docker (optional)

### Local setup

```bash
git clone https://github.com/Chowdri-Furkhan07/jyotish-ai-astrologer.git
cd jyotish-ai-astrologer

python -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate
pip install -r requirements.txt
```

Create a `.env` file in the project root:

```env
# Replace with the variable names your settings module reads
OPENAI_API_KEY=<your-key>
APP_ENV=development
```

Run the app:

```bash
python app.py
```

It starts at `http://127.0.0.1:5000`. `HOST` and `PORT` can be overridden through environment variables.

### Docker

```bash
docker compose up --build
```

This starts the web app on `http://localhost:8000` with Redis for chart context and rate limiting. It reads settings from `.env`.

## Development

```bash
make install   # create venv and install dev dependencies
make run       # run locally
make test      # pytest with coverage
make lint      # ruff check and format check
make format    # auto-fix and format
make docker    # docker compose up --build
```

## Screenshots

See the [`Screenshots`](./Screenshots) folder.

## Notes

Astrological output is for informational and entertainment purposes only and is not a substitute for professional advice.

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md). Security issues: see [SECURITY.md](SECURITY.md).

## License

MIT. See [LICENSE](LICENSE).
