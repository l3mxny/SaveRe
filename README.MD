# SaveRe

A full-stack web application to help you reduce food waste — scan receipts with OCR, track expiration dates, and generate AI-powered recipes from what you have on hand.

## Features

- **Receipt scanning** — upload a photo of your grocery receipt and items are extracted automatically via Tesseract OCR + Google Gemini AI
- **Manual entry** — add groceries by name and quantity without a receipt
- **Expiration tracking** — ingredients are color-coded by urgency (critical → excellent) so you always know what to use first
- **Calendar view** — see what's expiring week by week in a monthly calendar overlay
- **AI recipe generation** — generate recipes based on your current ingredients using Google Gemini
- **User accounts** — JWT-based authentication with sign up, sign in, and password change

## Tech Stack

| Layer | Technologies |
|---|---|
| **Frontend** | Next.js 16, React 19, TypeScript, Tailwind CSS 4, Framer Motion, PWA |
| **Backend** | FastAPI, Python 3.11+, Tesseract OCR, Google Gemini AI (gemini-1.5-flash) |
| **Auth** | JWT tokens |
| **Infrastructure** | Docker, Docker Compose |

## Project Structure

```
grocery-management/
├── apps/
│   ├── backend/        # FastAPI Python backend
│   │   ├── server.py
│   │   ├── auth.py / auth_routes.py
│   │   ├── ocr.py / receipt_parser.py
│   │   ├── groceries.py / recipes.py
│   │   ├── models.py / database.py
│   │   ├── Dockerfile
│   │   ├── docker-compose.yml
│   │   └── requirements.txt
│   └── frontend/       # Next.js React app
│       └── app/
│           ├── components/   # auth, calendar, ingredients, recipes, profile
│           ├── hooks/        # useFileUpload, useReceiptUpload
│           └── (pages)       # ingredients, recipes, home
└── README.MD
```

## Getting Started

### Prerequisites

- [Node.js 18+](https://nodejs.org/)
- [Python 3.11+](https://www.python.org/)
- [Docker and Docker Compose](https://docs.docker.com/get-docker/) (for containerized setup)
- [Tesseract OCR](https://github.com/tesseract-ocr/tesseract) (for local backend setup)
  - macOS: `brew install tesseract`
  - Ubuntu/Debian: `sudo apt-get install tesseract-ocr`
  - Windows: [Download installer](https://github.com/UB-Mannheim/tesseract/wiki)

### Environment Variables

Create a `.env` file in `apps/backend/`:

```env
GEMINI_API_KEY=your_gemini_api_key_here
LLM_MODEL=gemini-1.5-flash
LLM_MAX_TOKENS=1000
LLM_TEMPERATURE=0.7
MAX_FILE_SIZE_MB=10
ALLOWED_EXTENSIONS=jpg,jpeg,png,pdf
OCR_PREPROCESS_METHOD=thresh
```

---

## Running with Docker

```bash
cd apps/backend
docker-compose up --build
```

The API will be available at `http://localhost:8000`. The Docker setup includes hot-reload.

```bash
docker-compose down       # stop
docker-compose logs -f    # view logs
```

Then start the frontend separately:

```bash
cd apps/frontend
npm install
npm run dev
```

The frontend will be available at `http://localhost:3000`.

---

## Running Locally (without Docker)

### Backend

```bash
cd apps/backend
python3 -m venv .venv
source .venv/bin/activate   # Windows: .venv\Scripts\activate
pip install -r requirements.txt
python3 server.py
```

### Frontend

```bash
cd apps/frontend
npm install
npm run dev
```

---

## API Endpoints

- `GET /` — API info
- `GET /health` — health check
- `POST /api/auth/register` — register a new user
- `POST /api/auth/login` — log in and receive a JWT
- `GET /api/auth/me` — get current user profile
- `GET /api/groceries/` — list all groceries for the authenticated user
- `POST /api/groceries/recipe` — generate a recipe from current groceries
- `POST /api/receipt/upload` — upload a receipt image for OCR parsing
- `POST /api/receipt/analyze-text` — submit manually entered grocery text
- `GET /docs` — interactive API documentation (Swagger UI)

---

## Troubleshooting

### Tesseract Not Found

Ensure Tesseract is installed and available in your `PATH`. Run `tesseract --version` to verify.

### Permission Issues with Uploads (Docker)

```bash
chmod 777 apps/backend/uploads
```
