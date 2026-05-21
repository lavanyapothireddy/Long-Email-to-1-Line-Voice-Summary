# VoiceBrief — Long Email to 1-Line Voice Summary

Paste any email. Get one voice-ready sentence powered by Groq + LLaMA 3.3 70B.

## Project Structure

```
email-voice-summary/
├── app.py               # Flask backend + Groq API
├── templates/
│   └── index.html       # Frontend UI
├── requirements.txt
├── render.yaml          # Render deploy config
├── Procfile
├── .python-version
└── .gitignore
```

## Local Development

1. **Clone the repo**
   ```bash
   git clone https://github.com/YOUR_USERNAME/YOUR_REPO.git
   cd YOUR_REPO
   ```

2. **Install dependencies**
   ```bash
   pip install -r requirements.txt
   ```

3. **Set your Groq API key**
   ```bash
   export GROQ_API_KEY=your_groq_api_key_here
   ```
   Get a free key at https://console.groq.com

4. **Run locally**
   ```bash
   python app.py
   ```
   Visit http://localhost:5000

## Deploy to Render

### Option A — Via render.yaml (recommended)

1. Push this repo to GitHub
2. Go to https://render.com → New → Blueprint
3. Connect your GitHub repo
4. Render reads `render.yaml` automatically
5. Set the `GROQ_API_KEY` environment variable in the Render dashboard
6. Deploy ✅

### Option B — Manual setup

1. Go to https://render.com → New → Web Service
2. Connect your GitHub repo
3. Set:
   - **Runtime**: Python 3
   - **Build Command**: `pip install -r requirements.txt`
   - **Start Command**: `gunicorn app:app --bind 0.0.0.0:$PORT --workers 2 --timeout 60`
4. Add environment variable:
   - Key: `GROQ_API_KEY`
   - Value: your key from https://console.groq.com
5. Deploy ✅

## API

### POST /summarize

**Request:**
```json
{ "email": "Dear team, I wanted to follow up..." }
```

**Response:**
```json
{
  "summary": "Sarah needs the Q3 budget report by Friday for the board presentation.",
  "char_count": 842,
  "model": "llama-3.3-70b-versatile"
}
```

**Limits:** 20–15,000 characters per email

### GET /health
Returns `{"status": "ok"}` — used by Render health checks.

## Features

- One-sentence voice-friendly AI summaries
- Read aloud via browser Text-to-Speech
- Copy to clipboard
- Keyboard shortcut: Ctrl+Enter / Cmd+Enter
- Character count with warnings
- Mobile responsive
