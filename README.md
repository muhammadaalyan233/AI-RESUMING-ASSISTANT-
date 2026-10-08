# AI-RESUMING-ASSISTANT-
# 📄 Resume ATS Analyzer

A Streamlit app that scores a resume for ATS (Applicant Tracking System) compatibility and gives specific improvements, powered by Google Gemini Flash.

## Features
- Upload a resume as **PDF, DOCX or TXT**
- **ATS score (0-100)** computed from five weighted categories:
  Formatting (20%), Keywords (25%), Impact (25%), Structure (15%), Clarity (15%)
- Prioritized improvements (High / Medium / Low)
- Found vs. missing keywords - paste a job description for targeted matching
- Before/after rewrites of weak bullet points
- Downloadable Markdown report

## Run locally

```bash
git clone https://github.com/<your-username>/resume-ats-analyzer.git
cd resume-ats-analyzer

python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate
pip install -r requirements.txt
```

Get a free API key at https://aistudio.google.com/apikey, then provide it in **one** of these ways:

1. Paste it into the sidebar when the app runs, or
2. Environment variable: `export GEMINI_API_KEY="your-key"` (Windows PowerShell: `$env:GEMINI_API_KEY="your-key"`), or
3. Create `.streamlit/secrets.toml`:
   ```toml
   GEMINI_API_KEY = "your-key"
   ```

Start the app:

```bash
streamlit run app.py
```

## Configuration
| Setting | How | Default |
|---|---|---|
| API key | sidebar / `GEMINI_API_KEY` env var / Streamlit secret | - |
| Model | sidebar / `GEMINI_MODEL` env var | `gemini-3.5-flash` |

Google renames and retires models over time. If you get a "model not found" error, check the current Flash model name in the [Gemini docs](https://ai.google.dev/gemini-api/docs/models) and enter it in the sidebar.

## Push to GitHub

```bash
# inside the project folder (app.py, requirements.txt, README.md)
printf "venv/\n__pycache__/\n.env\n.streamlit/secrets.toml\n" > .gitignore

git init
git add .
git commit -m "Initial commit: Resume ATS Analyzer"
git branch -M main
```

Create an empty repository on https://github.com/new (no README, no .gitignore), then:

```bash
git remote add origin https://github.com/<your-username>/resume-ats-analyzer.git
git push -u origin main
```

> Never commit your API key. The `.gitignore` above keeps `secrets.toml` out of the repo.

## Deploy on Streamlit Community Cloud (free)

1. Go to https://share.streamlit.io and sign in with GitHub.
2. Click **Create app** → **Deploy a public app from GitHub**.
3. Choose your repository, branch `main`, and main file path `app.py`.
4. Open **Advanced settings → Secrets** and add:
   ```toml
   GEMINI_API_KEY = "your-key"
   ```
5. Click **Deploy**. After a minute or two you'll get a public `*.streamlit.app` URL.

Any later `git push` to `main` redeploys automatically. Secrets can be edited anytime under the app's **Settings → Secrets**.

> If the app is public and uses your key, anyone with the link spends your quota. Consider leaving the secret out so visitors enter their own key in the sidebar, or restrict the app's viewers in Streamlit's sharing settings.

## Notes & limitations
- Scanned/image-only PDFs can't be read (ATS systems can't read them either) - use a text-based PDF or DOCX.
- The score is an AI-assisted estimate; real ATS products differ. Use it as guidance, not a guarantee.
- Resume text is sent to the Gemini API; the app itself stores nothing.
