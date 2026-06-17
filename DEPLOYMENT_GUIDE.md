# Deployment Guide

## ⚠️ Important: Your App Type

Your Flask backend + HTML/JS frontend app requires a **Python-capable platform**. 
- ❌ Streamlit Cloud does NOT support Flask apps
- ❌ Vercel minimal support for Python (limited)
- ✅ Recommended platforms (with native Python support)

---

## Option 1: Railway (Recommended)

Railway has native Flask support and is simple to set up.

### Setup:
1. Push to GitHub
2. Connect at [railway.app](https://railway.app)
3. Select Python environment
4. Railway auto-detects `requirements.txt` and `Procfile`

### Create `Procfile`:
```
web: gunicorn --bind 0.0.0.0:$PORT --timeout 120 app.app:app
```

---

## Option 2: Render

Similar to Railway with good free tier.

### Setup:
1. Create `render.yaml`:
```yaml
services:
  - type: web
    name: bcca
    env: python
    plan: free
    buildCommand: pip install -r requirements-deploy.txt && python -m app.model
    startCommand: gunicorn --bind 0.0.0.0:$PORT --timeout 120 app.app:app
```

---

## Option 3: Vercel (Limited Support)

Requires serverless function setup - NOT recommended for your app.

### If you must use Vercel:
1. Use `vercel.json` in root
2. Create `api/index.py` to wrap Flask app
3. Max execution time: 60 seconds (your model training may timeout)

---

## Option 4: Heroku (Paid)

Classic choice for Flask apps.

### Setup:
1. Create `Procfile` (same as Railway)
2. Push to GitHub
3. Connect Heroku app
4. Set `PYTHON_VERSION=3.10` in config

---

## Files Created for Deployment

- **`requirements-deploy.txt`**: Lightweight production dependencies
- **`vercel.json`**: For Vercel (if needed)
- **`.streamlit/config.toml`**: Streamlit config (for reference only)
- **Fixed `pyproject.toml`**: Corrected TOML syntax

---

## Environment Variables

Set these on your platform:
```
SECRET_KEY=your-secret-key-here
FLASK_ENV=production
```

---

## What to Keep as "Application Preset"

For **Vercel** preset (if you must use it):
- Framework: **Python**
- Python Version: **3.10**
- Build Command: `pip install -r requirements-deploy.txt && python -m app.model`
- Output Directory: `.`
- Install Command: `pip install -r requirements-deploy.txt`

For **Railway/Render**:
- Python 3.10
- requirements: `requirements-deploy.txt`
- Command: `gunicorn --bind 0.0.0.0:$PORT --timeout 120 app.app:app`

---

## Deploy Issues Fixed

✅ Fixed TOML syntax error in `pyproject.toml` (line 99)  
✅ Removed problematic dev dependencies (trufflehog, bandit, semgrep)  
✅ Updated pandas to `>=2.2.0` (Python 3.14 compatible)  
✅ Created lightweight `requirements-deploy.txt`  
