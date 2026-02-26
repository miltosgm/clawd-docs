# 🚀 Quick Start - Deploy Mission Control to Vercel

## 30-Second Summary

✅ Dashboard built  
✅ Code committed to Git  
✅ Ready to deploy  

**Next: Push to GitHub → Deploy to Vercel**

---

## 📋 Step 1: Create GitHub Repository

Go to https://github.com/new and create a repository named `mission-control`

Then run:

```bash
cd /Users/milton/mission-control
git remote add origin https://github.com/YOUR_USERNAME/mission-control.git
git branch -M main
git push -u origin main
```

Replace `YOUR_USERNAME` with your actual GitHub username.

---

## 🚀 Step 2: Deploy to Vercel

1. Go to **https://vercel.com/new**
2. Click **"Import Git Repository"**
3. Paste your GitHub repo URL
4. Click **"Import"**
5. Vercel auto-detects Next.js → Click **"Deploy"**

**That's it!** Your dashboard is live in ~2 minutes.

---

## ✅ Step 3: Get Your Live URL

After deployment, Vercel gives you:
```
https://mission-control-XXXXXX.vercel.app
```

This is your public dashboard URL.

---

## 🔌 Step 4: Setup OpenClaw Webhook

### 4a. Copy webhook library
```bash
cp /Users/milton/mission-control/lib/openclaw-webhook.ts /Users/milton/clawd/lib/
```

### 4b. Set environment variable
Add to your OpenClaw `.env`:
```
MISSION_CONTROL_URL=https://mission-control-XXXXXX.vercel.app
```

### 4c. Add logging to your agent
In your agent code:
```typescript
import { logActivity } from '@/lib/openclaw-webhook'

// Log file creation
await logActivity({
  type: 'file_created',
  description: 'Created portfolio-plan.md',
  result: '12.6 KB file'
})

// Log task completion
await logActivity({
  type: 'task_completed',
  description: 'Built dashboard',
  result: 'Deployed to Vercel'
})
```

### 4d. Test it
1. Trigger an action in your agent (create file, complete task, etc.)
2. Visit your live dashboard
3. Activity should appear in real-time! 🎉

---

## 📊 What You Now Have

### Dashboard Features
✅ **Activity Feed** - Every action logged with timestamp  
✅ **Calendar View** - Weekly scheduled tasks  
✅ **Global Search** - Search memory, documents, tasks  
✅ **Real-time Sync** - Activities appear instantly  
✅ **Mobile Responsive** - Works on all devices  

### Tech Stack
- Next.js 14+ (framework)
- React (UI components)
- Tailwind CSS (styling)
- Vercel (hosting)
- OpenClaw Agent (activity source)

### Files
- **3 components** (ActivityFeed, CalendarView, GlobalSearch)
- **1 API endpoint** (/api/log-activity) for webhook
- **1 library** (openclaw-webhook.ts) for logging
- **Beautiful dark theme** with color-coded badges
- **Full documentation** (3 guides included)

---

## 🧪 Test It Locally First

Before deploying to Vercel, test locally:

```bash
cd /Users/milton/mission-control
npm run dev
```

Visit **http://localhost:3000**

You should see:
- Dark dashboard with sidebar
- Global search bar
- Calendar view
- Activity feed
- Mock sample data

Try the search - it works! Try the filters - they work!

---

## 🎯 Next: Integrate with OpenClaw

See **OPENCLAW_INTEGRATION.md** for:
- How to import the webhook library
- Where to add logging calls
- Real-world integration examples
- Testing instructions

---

## ❓ Common Questions

**Q: Do I need to pay?**
A: No. Vercel free tier works great for this dashboard.

**Q: Will activities persist?**
A: For now, only while the app runs (in-memory).
Later, add Convex backend for permanent storage (still free).

**Q: Can I share the dashboard?**
A: Yes! The Vercel URL is public.
Anyone can visit your live dashboard.

**Q: What if I deploy again?**
A: Activities in memory will reset.
Add Convex backend (see DEPLOYMENT.md) to fix this.

**Q: How do I update the dashboard?**
A: Push changes to GitHub:
```bash
git add .
git commit -m "Your message"
git push origin main
```
Vercel auto-deploys. Updates live in ~1 minute!

---

## 🚨 Troubleshooting

**Dashboard not loading after deploy?**
→ Check Vercel deploy logs (click on deployment in Vercel dashboard)

**Activities not appearing?**
→ Make sure `MISSION_CONTROL_URL` env var is set correctly

**Search not working?**
→ Currently uses mock data. Will show real data once integrated.

**404 on /api/log-activity?**
→ Vercel may need restart. Redeploy from Vercel dashboard.

---

## 📚 Full Documentation

- **DEPLOYMENT.md** - Detailed deployment guide
- **OPENCLAW_INTEGRATION.md** - How to integrate with agent
- **BACKEND_INTEGRATION_EXPLAINED.md** - Backend concerns explained
- **README.md** - Project overview

---

## ✨ You're Done!

**In 30 minutes you went from:**
- 📋 Idea
- 🛠️ Built 3 features
- 📦 Packaged for production
- 🚀 Ready to deploy

**Next:** Push to GitHub and deploy to Vercel (5 minutes)

Then watch activities flow in real-time! 🎮

---

**Ready?** Follow Steps 1-2 above and you're live! 🎉
