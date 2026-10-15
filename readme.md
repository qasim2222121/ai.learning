# 🗺️ AI Learning Roadmap Generator

A Streamlit web app that creates a personalised, step-by-step learning roadmap using Google's Gemini Flash model.

You enter:

1. **The field** you want to learn (e.g. *Data Science*, *Web Development*, *Cybersecurity*)
2. **Your skill level**: Beginner, Intermediate or Advanced
3. **Your time**: how long you have (weeks or months) and how many hours per week you can study

The app returns a structured roadmap with an overview, prerequisites, time-boxed phases (goals, topics, practice projects, resources, milestones), a capstone project, a progress checklist, study tips, and what to learn next. You can download the result as a Markdown file.

## Features

- Roadmap scaled to your level and your total available hours
- Phase granularity adapts to the plan length (weekly for short plans, monthly for long ones)
- Input validation and friendly error messages (bad key, quota limits, blocked responses, etc.)
- Result stays on screen after downloading (stored in `st.session_state`)
- Built-in cooldown between requests to protect your API quota
- Model name is configurable, so you are not locked to one Gemini version

## Project structure

```
.
├── app.py             # Streamlit app (UI + Gemini call)
├── requirements.txt   # Python dependencies
└── README.md
```

## Run locally

**Prerequisites:** Python 3.10+ and a free Gemini API key from [Google AI Studio](https://aistudio.google.com/apikey).

```bash
# 1. Clone and enter the project
git clone https://github.com/<your-username>/<your-repo>.git
cd <your-repo>

# 2. Create and activate a virtual environment
python -m venv venv
source venv/bin/activate          # Windows: venv\Scripts\activate

# 3. Install dependencies
pip install -r requirements.txt
```

**Provide your API key** using any ONE of these options:

*Option A: Streamlit secrets (recommended)*

Create the file `.streamlit/secrets.toml`:

```toml
GEMINI_API_KEY = "your-api-key-here"
```

*Option B: environment variable*

```bash
export GEMINI_API_KEY="your-api-key-here"      # Windows PowerShell: $env:GEMINI_API_KEY="your-api-key-here"
```

*Option C: paste it into the sidebar* when the app opens.

**Start the app:**

```bash
streamlit run app.py
```

Then open http://localhost:8501.

## Configuration

| Setting          | Where                              | Default               | Purpose                          |
|------------------|------------------------------------|-----------------------|----------------------------------|
| `GEMINI_API_KEY` | secrets / env var / sidebar        | none (required)       | Your Gemini API key              |
| `GEMINI_MODEL`   | secrets / env var / sidebar        | `gemini-flash-latest` | Which Gemini Flash model to use  |

`gemini-flash-latest` is an alias that points at the newest Flash model. If you want to pin a specific version, set `GEMINI_MODEL` (for example, `gemini-2.5-flash`). Check the [Gemini models page](https://ai.google.dev/gemini-api/docs/models) for current model names.

## Deploy on Streamlit Community Cloud

1. Push this project to a **GitHub** repository (make sure `.streamlit/secrets.toml` is **not** committed).
2. Go to [share.streamlit.io](https://share.streamlit.io) and sign in with GitHub.
3. Click **Create app** → choose your repository, branch (`main`) and main file path (`app.py`).
4. Open **Advanced settings → Secrets** and paste:
   ```toml
   GEMINI_API_KEY = "your-api-key-here"
   ```
5. Click **Deploy**. After a minute or two you get a public URL.

## Security notes

- **Never commit your API key.** Keep it in `.streamlit/secrets.toml` (git-ignored) or in Streamlit Cloud's Secrets.
- If you deploy the app publicly with *your* key, anyone using it spends *your* quota. Consider leaving the key out of Secrets so each visitor pastes their own in the sidebar, or set usage limits on your key in Google AI Studio.

## Troubleshooting

| Problem | Fix |
|---------|-----|
| "Model not found" | Change the model name in **Sidebar → Advanced settings** or set `GEMINI_MODEL`. |
| "Gemini rejected the request" | Check that the API key is correct and active. |
| "Rate limit or quota reached" | Wait a minute; the free tier has per-minute and daily limits. |
| Empty / blocked response | Rephrase the field (e.g. "Machine Learning" instead of a long sentence). |
| `ModuleNotFoundError` | Activate your virtual environment and run `pip install -r requirements.txt`. |

## Tech stack

- [Streamlit](https://streamlit.io) for the UI
- [Google Gen AI SDK (`google-genai`)](https://pypi.org/project/google-genai/) for Gemini access

## License

MIT. Use it, modify it, and learn from it.
