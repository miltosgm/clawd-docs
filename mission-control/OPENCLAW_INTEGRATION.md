# OpenClaw Agent Integration Guide

This guide shows how to integrate Mission Control dashboard with your OpenClaw agent to automatically log every action.

## Setup

### 1. Copy the webhook library to your agent

Copy `/mission-control/lib/openclaw-webhook.ts` to your agent directory:

```bash
cp /Users/milton/mission-control/lib/openclaw-webhook.ts /Users/milton/clawd/lib/openclaw-webhook.ts
```

### 2. Set environment variable

In your agent's `.env` file or OpenClaw config:

```
MISSION_CONTROL_URL=http://localhost:3000
# Or when deployed:
# MISSION_CONTROL_URL=https://mission-control-XXXXXX.vercel.app
```

---

## Integration Examples

### File Operations

```typescript
import { logFileCreated, logFileEdited } from '@/lib/openclaw-webhook'

// When creating a file
const fs = require('fs').promises
async function createProposal(path: string, content: string) {
  await fs.writeFile(path, content)
  await logFileCreated(path, content.length)
}

// When editing a file
async function updateMemory(path: string, newContent: string) {
  await fs.writeFile(path, newContent)
  await logFileEdited(path, 'Updated weekly summary and project notes')
}
```

### Searches

```typescript
import { logSearch } from '@/lib/openclaw-webhook'

async function searchWorkspace(query: string) {
  const results = await memorySearch(query)
  await logSearch(
    query,
    results.length,
    ['memory', 'documents', 'tasks']
  )
  return results
}
```

### Task Completion

```typescript
import { logTaskCompleted } from '@/lib/openclaw-webhook'

async function executeTask(taskName: string) {
  const startTime = Date.now()
  try {
    // ... do work ...
    await logTaskCompleted(
      taskName,
      Date.now() - startTime,
      'success'
    )
  } catch (error) {
    await logTaskCompleted(
      taskName,
      Date.now() - startTime,
      'failed'
    )
  }
}
```

### Major Deliverables (Proposals, Plans, etc.)

```typescript
import { logProposal } from '@/lib/openclaw-webhook'

async function buildProposal(name: string, content: string) {
  // Generate proposal...
  const pageCount = content.split('\n---\n').length
  
  await logProposal(
    name,
    pageCount,
    'Complete growth strategy with ad campaigns and projections'
  )
}
```

### Custom Events

```typescript
import { logCustom, logActivity } from '@/lib/openclaw-webhook'

// Simple helper
await logCustom(
  'Gateway restarted',
  'Success: Gateway authenticated and ready'
)

// Full control
await logActivity({
  type: 'other',
  description: 'Deployed to Vercel',
  result: 'Live at mission-control-xyz.vercel.app',
  metadata: {
    deployment_url: 'https://vercel.com/...',
    build_time: 45000
  }
})
```

---

## Ideal Integration Points

### In sessions_spawn / subagent spawning
```typescript
import { logCustom } from '@/lib/openclaw-webhook'

export async function spawnSubagent(task: string) {
  await logCustom(
    `Spawned subagent: ${task}`,
    'Running in background session'
  )
  // ... spawn logic ...
}
```

### In memory operations
```typescript
import { logFileEdited } from '@/lib/openclaw-webhook'

// When updating MEMORY.md
await logFileEdited(
  'MEMORY.md',
  'Saved weekly summary and key decisions'
)

// When creating daily log
await logFileCreated(
  'memory/2026-02-14.md',
  dailyLog.length
)
```

### In web operations (searches, fetches)
```typescript
import { logSearch } from '@/lib/openclaw-webhook'

// When doing web_search
const results = await webSearch(query)
await logSearch(
  query,
  results.length,
  ['web search']
)

// When fetching web content
const content = await webFetch(url)
await logCustom(
  `Fetched content from ${url}`,
  `Retrieved ${content.length} characters`
)
```

