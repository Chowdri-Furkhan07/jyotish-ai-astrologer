# Jyotish AI Astrologer

![Python](https://img.shields.io/badge/Python-3776AB?logo=python&logoColor=white)
![Flask](https://img.shields.io/badge/Flask-000000?logo=flask&logoColor=white)
![Swiss Ephemeris](https://img.shields.io/badge/Swiss%20Ephemeris-pyswisseph-blue)
![Docker](https://img.shields.io/badge/Docker-2496ED?logo=docker&logoColor=white)
![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)

A Flask web application for Vedic (Jyotish) astrology. It generates sidereal birth charts, performs Kundali compatibility matching, provides predictions and remedies, and includes an AI chat assistant that answers questions using the user's own chart.

## Table of Contents

- [Features](#features)
- [Screenshots](#screenshots)
- [Tech Stack](#tech-stack)
- [Project Structure](#project-structure)
- [Getting Started](#getting-started)
- [Configuration](#configuration)
- [Development](#development)
- [Testing and Code Quality](#testing-and-code-quality)
- [Deployment](#deployment)
- [Calculation Notes](#calculation-notes)
- [Disclaimer](#disclaimer)
- [Contributing](#contributing)
- [Security](#security)
- [Author](#author)
- [License](#license)

## Features

- **Birth chart (Kundali):** Sidereal planetary positions using the Lahiri ayanamsa, calculated with Swiss Ephemeris. The ascendant, houses and Rahu/Ketu are all computed sidereally, so the whole chart is consistent.
- **Kundali matching:** Compatibility scoring between two charts based on the traditional Ashtakoota factors, including Bhakoot, Nadi, Tara and Graha Maitri. Birth time is optional for matching and defaults to noon.
- **Panchanga details:** Yoga and Karana names, along with Manglik status and house placements computed from the Lagna using whole-sign houses.
- **Predictions and remedies:** Chart-based predictions with suggested remedies.
- **AI chat assistant:** Ask questions in plain language and receive answers grounded in your generated chart. Chart context is stored server-side (in memory or Redis); the browser cookie carries only an opaque identifier.
- **Production-oriented design:** Input validation, structured logging, request IDs, security headers, rate limiting and a `/health` endpoint.

## Screenshots

### Home

![Cosmic Blueprint Home](Screenshots/Cosmic%20Blueprint_Home.png)

### Kundali Matching

![Kundali Matching](Screenshots/Kundali%20Matching.png)

### Cosmic Sage (AI Chat)

![Cosmic Sage](Screenshots/Cosmic%20Sage.png)

### About

![About Section](Screenshots/About%20Section.png)

## Tech Stack

| Area | Tools |
|---|---|
| Backend | Python, Flask, Gunicorn |
| Astronomical calculations | pyswisseph (Swiss Ephemeris) |
| AI integration | OpenAI Python SDK (v1 client) |
| State and rate limiting | Redis, Flask-Limiter |
| Configuration | python-dotenv |
| Tooling | pytest, ruff, GitHub Actions |
| Containerization | Docker, Docker Compose |

## Project Structure

```
.
├── app.py                 # Local development entry point
├── wsgi.py                # WSGI entry point (gunicorn wsgi:app)
├── vedic_astro/           # Application package (core / services / web)
├── templates/             # HTML templates
├── static/css/            # Stylesheets
├── tests/                 # Pytest suite
├── Screenshots/           # Application screenshots
├── Dockerfile
├── docker-compose.yml     # Web application and Redis
├── Makefile               # Common development commands
├── pyproject.toml
├── requirements.txt       # Runtime dependencies
├── requirements-dev.txt   # Development dependencies
├── CHANGELOG.md
├── CONTRIBUTING.md
├── SECURITY.md
└── LICENSE
```

The `vedic_astro` package is organised into three layers: `core` (astrological calculations), `services` (chart, matching and AI logic) and `web` (routes and the application factory).

## Getting Started

### Prerequisites

- Python 3.x
- An OpenAI API key (required for the AI chat assistant)
- Docker and Docker Compose (optional, for containerized runs)

### Local setup

```bash
git clone https://github.com/Chowdri-Furkhan07/jyotish-ai-astrologer.git
cd jyotish-ai-astrologer

python -m venv .venv
source .venv/bin/activate          # Windows: .venv\Scripts\activate
pip install -r requirements.txt
```

Create a `.env` file in the project root (see [Configuration](#configuration)), then start the app:

```bash
python app.py
```

The application runs at `http://127.0.0.1:5000` by default. Set the `HOST` and `PORT` environment variables to change this.

### Run with Docker

```bash
docker compose up --build
```

This starts the web application on `http://localhost:8000` together with a Redis instance used for chart context and rate limiting. Settings are read from your `.env` file.

## Configuration

Settings are loaded from environment variables, and from a `.env` file in development.

| Variable | Purpose |
|---|---|
| `OPENAI_API_KEY` | API key for the AI chat assistant |
| `APP_ENV` | `development` enables debug mode; `production` is used in Docker |
| `REDIS_URL` | Redis connection string (set automatically by Docker Compose) |
| `WEB_CONCURRENCY` | Number of Gunicorn workers in Docker |
| `HOST` / `PORT` | Bind address and port for `python app.py` |

Example `.env`:

```env
OPENAI_API_KEY=your-api-key
APP_ENV=development
```

Never commit your `.env` file or API keys to version control.

## Development

Common tasks are available through `make`:

| Command | Description |
|---|---|
| `make install` | Create a virtual environment and install development dependencies |
| `make run` | Run the application locally |
| `make test` | Run the test suite with coverage |
| `make lint` | Run ruff checks and a format check |
| `make format` | Apply ruff fixes and formatting |
| `make docker` | Build and start the Docker Compose stack |

## Testing and Code Quality

The project includes a pytest suite with high coverage, linted and formatted with ruff, and a GitHub Actions workflow for continuous integration.

```bash
make test
make lint
```

## Deployment

For production, run the app with Gunicorn behind a reverse proxy:

```bash
gunicorn wsgi:app
```

Use Redis (`REDIS_URL`) in production so chart context and rate limits are shared across workers. The provided `Dockerfile` and `docker-compose.yml` set this up with `APP_ENV=production`.

## Calculation Notes

- All positions use the Lahiri (Chitrapaksha) ayanamsa with sidereal zodiac calculations.
- Houses follow the whole-sign system, counted from the Lagna.
- Kundali matching follows the traditional rules for Bhakoot, Nadi, Tara and Graha Maitri. Vashya and Yoni use simplified rules.
- When birth time is not supplied for matching, noon is assumed.

See [CHANGELOG.md](CHANGELOG.md) for the history of calculation fixes.

## Disclaimer

This application is intended for informational and entertainment purposes only. Astrological output is not a substitute for professional medical, legal, financial or psychological advice. AI-generated responses may contain errors.

## Contributing

Contributions are welcome. Please read [CONTRIBUTING.md](CONTRIBUTING.md) before opening a pull request, and run `make lint` and `make test` before submitting changes.

## Security

To report a vulnerability, follow the process in [SECURITY.md](SECURITY.md). Please do not open public issues for security problems.

## Author

**Chowdri Furkhan**
GitHub: [@Chowdri-Furkhan07](https://github.com/Chowdri-Furkhan07)

## License

This project is licensed under the MIT License. See [LICENSE](LICENSE) for details.
