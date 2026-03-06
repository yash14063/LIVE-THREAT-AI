# Vercel Deployment Guide - ThreatSense OS Frontend

## Overview
Your **frontend** (HTML/CSS/JS) deploys to Vercel. Your **backend** (Python/FastAPI) deploys to a separate service.

## Architecture
```
┌─────────────────────────┐
│  Vercel (CDN)           │  ← Frontend (static site)
│  public/index.html      │     https://yourdomain.vercel.app
└──────────────┬──────────┘
               │ API Calls & WebSocket
               ▼
┌─────────────────────────┐
│  Backend Service        │  ← FastAPI Server
│  (Railway/Render/etc)   │     https://backend.railway.app
│  main.py                │
└─────────────────────────┘
```

---

## Step 1: Prepare for Vercel Deployment

### Structure Already Ready ✅
Your project is already Vercel-ready:
- `public/index.html` - Main dashboard (static)
- `vercel.json` - Deployment configuration
- `.vercelignore` - Excludes backend files

### Files Already in Place
```
live-threat-detection/
├── public/
│   └── index.html          ← Deploys to Vercel
├── vercel.json             ← Vercel config
├── .vercelignore           ← Excludes Python files
├── main.py                 ← Keep locally (deploy separately)
├── database.py             ← Keep locally
└── ... other backend files
```

---

## Step 2: Deploy Frontend to Vercel

### Option A: Deploy via Vercel CLI (Recommended)

**1. Install Vercel CLI:**
```bash
npm install -g vercel
```

**2. Navigate to project:**
```bash
cd c:\Users\yashs\OneDrive\Documents\GitHub\live-threat-detection
```

**3. Deploy:**
```bash
vercel
```

**4. Follow prompts:**
- Link to existing project or create new
- Confirm project settings
- Done! Frontend is live at `https://your-project.vercel.app`

### Option B: Deploy via GitHub (Recommended for CI/CD)

**1. Push to GitHub:**
```bash
git init
git add .
git commit -m "Deploy frontend to Vercel"
git remote add origin https://github.com/yourusername/live-threat-detection.git
git push -u origin main
```