### In long-running tasks
```typescript
import { logTaskCompleted } from '@/lib/openclaw-webhook'

async function buildPortfolioWebsite() {
  const startTime = Date.now()
  
  try {
    // Build steps...
    await logTaskCompleted(
      'Build portfolio website',
      Date.now() - startTime
    )
  } catch (error) {
    await logTaskCompleted(
      'Build portfolio website',
      Date.now() - startTime,
      'failed'
    )
  }
}
```

---

## Real-World Example: Complete Flow

```typescript
import { 
  logFileCreated, 
  logProposal, 
  logSearch, 
  logTaskCompleted 
} from '@/lib/openclaw-webhook'

async function buildRenouveatuProposal() {
  const startTime = Date.now()
  const taskName = 'Build Renouveau Capital Proposal'
  
  try {
    // Log search
    const searchResults = await memory_search('Renouveau metrics')
    await logSearch('Renouveau metrics', searchResults.length, ['memory'])
    
    // Build proposal
    const proposal = await generateProposal({
      client: 'Renouveau Capital',
      target_aum: '€10M',
      metrics: searchResults
    })
    
    // Log file creation
    const filePath = '/Users/milton/clawd/renouveau-proposal.md'
    await fs.writeFile(filePath, proposal)
    await logFileCreated(filePath, proposal.length)
    
    // Log completion
    await logProposal(
      'Renouveau Capital Proposal',
      45,
      'Complete growth strategy with 3 ad campaigns and 12-month projections'
    )
    
    // Log task completion
    await logTaskCompleted(
      taskName,
      Date.now() - startTime,
      'success'
    )
  } catch (error) {
    await logTaskCompleted(
      taskName,
      Date.now() - startTime,
      'failed'
    )
  }
}
```

---

## Testing the Integration

### 1. Start dashboard locally
```bash
cd /Users/milton/mission-control
npm run dev
# Runs on http://localhost:3000
```

### 2. Test webhook manually
```bash
curl -X POST http://localhost:3000/api/log-activity \
  -H "Content-Type: application/json" \
  -d '{
    "type": "file_created",
    "description": "Test activity",
    "result": "Successfully logged"
  }'
```

### 3. View dashboard
Open http://localhost:3000 and you should see the activity in the feed!

### 4. Check API
```bash
curl http://localhost:3000/api/log-activity?limit=10
```

Returns:
```json
{
  "activities": [...],
  "total": 1,
  "filtered": 1,
  "hasMore": false
}
```

---

## Troubleshooting

**Activities not appearing:**
1. Check dashboard is running on correct port
2. Verify `MISSION_CONTROL_URL` environment variable
3. Check browser console for errors
4. Verify webhook endpoint: `curl http://localhost:3000/api/log-activity`

**"Connection error" in logs:**
→ Dashboard is offline. Make sure it's running.

**Activities disappear after refresh:**
→ Using in-memory storage. Add Convex backend for persistence (see DEPLOYMENT.md).

---

## Next Steps

1. ✅ Copy `openclaw-webhook.ts` to your agent
2. ✅ Set `MISSION_CONTROL_URL` environment variable
3. ✅ Add import statements to your agent code
4. ✅ Call logging functions at key points
5. ✅ Test locally with `npm run dev`
6. ✅ Deploy dashboard to Vercel
7. ✅ Update environment variable to production URL
8. ✅ Watch activities appear in real-time!

---

## Activity Types Reference

```typescript
'file_created'   // When creating new files
'file_edited'    // When modifying existing files
'search'         // When searching memory/documents
'task_completed' // When completing tasks/work
'proposal'       // When generating major deliverables
'other'          // Anything else
```

Use `task_created` for:
- Spawning subagents
- Starting long-running tasks
- Creating new projects

Use `task_completed` for:
- Finishing those tasks
- Successful execution
- Failed attempts

---

**Result:** Every action is now visible in your Mission Control dashboard in real-time! 🎮
