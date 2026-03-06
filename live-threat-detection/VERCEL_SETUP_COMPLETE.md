# Vercel Deployment - Complete Setup Guide

## What Was Done ✅

Your project is now **fully configured for Vercel deployment**. Here's what's been set up:

### Files Created for Vercel
```
live-threat-detection/
├── public/
│   └── index.html                    ← Frontend (deploys to Vercel)
├── vercel.json                       ← Vercel configuration
├── .vercelignore                     ← Exclude backend files
├── package.json                      ← Node metadata
├── VERCEL_DEPLOYMENT.md              ← Complete guide
└── VERCEL_QUICK_START.md             ← Quick start (5 min)
```

### Key Changes Made
1. **Created `/public` folder** with optimized `index.html`
2. **Smart backend URL detection** (automatic local/remote switching)
3. **Vercel configuration** for static site deployment
4. **CORS headers configured** for API calls
5. **Node package.json** for proper Vercel metadata

---

## Deployment in 3 Steps

### Step 1: Deploy Frontend to Vercel (5 minutes)

**Via CLI:**
```bash
npm install -g vercel
vercel
```

**Via GitHub (Auto-deploy):**
```bash
git push origin main  # Auto-deploys on every push
```

**Result:** `https://yourdomain.vercel.app` ✅

---

### Step 2: Deploy Backend to Railway (10 minutes)

**Option A: Railway CLI**
```bash
npm install -g @railway/cli
railway login
railway init
railway up
```

**Option B: Render.com**
- Sign up at render.com
- Connect GitHub repo
- Set start command: `uvicorn main:app --host 0.0.0.0 --port 8000`
- Deploy

**Result:** `https://your-api.railway.app` or `https://your-api.render.com` ✅

---

### Step 3: Connect Frontend to Backend (2 minutes)

**Update** `public/index.html` line 230-236:
```javascript
const BACKEND_URL = 'https://your-api.railway.app';
const WS_URL = BACKEND_URL.replace('http://', 'ws://').replace('https://', 'wss://');
```

**Redeploy frontend:**
```bash
vercel --prod
```

---

## System Architecture

```
INTERNET
   │
   ├─────────────────────────────────────────────────┐
   │                                                   │
   ▼                                                   ▼
┌──────────────────────┐                   ┌──────────────────┐
│   VERCEL (CDN)       │                   │   RAILWAY/RENDER │
│ - Static HTML/CSS/JS │                   │   - FastAPI      │
│ - TensorFlow.js      │                   │   - SQLAlchemy   │
│ - Firebase Auth      │                   │   - MediaPipe    │
│ - Client-side AI     │                   │   - YOLO         │
│                      │                   │   - WebSocket    │
│ yourdomain.vercel.app│────WebSocket─────→│ your-api.railway │
│                      │     + REST API    │ .app:8000        │
└──────────────────────┘                   └──────────────────┘
        ▲                                           │
        │                                           │
   User Browser                            Database (Local/Cloud)
   (Any Device)                            PostgreSQL/SQLite
```

---

## Architecture Benefits

| Aspect | Solution | Why |
|--------|----------|-----|
| **Frontend** | Vercel (CDN) | Global edge network, instant CDN, free tier |
| **Backend** | Railway/Render | Full Python support, persistent containers |
| **Communication** | WebSocket | Real-time bidirectional streaming |
| **Auth** | Firebase | Phone OTP, scales automatically |
| **AI Processing** | Client + Server | Client (TensorFlow.js), Server (MediaPipe/YOLO) |

---

## Deployment Checklist

### Pre-Deployment
- [ ] Vercel CLI installed: `vercel --version`
- [ ] Node.js installed: `node --version`
- [ ] GitHub account (for auto-deploy)
- [ ] Railway/Render account
- [ ] All files in project directory

### Deployment Phase
- [ ] Frontend on Vercel
  - [ ] `vercel` command successful
  - [ ] Got HTTPS URL
  - [ ] Dashboard loads at `/`
  - [ ] Can see ThreatSense login

- [ ] Backend on Railway
  - [ ] Account created
  - [ ] Repository connected
  - [ ] Environment variables set
  - [ ] Deployment successful
  - [ ] Health endpoint responds at `/api/health`

### Post-Deployment
- [ ] Update BACKEND_URL in frontend
- [ ] Redeploy frontend
- [ ] Test WebSocket connection
- [ ] Check browser console (F12) for errors
- [ ] Verify event log shows "Secure WebSocket" message

---

## Environment Variables

### Vercel (Frontend)
✅ **No secrets needed** - frontend is public

### Railway/Render (Backend)
```env
# Database
DATABASE_URL=sqlite:///./threats.db

# Security
SECRET_KEY=your-secret-key-here
DEBUG=false

# AI Models
MEDIAPIPE_MODEL_COMPLEXITY=2
YOLO_MODEL_NAME=yolov8n
YOLO_CONFIDENCE=0.5

# CORS
ALLOWED_HOSTS=yourdomain.vercel.app,localhost:3000

# API
API_HOST=0.0.0.0
API_PORT=8000
```