**2. Connect to Vercel:**
- Go to [vercel.com](https://vercel.com)
- Click "New Project"
- Import from GitHub
- Select `live-threat-detection` repo
- Click "Deploy"

**3. Automatic deployments:**
- Every push to main branch = auto-deploy ✅

---

## Step 3: Deploy Backend (Separate Service)

FastAPI cannot run on Vercel's free tier. Choose one:

### Option A: Railway (Recommended - Easiest)
**Cost:** $5/month free credit, pay-as-you-go after

1. **Create Railway account:** [railway.app](https://railway.app)
2. **Connect GitHub repo**
3. **Set environment variables** in Railway dashboard:
   - `DATABASE_URL=sqlite:///./threats.db`
   - `SECRET_KEY=your-secret-key`
   - `DEBUG=false`
4. **Deploy automatically** with git push

**Backend runs at:** `https://your-project.up.railway.app`

### Option B: Render (Alternative)
**Cost:** Free tier available with limitations

1. Go to [render.com](https://render.com)
2. Create New Web Service
3. Connect GitHub
4. Set `start command`: `uvicorn main:app --host 0.0.0.0 --port 8000`
5. Deploy

### Option C: AWS Lambda (Serverless)
Requires rewriting FastAPI as serverless functions (more complex)

### Option D: Local/VPS (Self-hosted)
- Keep FastAPI running on your own server
- Update `BACKEND_URL` in frontend

---

## Step 4: Connect Frontend to Backend

### Automatic Detection (Already Implemented)
Your `public/index.html` automatically detects backend URL:

```javascript
// Lines 230-236 in public/index.html
const BACKEND_URL = window.location.origin === 'http://localhost:3000' 
  ? 'http://localhost:8000' 
  : window.location.origin;
```

### Manual Configuration (If Needed)
Edit `public/index.html` line 230-236:

```javascript
// Change this:
const BACKEND_URL = 'https://your-backend.railway.app';
const WS_URL = BACKEND_URL.replace('http://', 'ws://').replace('https://', 'wss://');
```

Then redeploy to Vercel.

---

## Step 5: CORS Configuration for Vercel

Your backend needs to allow Vercel domain. Update `main.py`:

```python
# Line 36-47 in main.py
app.add_middleware(
    CORSMiddleware,
    allow_origins=[
        "http://localhost:3000",
        "http://localhost:8000",
        "https://your-project.vercel.app",    # ← Add your Vercel URL
        "https://your-backend.railway.app",    # ← Add your backend URL
    ],
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)
```

---

## Step 6: Environment Variables for Vercel

No backend secrets needed on Vercel (it's just static frontend).

**But for Railway backend**, set:
```
DATABASE_URL=postgresql://...  (or sqlite if local)
SECRET_KEY=very-secret-key-here
DEBUG=false
MEDIAPIPE_MODEL_COMPLEXITY=2
YOLO_MODEL_NAME=yolov8n
YOLO_CONFIDENCE=0.5
```

---

## Deployment Checklist

- [ ] **Frontend on Vercel:**
  - [ ] Clone repo
  - [ ] `vercel deploy` or connect GitHub
  - [ ] Get `https://your-project.vercel.app` URL
  - [ ] Test at `/` (should show ThreatSense dashboard)

- [ ] **Backend on Railway/Render:**
  - [ ] Create account
  - [ ] Deploy `main.py`
  - [ ] Get backend URL (e.g., `https://your-api.railway.app`)
  - [ ] Set environment variables
  - [ ] Test `/api/health` endpoint

- [ ] **Connect Frontend to Backend:**
  - [ ] Update BACKEND_URL in `public/index.html`
  - [ ] Redeploy to Vercel
  - [ ] Check browser console for WebSocket connection

- [ ] **Test End-to-End:**
  - [ ] Open dashboard at `https://your-project.vercel.app`
  - [ ] Authenticate with phone OTP
  - [ ] Select mode (HOME or PUBLIC)
  - [ ] Check event log for backend connection status

---

## Troubleshooting

### **"Cannot connect to backend"**
- Check BACKEND_URL in `public/index.html`
- Verify backend service is running
- Open DevTools → Network tab → check WebSocket

### **"CORS error"**
- Add Vercel URL to `allow_origins` in `main.py`
- Redeploy backend

### **"Vercel deployment fails"**
- Check `vercel.json` exists
- Verify `public/` folder has `index.html`
- Run `vercel --version` to confirm CLI works

### **"WebSocket connection fails"**
- Ensure backend uses `wss://` for HTTPS
- Check CORS includes WebSocket origins

---

## Quick Links

- **Vercel Dashboard:** https://vercel.com/dashboard
- **Railway Dashboard:** https://railway.app/dashboard
- **Monitor Vercel Logs:** https://vercel.com/docs/concepts/monitoring/logs
- **Monitor Railway Logs:** Check in Railway dashboard

---

## File Reference

- **Frontend Config:** [vercel.json](vercel.json)
- **Frontend Code:** [public/index.html](public/index.html)
- **Backend Code:** [main.py](main.py)
- **Exclude List:** [.vercelignore](.vercelignore)

---

## What's Next?

1. **Deploy frontend to Vercel** (5 minutes)
2. **Deploy backend to Railway** (10 minutes)
3. **Connect backend URL** (2 minutes)
4. **Test dashboard** (5 minutes)
5. **Connect real cameras** (setup RTSP streams)
6. **Monitor in production** (view logs, metrics)

---

**Total Setup Time:** ~25 minutes

**Cost:** 
- Vercel: Free for hobby projects
- Railway: $5 free/month, then $0.10/hour after
- Total: ~$15-20/month for small deployment
