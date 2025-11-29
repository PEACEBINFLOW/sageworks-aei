# SageWorks AI — API Reference

**Version:** 1.0-alpha  
**Last Updated:** November 2025  
**Base URL:** `https://api.sageworks.ai` (Development: `http://localhost:3000`)

---

## Table of Contents

1. [Authentication](#authentication)
2. [LAW-N Network API](#law-n-network-api)
3. [Network SQL (N-SQL) API](#network-sql-n-sql-api)
4. [MindsEye Agent API](#mindseye-agent-api)
5. [Binary Ledger API](#binary-ledger-api)
6. [Temporal Query API](#temporal-query-api)
7. [WebSocket Events](#websocket-events)
8. [Error Handling](#error-handling)
9. [Rate Limits](#rate-limits)
10. [SDK Examples](#sdk-examples)

---

## Authentication

### API Key Authentication

Include your API key in the `Authorization` header:

```bash
Authorization: Bearer YOUR_API_KEY
```

### Obtaining an API Key

```bash
POST /auth/register
Content-Type: application/json

{
  "email": "your@email.com",
  "device_id": "unique-device-identifier",
  "organization": "optional-org-name"
}
```

**Response:**
```json
{
  "api_key": "sk_live_...",
  "device_id": "dev_...",
  "created_at": 1700000000000,
  "expires_at": null
}
```

### Device Registration

Register devices that will emit events to LAW-N:

```bash
POST /devices/register
Authorization: Bearer YOUR_API_KEY
Content-Type: application/json

{
  "device_type": "chrome|android|ios|iot",
  "device_name": "My Chrome Browser",
  "capabilities": ["event_emission", "query_execution"]
}
```

**Response:**
```json
{
  "device_id": "dev_abc123",
  "device_token": "dt_...",
  "network_profile": "chrome_default"
}
```

---

## LAW-N Network API

### Emit Time-Labeled Event

Send an event into the LAW-N network:

```bash
POST /law-n/emit
Authorization: Bearer YOUR_API_KEY
Content-Type: application/json

{
  "event_type": "user.action",
  "device_id": "dev_abc123",
  "timestamp": 1700000000000,
  "data": {
    "action": "click",
    "target": "button",
    "page": "/dashboard"
  },
  "context": {
    "user_id": "user_123",
    "session_id": "sess_456"
  },
  "routing_rules": {
    "priority": "high",
    "max_hops": 5,
    "target_devices": ["android", "web"]
  }
}
```

**Response:**
```json
{
  "event_id": "evt_xyz789",
  "timestamp": 1700000000000,
  "status": "routed",
  "network_path": ["chrome", "law-n-core", "android"],
  "temporal_weight": 1.0
}
```

### Query Network Topology

Get current network state:

```bash
GET /law-n/topology
Authorization: Bearer YOUR_API_KEY
```

**Response:**
```json
{
  "devices": [
    {
      "device_id": "dev_abc123",
      "device_type": "chrome",
      "status": "active",
      "last_seen": 1700000000000,
      "network_position": {
        "hop_distance": 0,
        "neighbors": ["dev_def456", "dev_ghi789"]
      }
    }
  ],
  "total_devices": 15,
  "network_health": 0.98
}
```

### Define Network Law

Create routing rules:

```bash
POST /law-n/laws
Authorization: Bearer YOUR_API_KEY
Content-Type: application/json

{
  "law_name": "prioritize_user_actions",
  "conditions": {
    "event_type": "user.*",
    "priority": "high"
  },
  "actions": {
    "max_hops": 3,
    "timeout_ms": 1000,
    "fallback": "queue"
  },
  "temporal_scope": {
    "active_from": 1700000000000,
    "active_until": null
  }
}
```

**Response:**
```json
{
  "law_id": "law_abc123",
  "status": "active",
  "created_at": 1700000000000
}
```

---

## Network SQL (N-SQL) API

### Execute Query

Run an N-SQL query across the network:

```bash
POST /nsql/query
Authorization: Bearer YOUR_API_KEY
Content-Type: application/json

{
  "query": "SELECT * FROM events WHERE timestamp > TEMPORAL_NOW() - INTERVAL '1 hour' ORDER BY temporal_weight DESC LIMIT 100",
  "parameters": {},
  "timeout_ms": 5000,
  "explain": false
}
```

**Response:**
```json
{
  "query_id": "q_xyz789",
  "rows": [
    {
      "event_id": "evt_123",
      "event_type": "user.action",
      "timestamp": 1700000000000,
      "device_id": "dev_abc123",
      "temporal_weight": 0.95,
      "data": { "action": "click" }
    }
  ],
  "row_count": 42,
  "execution_time_ms": 234,
  "network_hops": 3
}
```

### Query Execution Plan

Get query optimization details:

```bash
POST /nsql/explain
Authorization: Bearer YOUR_API_KEY
Content-Type: application/json

{
  "query": "SELECT * FROM events WHERE timestamp > TEMPORAL_NOW() - INTERVAL '1 hour'"
}
```

**Response:**
```json
{
  "plan": {
    "type": "temporal_scan",
    "estimated_rows": 1000,
    "index_used": "idx_timestamp",
    "network_distribution": {
      "nodes": ["node_1", "node_2", "node_3"],
      "strategy": "temporal_partition"
    }
  },
  "estimated_cost": 0.42,
  "estimated_time_ms": 150
}
```

### Parameterized Queries

Use parameters for safe queries:

```bash
POST /nsql/query
Authorization: Bearer YOUR_API_KEY
Content-Type: application/json

{
  "query": "SELECT * FROM events WHERE device_id = :device_id AND timestamp > :start_time",
  "parameters": {
    "device_id": "dev_abc123",
    "start_time": 1700000000000
  }
}
```

---

## MindsEye Agent API

### Create Agent

Initialize a new cognitive agent:

```bash
POST /mindseye/agents
Authorization: Bearer YOUR_API_KEY
Content-Type: application/json

{
  "agent_name": "assistant_1",
  "agent_type": "cognitive",
  "capabilities": ["memory", "reasoning", "automation"],
  "memory_config": {
    "ledger_enabled": true,
    "context_window": 10000,
    "retention_days": 90
  }
}
```

**Response:**
```json
{
  "agent_id": "agent_xyz789",
  "agent_name": "assistant_1",
  "status": "initialized",
  "created_at": 1700000000000
}
```

### Agent Interaction

Send a message to an agent:

```bash
POST /mindseye/agents/:agent_id/interact
Authorization: Bearer YOUR_API_KEY
Content-Type: application/json

{
  "message": "Summarize my activity from the last hour",
  "context": {
    "user_id": "user_123",
    "device_id": "dev_abc123"
  },
  "options": {
    "include_memory": true,
    "max_tokens": 1000
  }
}
```

**Response:**
```json
{
  "interaction_id": "int_abc123",
  "agent_response": "In the last hour, you clicked 23 buttons, viewed 5 documents, and sent 3 emails. Your most active period was between 2:15-2:30 PM.",
  "context_used": {
    "memory_entries": 47,
    "temporal_range": [1699999000000, 1700003000000]
  },
  "timestamp": 1700003000000
}
```

### Agent Memory Query

Retrieve agent's historical memory:

```bash
GET /mindseye/agents/:agent_id/memory
Authorization: Bearer YOUR_API_KEY
Query Parameters:
  - start_time: 1700000000000
  - end_time: 1700003000000
  - limit: 100
```

**Response:**
```json
{
  "agent_id": "agent_xyz789",
  "memory_entries": [
    {
      "entry_id": "mem_123",
      "timestamp": 1700001000000,
      "event_type": "user_interaction",
      "summary": "User asked about project timeline",
      "context_snapshot": { "project": "alpha", "phase": "development" }
    }
  ],
  "total_entries": 247
}
```

---

## Binary Ledger API

### Append to Ledger

Write a time-labeled entry:

```bash
POST /ledger/append
Authorization: Bearer YOUR_API_KEY
Content-Type: application/json

{
  "ledger_id": "user_123_activity",
  "entry_type": "user.action",
  "timestamp": 1700000000000,
  "data": {
    "action": "document_edit",
    "document_id": "doc_456",
    "changes": { "word_count": 1523 }
  },
  "binary_pattern": "10110101...",
  "entropy_score": 0.87
}
```

**Response:**
```json
{
  "entry_id": "entry_abc123",
  "ledger_id": "user_123_activity",
  "timestamp": 1700000000000,
  "block_hash": "0x1a2b3c...",
  "previous_hash": "0x9d8e7f..."
}
```

### Query Ledger

Retrieve ledger entries:

```bash
GET /ledger/:ledger_id/query
Authorization: Bearer YOUR_API_KEY
Query Parameters:
  - start_time: 1700000000000
  - end_time: 1700003000000
  - entry_type: user.action
  - limit: 100
```

**Response:**
```json
{
  "ledger_id": "user_123_activity",
  "entries": [
    {
      "entry_id": "entry_123",
      "timestamp": 1700001000000,
      "entry_type": "user.action",
      "data": { "action": "click" },
      "entropy_score": 0.92
    }
  ],
  "total_entries": 523,
  "ledger_integrity": "verified"
}
```

### Ledger Analytics

Get statistical analysis:

```bash
GET /ledger/:ledger_id/analytics
Authorization: Bearer YOUR_API_KEY
Query Parameters:
  - metric: entropy|frequency|pattern
  - time_window: 3600000  # 1 hour in ms
```

**Response:**
```json
{
  "ledger_id": "user_123_activity",
  "analytics": {
    "average_entropy": 0.85,
    "event_frequency": 23.5,
    "dominant_patterns": ["10110101", "01011010"],
    "temporal_distribution": {
      "peak_hours": [14, 15, 16],
      "low_hours": [2, 3, 4]
    }
  }
}
```

---

## Temporal Query API

### Relative Time Queries

Query using temporal functions:

```bash
POST /temporal/query
Authorization: Bearer YOUR_API_KEY
Content-Type: application/json

{
  "base_time": "NOW",
  "offset": {
    "hours": -24
  },
  "range": {
    "hours": 1
  },
  "aggregation": "sum",
  "metric": "event_count"
}
```

**Response:**
```json
{
  "results": [
    {
      "time_bucket": 1699999000000,
      "value": 234
    },
    {
      "time_bucket": 1699995400000,
      "value": 189
    }
  ],
  "total_buckets": 24,
  "aggregation_type": "sum"
}
```

### Temporal Weight Calculation

Calculate relevance decay:

```bash
POST /temporal/weight
Authorization: Bearer YOUR_API_KEY
Content-Type: application/json

{
  "events": [
    {
      "event_id": "evt_123",
      "timestamp": 1700000000000,
      "base_weight": 1.0
    }
  ],
  "decay_function": "exponential",
  "half_life_hours": 24
}
```

**Response:**
```json
{
  "weights": [
    {
      "event_id": "evt_123",
      "temporal_weight": 0.87,
      "decay_factor": 0.87,
      "time_since_ms": 3600000
    }
  ]
}
```

---

## WebSocket Events

### Connect to Event Stream

```javascript
const ws = new WebSocket('wss://api.sageworks.ai/stream');

ws.onopen = () => {
  // Authenticate
  ws.send(JSON.stringify({
    type: 'auth',
    api_key: 'YOUR_API_KEY',
    device_id: 'dev_abc123'
  }));
  
  // Subscribe to events
  ws.send(JSON.stringify({
    type: 'subscribe',
    channels: ['events', 'network_status', 'agent_updates']
  }));
};

ws.onmessage = (event) => {
  const data = JSON.parse(event.data);
  console.log('Received:', data);
};
```

### Event Types

**Network Event:**
```json
{
  "type": "network.event",
  "event_id": "evt_123",
  "event_type": "user.action",
  "device_id": "dev_abc123",
  "timestamp": 1700000000000,
  "data": { "action": "click" }
}
```

**Agent Update:**
```json
{
  "type": "agent.update",
  "agent_id": "agent_xyz789",
  "status": "processing",
  "timestamp": 1700000000000
}
```

**Network Status:**
```json
{
  "type": "network.status",
  "active_devices": 15,
  "network_health": 0.98,
  "timestamp": 1700000000000
}
```

---

## Error Handling

### Error Response Format

```json
{
  "error": {
    "code": "invalid_parameter",
    "message": "The 'timestamp' field must be a valid Unix timestamp",
    "details": {
      "field": "timestamp",
      "provided": "invalid",
      "expected": "number"
    },
    "request_id": "req_abc123"
  }
}
```

### Common Error Codes

| Code | HTTP Status | Description |
|------|-------------|-------------|
| `unauthorized` | 401 | Invalid or missing API key |
| `forbidden` | 403 | Insufficient permissions |
| `not_found` | 404 | Resource not found |
| `invalid_parameter` | 400 | Invalid request parameter |
| `rate_limit_exceeded` | 429 | Too many requests |
| `internal_error` | 500 | Server error |
| `temporal_violation` | 400 | Invalid temporal query |
| `network_unreachable` | 503 | LAW-N network unavailable |

---

## Rate Limits

### Default Limits

- **API Requests:** 1000 requests/hour
- **Event Emission:** 10000 events/hour
- **WebSocket Connections:** 5 concurrent connections
- **N-SQL Queries:** 100 queries/minute

### Rate Limit Headers

```
X-RateLimit-Limit: 1000
X-RateLimit-Remaining: 847
X-RateLimit-Reset: 1700003600
```

### Exceeding Limits

When rate limit is exceeded:
```json
{
  "error": {
    "code": "rate_limit_exceeded",
    "message": "API rate limit exceeded",
    "retry_after": 3600,
    "request_id": "req_abc123"
  }
}
```

---

## SDK Examples

### JavaScript/TypeScript

```typescript
import { SageWorksClient } from '@sageworks/sdk';

const client = new SageWorksClient({
  apiKey: process.env.SAGEWORKS_API_KEY,
  deviceId: 'dev_abc123'
});

// Emit event
await client.lawN.emit({
  eventType: 'user.action',
  data: { action: 'click', target: 'button' },
  timestamp: Date.now()
});

// Query with N-SQL
const results = await client.nsql.query(
  'SELECT * FROM events WHERE timestamp > TEMPORAL_NOW() - INTERVAL :hours HOUR',
  { hours: 24 }
);

// Interact with agent
const response = await client.mindseye.interact('agent_xyz789', {
  message: 'Summarize my day',
  includeMemory: true
});
```

### Python

```python
from sageworks import SageWorksClient

client = SageWorksClient(
    api_key=os.getenv('SAGEWORKS_API_KEY'),
    device_id='dev_abc123'
)

# Emit event
client.law_n.emit(
    event_type='user.action',
    data={'action': 'click', 'target': 'button'},
    timestamp=int(time.time() * 1000)
)

# Query with N-SQL
results = client.nsql.query(
    "SELECT * FROM events WHERE timestamp > TEMPORAL_NOW() - INTERVAL '1 hour'"
)

# Agent interaction
response = client.mindseye.interact(
    agent_id='agent_xyz789',
    message='Summarize my day',
    include_memory=True
)
```

---

## Versioning

API version is specified in the `Accept` header:

```bash
Accept: application/vnd.sageworks.v1+json
```

---

**Last Updated:** November 2025  
**Status:** Alpha — API subject to change  
**Feedback:** peacethabibinflow@proton.me
