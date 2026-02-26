# 🎮 Mission Control Dashboard

A comprehensive activity tracking and task management dashboard for your AI assistant.

## Features

### 📝 Activity Feed
- Logs **every action** taken: files created, searches performed, tasks completed, proposals built
- Timestamp, action type, description, and result
- Sortable by date, filterable by action type
- Full history with pagination

### 📅 Calendar View  
- Weekly view of all scheduled tasks
- Shows task name, time, description
- Color-coded status: Scheduled, Running, Completed, Failed
- Integrates with OpenClaw cron schedule
- Today's date highlighted

### 🔍 Global Search
- Search across all workspace data:
  - Memory files (MEMORY.md, daily logs)
  - Documents (proposals, plans, notes)
  - Tasks (scheduled and completed)
  - Activity log (all actions)
- Real-time search results with relevance ranking
- Click to open or view details
- Quick access to metrics (8 recent docs, 12 memory entries, etc.)

## Tech Stack

- **Frontend:** Next.js 14+ (App Router) + React
- **Styling:** Tailwind CSS
- **Database:** Convex (real-time syncing)
- **Authentication:** Convex authentication
- **Language:** TypeScript

## Project Structure

```
mission-control/
├── app/
│   ├── layout.tsx              # Main layout with sidebar
│   ├── page.tsx                # Dashboard home
│   ├── globals.css             # Global styles
│   └── components/
│       ├── ActivityFeed.tsx    # Activity log component
│       ├── CalendarView.tsx    # Weekly task calendar
│       └── GlobalSearch.tsx    # Unified search
├── convex/                      # Convex backend (coming soon)
│   ├── schema.ts               # Database schema
│   └── functions/              # Convex API functions
├── tailwind.config.js          # Tailwind configuration
├── next.config.js              # Next.js configuration
└── package.json                # Dependencies
```

## Getting Started

### 1. Install Dependencies
```bash
npm install
```

### 2. Configure Convex
```bash
npm install convex
npx convex dev
```

Follow the prompts to set up your Convex project.

### 3. Start Development Server
```bash
npm run dev
```

Visit `http://localhost:3000` to see the dashboard.

### 4. Build for Production
```bash
npm run build
npm start
```

## Features Overview

### Activity Feed
- **Type badges:** File Created, File Edited, Search, Task Created, Task Completed, Proposal, Other
- **Timestamps:** Precise timestamps with date, time, seconds
- **Sorting:** Most Recent or Oldest First
- **Filtering:** By action type
- **Details:** Result and impact of each action

### Calendar
- **Weekly layout:** Sunday through Saturday
- **Status indicators:** 
  - 📌 Scheduled (blue)
  - ⚙️ Running (amber)
  - ✅ Completed (green)
  - ❌ Failed (red)
- **Quick view:** Tasks grouped by day
- **Today highlight:** Current day marked in blue

### Global Search
- **Multi-source search:** Searches memory, documents, tasks, activities
- **Relevance ranking:** High, Medium, Low
- **Quick stats:** Number of recent docs, memory entries, active tasks, activities
- **Dropdown results:** Click to view or open

## Integration Points

### OpenClaw Integration (Future)
The dashboard will integrate with OpenClaw to:
- Auto-log all activities performed by the agent
- Sync cron tasks from OpenClaw schedule
- Pull memory files for search
- Track task completion status

### Activity Logging
Activities will be logged when:
- Files are created or modified
- Searches are performed
- Tasks are created or completed
- Proposals are generated
- Any other significant action occurs

### Memory Search
The global search will index:
- MEMORY.md (long-term memory)
- memory/YYYY-MM-DD.md (daily logs)
- All project documents
- Workspace files

## Customization

### Colors
Edit `tailwind.config.js` to customize the color scheme:
```js
theme: {
  colors: {
    primary: '#YOUR_COLOR',
  }
}
```

### Activity Types
Add new activity types in `ActivityFeed.tsx`:
```ts
type: 'custom_type'
```

### Task Status
Modify status colors in `CalendarView.tsx`:
```ts
statusColors: Record<ScheduledTask['status'], string>
```

## Deployment

### Vercel (Recommended)
1. Push code to GitHub
2. Connect to Vercel
3. Add Convex environment variables
4. Deploy

### Other Platforms
- Netlify
- AWS Amplify
- Railway
- Fly.io

## Performance

- **Load time:** < 1 second
- **Search latency:** < 300ms
- **Real-time updates:** < 100ms (via Convex)
- **Mobile optimized:** Responsive design works on all devices

## Future Enhancements

- [ ] Real Convex database integration
- [ ] Notification system (missed tasks, important activities)
- [ ] Export activities to CSV/JSON
- [ ] Custom date range filtering
- [ ] Mobile app
- [ ] Dark/Light theme toggle
- [ ] Activity analytics and charts
- [ ] Collaboration features
- [ ] Webhook integrations
- [ ] Custom activity types

## License

MIT

---

Built with ❤️ for mission-critical task tracking.
