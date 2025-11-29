# Getting Started with SageWorks AI

**Welcome to the SageWorks AI ecosystem!**  
This guide will help you understand, explore, and start building with the temporal and network-native computing stack.

---

## 📚 Table of Contents

1. [Understanding the Ecosystem](#understanding-the-ecosystem)
2. [Choosing Your Entry Point](#choosing-your-entry-point)
3. [Quick Start Guides](#quick-start-guides)
4. [Development Environment Setup](#development-environment-setup)
5. [Your First Project](#your-first-project)
6. [Common Patterns](#common-patterns)
7. [Next Steps](#next-steps)
8. [Getting Help](#getting-help)

---

## Understanding the Ecosystem

### What is SageWorks AI?

SageWorks AI is **not a single tool**—it's an ecosystem of interconnected systems that treat data, time, and networks as living entities rather than static storage.

**Key Concepts:**
- **Time-Native:** Every piece of data carries temporal context
- **Network-Aware:** Systems understand device topology and routing
- **Agent-Centric:** AI agents have persistent memory and evolve over time
- **Ledger-Based:** Everything flows through auditable, time-labeled ledgers

### What Can You Build?

**Examples of what others have built:**
- 🌾 **Kisan** — AI farming assistant with real-time crop diagnostics (by Yashwanth Krishna Pavush)
- 📊 **Custom Analytics** — Time-aware dashboards for business intelligence
- 🤖 **Cognitive Agents** — AI assistants that remember context across sessions
- 📱 **Cross-Device Apps** — Seamless experiences across Chrome, Android, and web

---

## Choosing Your Entry Point

### For Researchers & Theorists
**Start here:** [LAW-N Thesis on DEV](https://dev.to/peacebinflow/law-n-the-network-layer-for-minds-eye-a-research-backed-thesis-on-context-aware-data-movement-jd8)

Understand the foundational concepts before diving into code.

### For Developers
**Start here:** [MindsEye Workspace Automation](https://github.com/PEACEBINFLOW/mindseye-workspace-automation)

Get hands-on with Google Workspace integration and temporal event processing.

### For Data Scientists
**Start here:** [Kaggle Notebooks](https://www.kaggle.com/peacebinflow)

Explore binary pattern analysis, entropy scoring, and temporal datasets.

### For Product Builders
**Start here:** [5-Day AI Agents Journey](https://dev.to/peacebinflow/my-5-day-ai-agents-intensive-journey-and-how-i-built-a-google-native-mindseye-os-using-6-repos-4igh)

Learn how to build a full cognitive OS in just 5 days.

---

## Quick Start Guides

### Option 1: Explore the Website
The simplest way to understand the ecosystem:

```bash
# Clone the website
git clone https://github.com/PEACEBINFLOW/sageworks-ai.git
cd sageworks-ai

# Open in browser
open index.html
# or
python -m http.server 8000
```

Visit: `http://localhost:8000`

---

### Option 2: Run a MindsEye Example

**Prerequisites:**
- Node.js 18+
- Google Cloud account (for Workspace integration)
- Git

**Steps:**

```bash
# 1. Clone MindsEye core
git clone https://github.com/PEACEBINFLOW/minds-eye-core.git
cd minds-eye-core

# 2. Install dependencies
npm install

# 3. Set up environment
cp .env.example .env
# Edit .env with your credentials

# 4. Run development server
npm run dev

# 5. Open dashboard
open http://localhost:3000
```

**What you'll see:**
- Real-time event stream from your Google Workspace
- Temporal pattern visualization
- Agent interaction logs
- Binary ledger explorer

---

### Option 3: Try Network SQL (N-SQL)

**Prerequisites:**
- Python 3.9+
- PostgreSQL or TigerData instance
- Basic SQL knowledge

**Steps:**

```bash
# 1. Clone N-SQL playground
git clone https://github.com/PEACEBINFLOW/law-n-sql-playground.git
cd law-n-sql-playground

# 2. Create virtual environment
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# 3. Install dependencies
pip install -r requirements.txt

# 4. Configure database
cp config.example.yaml config.yaml
# Edit config.yaml with your database credentials

# 5. Start playground
python main.py

# 6. Open UI
open http://localhost:5000
```

**Try this query:**
```sql
-- Find all events from the last hour
SELECT 
  event_id,
  device_id,
  event_type,
  timestamp,
  network_hops
FROM temporal_events
WHERE timestamp > TEMPORAL_NOW() - INTERVAL '1 hour'
ORDER BY timestamp DESC;
```

---

### Option 4: Experiment on Kaggle

**No local setup needed!**

1. Visit [Kaggle - peacebinflow](https://www.kaggle.com/peacebinflow)
2. Fork any notebook (e.g., "MindsEye Notebook")
3. Run cells to see binary pattern analysis
4. Modify and experiment with your own data

**Recommended notebooks:**
- `mindseye-notebook` — Core pattern engine
- `g2n-google-layer` — Google Workspace processing
- `01-pattern-explorer` — Binary visualization

---

## Development Environment Setup

### Required Tools

```bash
# Node.js & npm (for JavaScript projects)
node --version  # Should be 18+
npm --version

# Python (for data processing)
python --version  # Should be 3.9+
pip --version

# Git (for version control)
git --version

# Docker (optional, for containerization)
docker --version
```

### Recommended IDE Setup

**VS Code Extensions:**
- ESLint
- Prettier
- Python
- GitLens
- Thunder Client (API testing)

**Configuration:**
```json
// .vscode/settings.json
{
  "editor.formatOnSave": true,
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "python.linting.enabled": true,
  "python.linting.pylintEnabled": true
}
```

### Environment Variables

Create a `.env` file in your project root:

```bash
# Google Cloud
GOOGLE_CLIENT_ID=your_client_id
GOOGLE_CLIENT_SECRET=your_secret
GOOGLE_REDIRECT_URI=http://localhost:3000/callback

# Database
DATABASE_URL=postgresql://user:pass@localhost:5432/sageworks
TIGERDATA_URL=postgres://user:pass@tiger-host:5432/temporal

# API Keys
GEMINI_API_KEY=your_gemini_key
ANTHROPIC_API_KEY=your_anthropic_key  # Optional

# Network
LAW_N_ENDPOINT=wss://law-n.sageworks.local
DEVICE_ID=dev-001

# Development
NODE_ENV=development
LOG_LEVEL=debug
PORT=3000
```

---

## Your First Project

### Project: Build a Time-Aware Todo App

**Goal:** Create a todo app where tasks are aware of their temporal context.

**Step 1: Initialize Project**
```bash
mkdir temporal-todos
cd temporal-todos
npm init -y
npm install express sqlite3 uuid
```

**Step 2: Create Basic Schema**
```sql
-- schema.sql
CREATE TABLE todos (
  id TEXT PRIMARY KEY,
  title TEXT NOT NULL,
  created_at INTEGER NOT NULL,
  completed_at INTEGER,
  temporal_weight REAL DEFAULT 1.0,
  context_snapshot TEXT
);

CREATE INDEX idx_temporal ON todos(created_at, temporal_weight);
```

**Step 3: Implement Temporal Logic**
```javascript
// temporal-todos.js
const express = require('express');
const sqlite3 = require('sqlite3').verbose();
const { v4: uuidv4 } = require('uuid');

const app = express();
const db = new sqlite3.Database(':memory:');

app.use(express.json());

// Initialize database
db.run(`
  CREATE TABLE todos (
    id TEXT PRIMARY KEY,
    title TEXT NOT NULL,
    created_at INTEGER NOT NULL,
    completed_at INTEGER,
    temporal_weight REAL DEFAULT 1.0
  )
`);

// Create todo with temporal weight
app.post('/todos', (req, res) => {
  const { title } = req.body;
  const now = Date.now();
  const id = uuidv4();
  
  db.run(
    'INSERT INTO todos (id, title, created_at, temporal_weight) VALUES (?, ?, ?, ?)',
    [id, title, now, 1.0],
    (err) => {
      if (err) return res.status(500).json({ error: err.message });
      res.json({ id, title, created_at: now });
    }
  );
});

// Get todos sorted by temporal relevance
app.get('/todos', (req, res) => {
  const now = Date.now();
  
  db.all(`
    SELECT 
      *,
      -- Calculate temporal decay
      temporal_weight * (1.0 - ((${now} - created_at) / 86400000.0)) as relevance
    FROM todos
    WHERE completed_at IS NULL
    ORDER BY relevance DESC
  `, (err, rows) => {
    if (err) return res.status(500).json({ error: err.message });
    res.json(rows);
  });
});

app.listen(3000, () => {
  console.log('Temporal todos running on http://localhost:3000');
});
```

**Step 4: Test It**
```bash
node temporal-todos.js

# In another terminal:
curl -X POST http://localhost:3000/todos \
  -H "Content-Type: application/json" \
  -d '{"title": "Learn SageWorks AI"}'

curl http://localhost:3000/todos
```

**What makes this temporal?**
- Tasks have `temporal_weight` that decays over time
- Sorting considers "when" not just "what"
- Old tasks naturally deprioritize
- You can extend this to track context snapshots

---

## Common Patterns

### Pattern 1: Time-Labeled Events
```javascript
// Always include temporal metadata
const event = {
  id: uuidv4(),
  type: 'user.action',
  data: { action: 'click', target: 'button' },
  timestamp: Date.now(),
  device_id: getDeviceId(),
  context: getCurrentContext()
};
```

### Pattern 2: Temporal Queries
```sql
-- Always think in time ranges
SELECT * FROM events
WHERE timestamp BETWEEN start_time AND end_time
ORDER BY temporal_weight DESC;
```

### Pattern 3: Agent Memory
```javascript
// Store agent interactions in ledger
async function rememberInteraction(agentId, interaction) {
  await ledger.append({
    agent_id: agentId,
    interaction,
    timestamp: Date.now(),
    context: await getAgentContext(agentId)
  });
}
```

### Pattern 4: Cross-Device Sync
```javascript
// Emit events to LAW-N for network distribution
await lawClient.emit({
  type: 'sync.request',
  source_device: deviceId,
  target_devices: ['chrome', 'android'],
  data: syncPayload,
  timestamp: Date.now()
});
```

---

## Next Steps

### Level Up Your Skills

**Week 1: Foundation**
- [ ] Read all LAW-N series posts on DEV
- [ ] Fork and run 3 Kaggle notebooks
- [ ] Set up local MindsEye instance

**Week 2: Build**
- [ ] Create a time-aware app (like the todo example)
- [ ] Integrate with Google Workspace
- [ ] Experiment with N-SQL queries

**Week 3: Contribute**
- [ ] Open an issue or PR on a repo
- [ ] Write about your experience on DEV
- [ ] Join discussions on GitHub

### Explore Advanced Topics
- **LAW-T Programming** — Build time-native applications
- **Binary Pattern Analysis** — Deep dive into entropy scoring
- **Multi-Agent Systems** — Coordinate AI agents with memory
- **Network Topology Optimization** — Advanced LAW-N routing

---

## Getting Help

### Documentation
- 📖 [Full Architecture Guide](./ARCHITECTURE.md)
- 📝 [API Reference](./API_REFERENCE.md)
- 🎓 [DEV Community Posts](https://dev.to/peacebinflow)

### Community
- 💬 **GitHub Discussions** — Ask questions, share ideas
- 📧 **Email:** peacethabibinflow@proton.me
- 🐦 **Updates:** Follow on DEV Community

### Debugging Common Issues

**Issue: "Cannot connect to LAW-N endpoint"**
```bash
# Check if endpoint is accessible
curl -I https://law-n.sageworks.local

# Verify environment variables
echo $LAW_N_ENDPOINT
```

**Issue: "Temporal queries returning empty"**
```sql
-- Check your time range
SELECT MIN(timestamp), MAX(timestamp) FROM events;

-- Use relative time functions
SELECT * FROM events 
WHERE timestamp > TEMPORAL_NOW() - INTERVAL '1 hour';
```

**Issue: "Google OAuth not working"**
```bash
# Verify redirect URI matches Google Cloud Console
# Must be exact: http://localhost:3000/callback

# Check credentials are loaded
echo $GOOGLE_CLIENT_ID
```

---

## Additional Resources

### Sample Projects
- [Temporal Todo App](https://github.com/PEACEBINFLOW/examples/temporal-todos) (Example)
- [Chrome Event Logger](https://github.com/PEACEBINFLOW/mindseye-chrome-agent-shell)
- [Android LAW-T Runtime](https://github.com/PEACEBINFLOW/mindseye-android-lawt-runtime)

### Video Tutorials
*Coming soon — subscribe to updates on DEV*

### Research Papers
- LAW-N Thesis (DEV Community)
- Temporal Intelligence Whitepaper (In progress)

---

**Ready to build the future of temporal computing?**  
Start with any quick start guide above, and don't hesitate to reach out with questions!

---

**Last Updated:** November 2025  
**Maintained by:** Peace Thabiwa  
**Contribute:** [Submit improvements on GitHub](https://github.com/PEACEBINFLOW/sageworks-ai)
