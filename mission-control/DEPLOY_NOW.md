# 🚀 ONE-CLICK DEPLOY TO VERCEL

## Easiest Way (One Click)

**Click this link:** https://vercel.com/new?repo-url=https://github.com/miltosgm/mission-control&project-name=mission-control&repository-name=mission-control

This will:
1. ✅ Authenticate with your GitHub account (miltosgm)
2. ✅ Import the mission-control repository
3. ✅ Auto-detect Next.js framework
4. ✅ Deploy automatically
5. ✅ Give you a live URL in 2-3 minutes

---

## What Happens After You Click

1. **GitHub Sign-In** (if needed)
   - Sign in with miltosgm / Miltos899__
   
2. **Import Page**
   - Repository will be pre-filled
   - Click "Import"

3. **Configure Project**
   - Framework: Next.js (auto-selected)
   - Root Directory: ./ (default)
   - No env vars needed yet
   - Click "Deploy"

4. **Wait for Build** (2-3 minutes)
   - You'll see build logs
   - Status will change from "Building" → "Ready"

5. **Get Your Live URL**
   - Copy URL like: `https://mission-control-abc123.vercel.app`
   - This is your live dashboard!

---

## After Deployment (2 more minutes)

### Step 1: Add Environment Variable

1. Go to Vercel Project Settings
2. Navigate to **Environment Variables**
3. Add this variable:
   - **Name:** `MISSION_CONTROL_URL`
   - **Value:** `https://mission-control-abc123.vercel.app` (your URL from above)
   - **Environment:** Production
4. Click "Save"

### Step 2: Redeploy

1. Back in Deployments tab
2. Find your latest deployment
3. Click the three dots (...)
4. Select "Redeploy"
5. Confirm "Redeploy"

### Step 3: Test It

1. Open your live dashboard URL
2. Activities should start appearing
3. Done! 🎉

---

## Update Your Agent Config

Once you have the live URL:

```bash
# Edit /Users/milton/clawd/.env.local
MISSION_CONTROL_URL=https://mission-control-abc123.vercel.app
```

Then activities from your agent will log to the live dashboard in real-time.

---

## Quick Reference

| Step | Time | Status |
|------|------|--------|
| Click deploy link | 10 sec | 🟢 |
| GitHub auth | 30 sec | 🟢 |
| Project import | 10 sec | 🟢 |
| Initial deploy | 2 min | 🔵 |
| Add env var | 30 sec | 🟢 |
| Redeploy | 1 min | 🟢 |
| **Total** | **~5 min** | **✅** |

---

## Troubleshooting

**"Repository not found"**
→ Make sure you're signed in as `miltosgm` on GitHub

**"Deploy failed"**
→ Check build logs in Vercel. Most common: missing dependencies (not in our case)

**"Activities not appearing"**
→ Make sure you added the env var and redeployed

---

## Questions?

The dashboard is production-ready. The webhook library is in your agent. All that's left is clicking one link and waiting 5 minutes.

**That's it!**
