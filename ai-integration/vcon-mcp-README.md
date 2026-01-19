# vCon MCP Server

> Source: https://github.com/vcon-dev/vcon-mcp

An open-source Model Context Protocol (MCP) server that bridges AI assistants with conversation data management. It implements the IETF vCon standard combined with Anthropic's Model Context Protocol to enable AI systems to create, search, and analyze virtual conversations.

## Overview

The vCon MCP Server provides a standardized way for AI assistants like Claude to interact with conversation data. Think of vCon as "PDF for conversations" - a standardized container format for complete conversation records including participants, media, analysis, and attachments.

## Core Capabilities

The server provides **27+ tools** organized into five categories:

### Read Operations
- Retrieve vCons by UUID
- Execute semantic searches
- Query conversation metadata
- List conversations with filters

### Write Operations
- Create new vCon records
- Modify existing conversations
- Add parties, dialogs, and analysis
- Manage attachments

### Schema Tools
- Access vCon documentation
- Get schema definitions
- Retrieve example vCons

### Analytics
- Business intelligence on conversation data
- Aggregation queries
- Trend analysis

### Infrastructure
- Database monitoring
- Query analysis
- Connection health checks

## Key Features

- **Semantic Search**: Powered by pgvector embeddings for intelligent conversation discovery
- **Multi-Tenant Support**: Row-level security for data isolation
- **Redis Caching**: Optional caching delivers 20-50x faster reads for frequently accessed vCons
- **OpenTelemetry**: Production observability instrumentation
- **Full IETF Compliance**: Complete vCon specification support

## Deployment Options

### Local Development (stdio transport)

For Claude Desktop integration:

**Prerequisites:**
- Node.js 18+
- Supabase account

**Configuration** (`claude_desktop_config.json`):

```json
{
  "mcpServers": {
    "vcon": {
      "command": "npx",
      "args": ["vcon-mcp"],
      "env": {
        "SUPABASE_URL": "your-supabase-url",
        "SUPABASE_SERVICE_ROLE_KEY": "your-service-role-key"
      }
    }
  }
}
```

### Docker Deployment

```bash
# Pull the image
docker pull public.ecr.aws/vcon-dev/vcon-mcp:latest

# Run with environment variables
docker run -d \
  --name vcon-mcp \
  -e SUPABASE_URL=your-supabase-url \
  -e SUPABASE_SERVICE_ROLE_KEY=your-service-role-key \
  -e REDIS_URL=redis://localhost:6379 \
  -p 3000:3000 \
  public.ecr.aws/vcon-dev/vcon-mcp:latest
```

The Docker deployment supports:
- Stateful/stateless modes
- Multiple client connections
- Integration with local services

### HTTP Transport

The server implements the MCP Streamable HTTP specification (2025-03-26):

```bash
# Start HTTP server
npx vcon-mcp --transport http --port 3000
```

Features:
- Browser client support
- Remote connections
- RESTful API access
- Optional CORS support
- DNS rebinding protection

## Environment Variables

| Variable | Description | Required |
|----------|-------------|----------|
| `SUPABASE_URL` | Supabase project URL | Yes |
| `SUPABASE_SERVICE_ROLE_KEY` | Supabase service role key | Yes |
| `REDIS_URL` | Redis connection URL | No |
| `MCP_TRANSPORT` | Transport mode: stdio or http | No |
| `MCP_PORT` | HTTP port (default: 3000) | No |

## Database Schema

The implementation uses a PostgreSQL backend with:

- **8 tables** supporting the complete IETF vCon data model
- **25 optimized indexes** for query performance
- **Real-time subscriptions** for live updates
- **pgvector** for semantic search embeddings

### Tables

- `vcons` - Core vCon records
- `parties` - Conversation participants
- `dialogs` - Conversation content
- `analysis` - Processing results
- `attachments` - Supplementary files
- `tags` - Metadata key-value pairs
- `groups` - vCon collections
- `embeddings` - Vector embeddings for search

## Tool Reference

### Read Tools

```typescript
// Get a vCon by UUID
mcp.call("vcon_get", { uuid: "..." })

// Search vCons semantically
mcp.call("vcon_search", {
  query: "customer complaint about billing",
  limit: 10
})

// List vCons with filters
mcp.call("vcon_list", {
  created_after: "2024-01-01",
  tags: { customer_id: "12345" }
})
```

### Write Tools

```typescript
// Create a new vCon
mcp.call("vcon_create", {
  subject: "Support call",
  parties: [
    { tel: "+1234567890", role: "customer" },
    { tel: "+0987654321", role: "agent" }
  ]
})

// Add a dialog
mcp.call("vcon_add_dialog", {
  uuid: "...",
  type: "text",
  body: "Hello, I need help with my account",
  parties: [0, 1],
  originator: 0
})

// Add analysis
mcp.call("vcon_add_analysis", {
  uuid: "...",
  type: "sentiment",
  body: { score: 0.8, label: "positive" }
})
```

### Schema Tools

```typescript
// Get vCon schema
mcp.call("vcon_schema", {})

// Get example vCon
mcp.call("vcon_example", { type: "call_recording" })
```

## IETF vCon Compliance

The project maintains full compliance with IETF draft-ietf-vcon-vcon-core-00:

- Correct field naming per specification
- Proper type constraints
- Privacy controls
- Consent tracking
- Extension support

## Development

### Setup

```bash
git clone https://github.com/vcon-dev/vcon-mcp.git
cd vcon-mcp
npm install
```

### Testing

```bash
# Run tests
npm test

# Run with coverage
npm run test:coverage
```

### Building

```bash
npm run build
```

## Related Projects

- [vcon-server](https://github.com/vcon-dev/vcon-server) - vCon processing server
- [vcon-lib](https://github.com/vcon-dev/vcon-lib) - Python vCon library
- [vcon-admin](https://github.com/vcon-dev/vcon-admin) - Streamlit admin dashboard

## License

MIT License
