# Deployment Guide - Mission Control Dashboard

## 🚀 Deploy to Vercel (5 minutes)

### Step 1: Create GitHub Repository
```bash
cd /Users/milton/mission-control
git remote add origin https://github.com/YOUR_USERNAME/mission-control.git
git branch -M main
git push -u origin main
```

### Step 2: Deploy to Vercel
1. Go to https://vercel.com/new
2. Import your GitHub repository
3. Select "Next.js" as framework
4. Click "Deploy"

**That's it!** Vercel auto-detects Next.js and deploys.

### Step 3: Get Your Live URL
Your dashboard will be live at:
```
https://mission-control-XXXXXX.vercel.app
```

---

## 🔌 Setup OpenClaw Webhook Integration

The webhook system logs **every action** to your dashboard automatically.

### Step 1: Create Activity Logging Endpoint

Create file: `/Users/milton/mission-control/app/api/log-activity/route.ts`

```typescript
import { NextRequest, NextResponse } from 'next/server'

interface ActivityLog {
  timestamp: number
  type: 'file_created' | 'file_edited' | 'search' | 'task_completed' | 'proposal' | 'other'
  description: string
  result: string
  metadata?: Record<string, any>
}

// In-memory storage (replace with Convex for persistence)
const activities: ActivityLog[] = []

export async function POST(request: NextRequest) {
  try {
    const body: ActivityLog = await request.json()
    
    // Add timestamp if not provided
    if (!body.timestamp) {
      body.timestamp = Date.now()
    }

    // Log the activity
    activities.push(body)
    
    // Keep only last 1000 activities in memory
    if (activities.length > 1000) {
      activities.shift()
    }

    console.log('✅ Activity logged:', body.description)

    return NextResponse.json({
      ok: true,
      activity: body,
      totalActivities: activities.length
    })
  } catch (error) {
    console.error('❌ Error logging activity:', error)
    return NextResponse.json(
      { error: 'Failed to log activity' },
      { status: 500 }
    )
  }
}

export async function GET(request: NextRequest) {
  const limit = Number(request.nextUrl.searchParams.get('limit') || 50)
  const offset = Number(request.nextUrl.searchParams.get('offset') || 0)
  
  return NextResponse.json({
    activities: activities.slice(-limit - offset, -offset || undefined),
    total: activities.length
  })
}
```

### Step 2: Create OpenClaw Integration Hook

Create file: `/Users/milton/mission-control/lib/openclaw-webhook.ts`

```typescript
/**
 * OpenClaw Webhook Integration
 * Call this function whenever an action completes
 */

const DASHBOARD_URL = process.env.MISSION_CONTROL_URL || 'http://localhost:3000'

export interface ActivityEvent {
  type: 'file_created' | 'file_edited' | 'search' | 'task_completed' | 'proposal' | 'other'
  description: string
  result: string
  metadata?: Record<string, any>
}

export async function logActivity(event: ActivityEvent) {
  try {
    const response = await fetch(`${DASHBOARD_URL}/api/log-activity`, {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify({
        timestamp: Date.now(),
        ...event
      })
    })

    if (!response.ok) {
      console.error('Failed to log activity:', await response.text())
    }
  } catch (error) {
    console.error('Error logging activity to dashboard:', error)
  }
}

// Example usage in OpenClaw agent:
// logActivity({
//   type: 'file_created',
//   description: 'Created portfolio-plan.md',
//   result: '12.6 KB file generated successfully'
// })
```

### Step 3: Hook into OpenClaw Agent

In your OpenClaw agent code (e.g., `/Users/milton/clawd/index.ts` or wherever your agent runs):

```typescript
import { logActivity } from './lib/openclaw-webhook'

// When creating a file:
async function createFile(path: string, content: string) {
  const result = await fs.writeFile(path, content)
  await logActivity({
    type: 'file_created',
    description: `Created ${path}`,
    result: `${content.length} bytes written`
  })
}

// When completing a task:
async function completeTask(taskName: string, duration: number) {
  await logActivity({
    type: 'task_completed',
    description: taskName,
    result: `Completed in ${duration}ms`
  })
}

// When performing a search:
async function searchWorkspace(query: string, results: number) {
  await logActivity({
    type: 'search',
    description: `Searched for "${query}"`,
    result: `Found ${results} results`
  })
}
```

### Step 4: Setup Environment Variables

For Vercel, add to `.env.local`:
```
MISSION_CONTROL_URL=https://mission-control-XXXXXX.vercel.app
```

For development:
```
MISSION_CONTROL_URL=http://localhost:3000
```

---

## 📊 Activity Types

Supported activity types to log:

| Type | When to Use | Example |
|------|-----------|---------|
| `file_created` | New file written | "Created portfolio-plan.md" |
| `file_edited` | Existing file modified | "Updated MEMORY.md" |
| `search` | Search performed | "Searched for 'portfolio'" |
| `task_completed` | Task finished | "Completed gateway restart" |
| `proposal` | Major deliverable | "Built Renouveau proposal" |
| `other` | Anything else | "Updated configuration" |

---

## 🔐 Security Considerations

### Current Setup (Development)
- Activities stored in-memory (lost on restart)
- No authentication
- Local URL only

### For Production (What Convex Solves)
✅ **Persistent database** - Activities saved forever
✅ **Authentication** - Only authorized requests logged
✅ **Real-time sync** - Multiple clients see updates instantly
✅ **Scalability** - Handles thousands of activities
✅ **Backup & recovery** - Automatic backups

---

## 🚨 What Backend Integration Concerns Mean

When I mentioned "concerns about backend integration," I meant:

### Current Setup (No Backend)
❌ Activities lost when app restarts
❌ No persistence between sessions
❌ Can't share dashboard across devices
❌ No database for historical queries
❌ Limited to ~1000 in-memory activities

### With Convex Backend (Recommended)
✅ Activities saved to database forever
✅ Query historical data (last week, last month, etc.)
✅ Share dashboard across devices/browsers
✅ Real-time updates (see activities as they happen)
✅ Scale to unlimited activities
✅ Authentication & authorization
✅ Webhooks & integrations

---

## 📈 When to Add Convex

**Start without it (now):** MVP dashboard, local testing
**Add it (next):** When you want persistence or to scale

To add Convex later:
```bash
npm install convex
npx convex dev
# Follow setup wizard
```

Then update `/app/api/log-activity/route.ts` to use Convex instead of in-memory array.

---

## ✅ Deployment Checklist

- [ ] Push code to GitHub
- [ ] Deploy to Vercel
- [ ] Get live URL
- [ ] Set `MISSION_CONTROL_URL` environment variable
- [ ] Test dashboard at live URL
- [ ] Create activity logging endpoint
- [ ] Integrate webhook into OpenClaw agent
- [ ] Verify activities appear in real-time
- [ ] (Optional) Add Convex backend for persistence

---

## 🆘 Troubleshooting

**Dashboard shows "No results"**
→ Activities endpoint not being called. Check webhook integration.

**Activities appear but disappear after refresh**
→ Using in-memory storage. Add Convex to persist data.

**Webhook returning 404**
→ `MISSION_CONTROL_URL` environment variable not set correctly.

**Slow search**
→ Too many activities in memory. Implement Convex pagination.

---

## 📞 Next Steps

1. **Deploy to Vercel** - Get live URL
2. **Setup webhook** - Auto-log activities
3. **Add Convex** (optional) - Persistent database
4. **Monitor** - Watch activities appear in real-time

Questions? Let me know!