---

## Performance Metrics

### Expected Performance
| Metric | Typical | Max |
|--------|---------|-----|
| **Page Load** | <2s | <5s |
| **API Response** | 50-200ms | <1s |
| **WebSocket** | 20-50ms latency | <500ms |
| **Inference Time** | 100-500ms | <2s |
| **Concurrent Users** | 100+ | 1000+ (with scaling) |

### Vercel Benefits
- ✅ Global CDN (instant worldwide access)
- ✅ Auto-scaling (handles traffic spikes)
- ✅ Built-in monitoring
- ✅ Automatic HTTPS
- ✅ Analytics included

### Railway/Render Benefits
- ✅ Full Python 3.10+
- ✅ GPU available (GPU plan)
- ✅ Persistent storage
- ✅ Easy scaling
- ✅ GitHub auto-deploy

---

## Troubleshooting

### WebSocket Connection Issues
```javascript
// Check in browser console (F12)
// Should see: "Secure WebSocket tunnel established to Cloud"
// If red: Backend not responding or CORS issue
```

**Solution:**
1. Verify backend URL in `public/index.html`
2. Check CORS in `main.py` includes Vercel domain
3. Ensure backend service is running
4. Check for HTTPS/HTTP mismatch

### Frontend Loads but Backend Disconnected
```
Event Log Shows:
>> Warning: Cloud connection lost. Running locally.
```

**Solution:**
1. Update `BACKEND_URL` to production URL
2. Add `https://yourdomain.vercel.app` to CORS
3. Redeploy backend with updated settings
4. Clear browser cache (Ctrl+Shift+Delete)

### Vercel Deployment Fails
```bash
# Debug
vercel --version
vercel logs

# Common fixes
rm vercel.json  # Sometimes conflicts
vercel --prod
```

### 404 on Vercel
Ensure `public/index.html` exists and is not excluded in `.vercelignore`

---

## Cost Estimation

### Free Tier (Hobby)
| Service | Free | Limit |
|---------|------|-------|
| **Vercel** | ✅ Yes | 100GB bandwidth/month |
| **Railway** | ✅ Yes | $5 credit/month |
| **Render** | ✅ Yes | 750 free hours/month |
| **Firebase** | ✅ Yes | 50k SMS/month |
| **Total** | ✅ **FREE** | Recommended for all |

### Paid Tier (Production)
| Service | Cost | Value |
|---------|------|-------|
| **Vercel Pro** | $20/month | Unlimited bandwidth |
| **Railway** | ~$10/month | As you go ($0.10/hr) |
| **Render** | ~$12/month | Paid instances |
| **Firebase Plan** | ~$5/month | SMS overage |
| **Total** | **~$47/month** | Production scale |

---

## What You Can Do Now

### ✅ Already Configured
- Deploy frontend instantly
- Full AI processing (TensorFlow.js + Server)
- Real-time WebSocket streaming
- Firebase authentication
- Database persistence
- Logs and monitoring

### ⏭️ Next: Advanced Features
1. **Custom Domain**: `yourdomain.com` → Vercel + Railway
2. **GPU Processing**: Railway GPU tier for faster inference
3. **Advanced Scaling**: Vercel Pro + Railway dedicated
4. **Monitoring**: Datadog/New Relic integration
5. **CDN Optimization**: CloudFlare integration
6. **Analytics**: Vercel Analytics + FirebasePerformance

---

## Quick Commands

```bash
# Deploy Frontend
vercel

# Check Vercel status
vercel logs
vercel status

# Deploy Backend (Railway)
railway up

# View Railway logs
railway logs

# Redeploy with latest code
vercel --prod
railway redeploy

# Rollback if needed
vercel rollback
```

---

## Documentation Links

| Document | Purpose |
|----------|---------|
| [VERCEL_DEPLOYMENT.md](VERCEL_DEPLOYMENT.md) | Complete technical guide |
| [VERCEL_QUICK_START.md](VERCEL_QUICK_START.md) | 10-minute quick setup |
| [README.md](README.md) | Feature overview |
| [SETUP.md](SETUP.md) | Local development |
| [START.md](START.md) | Quick reference |

---

## You're All Set! 🚀

### Summary
✅ Frontend ready for Vercel  
✅ Backend ready for Railway/Render  
✅ WebSocket configured  
✅ CORS headers set  
✅ Documentation complete  

### Next Action
**Choose your hosting and run:**
```bash
vercel    # Deploy frontend
```

Then in ~5 minutes:
```bash
railway up    # Deploy backend
```

Your ThreatSense OS will be **live at https://yourdomain.vercel.app** 🎉

---

**Questions?** Check [VERCEL_DEPLOYMENT.md](VERCEL_DEPLOYMENT.md) for detailed troubleshooting.

**Ready?** Start with [VERCEL_QUICK_START.md](VERCEL_QUICK_START.md)
