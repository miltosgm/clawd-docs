# Vercel Deployment - Mission Control Dashboard

## ✅ Code Ready for Deployment

Your GitHub repo is set up and pushed:
- **Repository:** https://github.com/miltosgm/mission-control
- **Branch:** main
- **Status:** Ready to deploy

---

## 🚀 Deploy to Vercel (3 clicks)

### Step 1: Go to Vercel New Project
Open: https://vercel.com/new

### Step 2: Import GitHub Repository
1. Click "Import Git Repository"
2. Search for "mission-control"
3. Select `miltosgm/mission-control`
4. Click "Import"

### Step 3: Deploy
1. Framework: **Next.js** (auto-detected)
2. Click "Deploy"
3. **Wait 2-3 minutes** for build

### Your Live Dashboard
After deployment, you'll get a URL like:
```
https://mission-control-XXXXXX.vercel.app
```

---

## 🔌 After Deployment: Setup Environment Variable

### Step 1: Get Your Vercel URL
From the Vercel dashboard, copy the URL (e.g., `https://mission-control-abc123.vercel.app`)

### Step 2: Add to Vercel Environment Variables
1. Go to **Project Settings** → **Environment Variables**
2. Add new variable:
   - **Name:** `MISSION_CONTROL_URL`
   - **Value:** `https://mission-control-abc123.vercel.app`
   - **Environment:** Production
3. Click "Save"

### Step 3: Redeploy
1. Go to **Deployments**
2. Click the three dots on the latest deployment
3. Select "Redeploy"
4. Click "Redeploy" again

### Step 4: Update Your Agent
Once you have the live URL, update your OpenClaw agent:

In `/Users/milton/clawd/.env.local`:
```
MISSION_CONTROL_URL=https://mission-control-abc123.vercel.app
```

---

## ✅ Test the Integration

### Local Testing (Before Going Live)

1. **Start the dashboard**
   ```bash
   cd /Users/milton/mission-control
   npm run dev
   ```
   Opens on http://localhost:3000

2. **Test the webhook**
   ```bash
   curl -X POST http://localhost:3000/api/log-activity \
     -H "Content-Type: application/json" \
     -d '{
       "type": "file_created",
       "description": "Test activity from webhook",
       "result": "Successfully logged!"
     }'
   ```

3. **Check the dashboard**
   - Open http://localhost:3000
   - You should see the test activity in the feed!

4. **Check the API**
   ```bash
   curl http://localhost:3000/api/log-activity?limit=5
   ```
   Returns JSON with recent activities

---

## 📊 Integration Points in Your Agent

The webhook library is already in `/Users/milton/clawd/lib/openclaw-webhook.ts`

Use it like this:

```typescript
import { 
  logFileCreated,
  logFileEdited, 
  logSearch,
  logTaskCompleted,
  logProposal,
  logCustom
} from '@/lib/openclaw-webhook'

// When you create files
await logFileCreated('path/to/file.md', 12600)

// When you update files
await logFileEdited('MEMORY.md', 'Saved weekly summary')

// When you search
await logSearch('agent automation', 42, ['memory', 'docs'])

// When you complete tasks
await logTaskCompleted('Build proposal', 15000)

// When you generate proposals
await logProposal('Client Proposal', 25, 'Complete strategy with 3 campaigns')

// For anything else
await logCustom('Gateway restarted', 'Status: Ready')
```

---

## 🎯 What You Get

Once everything is deployed and connected:

✅ **Real-time activity feed** - See every action as it happens
✅ **Calendar view** - Track when work happened
✅ **Global search** - Find activities by keyword
✅ **Activity types** - Color-coded categories
✅ **Metadata** - Extra details for debugging
✅ **No maintenance** - Activities auto-logged from your agent

---

## 🔧 Troubleshooting

### "Connection error" in logs
**Problem:** Dashboard is offline or wrong URL
**Solution:** 
- Check `MISSION_CONTROL_URL` env var
- Make sure dashboard is running
- Verify correct Vercel URL

### Activities don't appear
**Problem:** Webhook not being called
**Solution:**
- Add logging calls to your agent code
- Test with curl command above
- Check dashboard is running

### Activities disappear after refresh
**Problem:** Using in-memory storage
**Solution:** Add Convex backend (see DEPLOYMENT.md)

### Build fails on Vercel
**Problem:** Missing dependencies
**Solution:** Rebuild on Vercel (go to Deployments → Redeploy)

---

## 📈 Next: Add Convex Backend (Optional)

Want activities to persist forever? Add Convex:

```bash
npm install convex
npx convex dev
```

Then update `/app/api/log-activity/route.ts` to use Convex instead of in-memory storage.

See `BACKEND_INTEGRATION_EXPLAINED.md` for details.

---

## ✅ Deployment Checklist

- [ ] Repo pushed to GitHub (https://github.com/miltosgm/mission-control)
- [ ] Visit https://vercel.com/new
- [ ] Import mission-control repo
- [ ] Click Deploy
- [ ] Wait 2-3 minutes for build
- [ ] Copy the live URL
- [ ] Add MISSION_CONTROL_URL to Vercel env vars
- [ ] Redeploy from Vercel dashboard
- [ ] Update `/Users/milton/clawd/.env.local` with live URL
- [ ] Test webhook locally first
- [ ] Test with live URL
- [ ] Watch activities flow in real-time! 🎉

---

## 📞 You're All Set!

Your dashboard is production-ready. The webhook library is in your agent. All that's left is:

1. Deploy to Vercel (5 minutes)
2. Add environment variable
3. Start logging activities
4. Watch your work appear in real-time!

Questions? Check OPENCLAW_INTEGRATION.md for detailed examples.
