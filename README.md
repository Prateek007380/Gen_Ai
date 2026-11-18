# AI Tutor Project

This project provides two FastAPI services for an AI-powered tutor:

- **Module JWT** - user registration, login, and JWT authentication backed by MongoDB.
- **Module 1** - authenticated AI-tutor features using Google Gemini for text, image, audio, video, and PDF analysis.

## Project Structure

```text
S54 Project/
├── Module JWT/
│   ├── main.py
│   ├── requirements.txt
│   └── .env.example
└── Module 1/
    ├── main.py
    ├── schema.py
    ├── requirements.txt
    └── .env.example
```

## Requirements

- Python 3.10 or later
- MongoDB
- A Google Gemini API key

## Configuration

Create a `.env` file in each module directory.

### `Module JWT/.env`

Copy the values from `Module JWT/.env.example` and configure:

```env
SECRET_KEY=replace_with_a_long_random_secret
ALGORITHM=HS256
MONGO_URI=mongodb://localhost:27017
```

### `Module 1/.env`

Copy the values from `Module 1/.env.example` and configure:

```env
SECRET_KEY=replace_with_the_same_secret_used_by_module_jwt
ALGORITHM=HS256
GEMINI_API_KEY=your_gemini_api_key
CHAT_MODEL=your_gemini_model_name
```

Never commit `.env` files or API keys.

## Installation

Create and activate a virtual environment for each module, then install its dependencies.

### Windows PowerShell

```powershell
cd "Module JWT"
python -m venv venv
.\venv\Scripts\Activate.ps1
pip install -r requirements.txt
```

```powershell
cd "..\Module 1"
python -m venv venv
.\venv\Scripts\Activate.ps1
pip install -r requirements.txt
```

## Running the Services

Start MongoDB first, then run each service from its module directory.

### JWT Service

```powershell
cd "Module JWT"
.\venv\Scripts\Activate.ps1
uvicorn main:app --reload --port 8000
```

### AI Tutor Service

```powershell
cd "Module 1"
.\venv\Scripts\Activate.ps1
uvicorn main:app --reload --port 8001
```

Interactive API documentation is available at:

- JWT service: `http://127.0.0.1:8000/docs`
- AI Tutor service: `http://127.0.0.1:8001/docs`

## API Endpoints

### JWT Service

| Method | Endpoint | Description |
|---|---|---|
| `POST` | `/signup` | Register a user |
| `POST` | `/signin` | Sign in and receive an access token |
| `GET` | `/protected` | Verify the JWT and access a protected route |

### AI Tutor Service

All AI Tutor endpoints require the JWT returned by `/signin` in the `token` request header.

| Method | Endpoint | Description |
|---|---|---|
| `POST` | `/generate_text` | Generate study material from a topic, course, and student level |
| `POST` | `/analyze_image` | Analyze an academic JPEG or PNG image |
| `POST` | `/evaluate_audio_answer` | Analyze an MP3 or WAV audio answer |
| `POST` | `/analyze_video` | Analyze an MP4 or AVI video and generate MCQs |
| `POST` | `/analyze_pdf` | Analyze a PDF and generate MCQs |

Use the Swagger UI at `/docs` to inspect request schemas and try the endpoints.

## Authentication Flow

1. Register with `POST /signup`.
2. Sign in with `POST /signin`.
3. Copy the returned `access_token`.
4. Send it in the `token` header when calling protected AI Tutor endpoints.

The `SECRET_KEY` and `ALGORITHM` values must match in both module environments.
