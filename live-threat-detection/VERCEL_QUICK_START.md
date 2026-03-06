# Vercel Quick Start - Deploy in 10 Minutes

## 🚀 Super Quick (Copy-Paste)

### 1️⃣ Install Vercel CLI
```powershell
npm install -g vercel
```

### 2️⃣ Deploy Frontend
```powershell
cd c:\Users\yashs\OneDrive\Documents\GitHub\live-threat-detection
vercel
```

**When prompted:**
- Project name: `live-threat-detection`
- Link to existing project? `n`
- Set as production? `y`

**Result:** Your frontend goes live! 🎉
```
✅ Production: https://live-threat-detection.vercel.app
```

---

## 3️⃣ Deploy Backend (Choose One)

### Railway (Easiest - 5 mins)
**Commands:**
```powershell
npm install -g @railway/cli
railway login
railway init
railway up
```

**Result:** Backend at `https://your-project.up.railway.app`

### Render.com (Free Alternative)
1. Go to [render.com](https://render.com)
2. New Web Service
3. Point to your GitHub repo
4. Start command: `uvicorn main:app --host 0.0.0.0 --port 8000`
5. Deploy

---

## 4️⃣ Connect Them

### Edit Frontend to Point to Backend

Find this in `public/index.html` (line 230):
```javascript
const BACKEND_URL = 'http://localhost:8000';
```

Change to your backend URL:
```javascript
const BACKEND_URL = 'https://your-project.up.railway.app';
```

### Redeploy Frontend
```powershell
vercel --prod
```

---

## ✅ Test It

Open: `https://live-threat-detection.vercel.app`

You should see:
1. **ThreatSense OS login screen** ✓
2. **Firebase OTP authentication** ✓
3. **After login → mode selection** ✓
4. **Event log shows: "Secure WebSocket tunnel established"** ✓

---

## 🎯 That's It!

| Component | Platform | Status |
|-----------|----------|--------|
| Frontend (HTML/CSS/JS) | Vercel | ✅ Live |
| Backend (Python/AI) | Railway/Render | ✅ Live |
| Database | Local SQLite or Cloud | ✅ Running |
| WebSocket | Bidirectional | ✅ Connected |

---

## 📊 Monitoring

**Vercel Logs:**
```
https://vercel.com/dashboard → Select project → Deployments → View logs
```

**Railway Logs:**
```
railway logs
```

---

## 💰 Estimated Costs

| Service | Free Tier | Cost |
|---------|-----------|------|
| Vercel | Yes (hobby) | Free |
| Railway | $5 credit/month | $0 if under credit |
| Render | Limited free | Free |
| **Total** | | **Free - $5/month** |

---

## 🔧 If Something Goes Wrong

**Frontend shows but backend disconnected?**
```javascript
// Check DevTools → Console for errors
// Open DevTools (F12) → Console tab
// Look for WebSocket error messages
```

**Connection success message should say:**
```
>> Secure WebSocket tunnel established to Cloud. [GREEN]
```

If red text instead, check:
1. Backend URL in `public/index.html`
2. Backend service is running
3. CORS is configured

**Quick fix - redeploy:**
```powershell
vercel --prod
```

---

## Next Steps

1. ✅ Frontend deployed to Vercel
2. ✅ Backend deployed to Railway/Render
3. ✅ Connected via WebSocket
4. ⬜ Connect real camera feeds (RTSP)
5. ⬜ Set up monitoring
6. ⬜ Custom domain (optional)

---

## Support

- **Vercel Docs:** https://vercel.com/docs
- **Railway Docs:** https://docs.railway.app
- **FastAPI Deployment:** https://fastapi.tiangolo.com/deployment/

Your system is now production-ready! 🎊
