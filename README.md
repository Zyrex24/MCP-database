# MCP Database Server

A [Model Context Protocol (MCP)](https://modelcontextprotocol.io/introduction) server that provides secure database access through GitHub OAuth authentication. This server enables AI assistants to interact with PostgreSQL databases with role-based permissions.

## Server URL

**Production Server**: `https://my-mcp-server.azxdawre.workers.dev/mcp`

## Features

- **🗄️ PostgreSQL Integration**: Direct database connection for all MCP tool calls
- **🔐 GitHub OAuth Authentication**: Secure user authentication via GitHub
- **🛡️ Role-Based Access Control**: Username-based permissions for database operations
- **📊 Schema Discovery**: Automatic table and column information retrieval
- **🛡️ SQL Injection Protection**: Built-in validation and sanitization
- **☁️ Cloudflare Workers**: Global scale and performance

## Available Tools

### 1. `listTables` (All Authenticated Users)
Discover database schema, tables, columns, and relationships.

### 2. `queryDatabase` (All Authenticated Users)  
Execute read-only SQL queries (SELECT statements only).

### 3. `executeDatabase` (Privileged Users Only)
Execute write operations (INSERT, UPDATE, DELETE, DDL) - restricted to authorized GitHub usernames.

## How to Use

### With Claude Desktop

Add this configuration to your Claude Desktop settings (`Settings -> Developer -> Edit Config`):

```json
{
  "mcpServers": {
    "database": {
      "command": "npx",
      "args": [
        "mcp-remote",
        "https://my-mcp-server.azxdawre.workers.dev/mcp"
      ]
    }
  }
}
```

### With MCP Inspector

Test the server using the [MCP Inspector](https://modelcontextprotocol.io/docs/tools/inspector):

```bash
npx @modelcontextprotocol/inspector@latest
```

Enter the server URL: `https://my-mcp-server.azxdawre.workers.dev/mcp`

## Authentication Flow

1. Connect to the MCP server
2. Authenticate via GitHub OAuth when prompted  
3. Tools become available based on your GitHub username permissions
4. Start querying your database through AI assistants

## Security Model

- **Read Access**: All authenticated GitHub users can query data
- **Write Access**: Only specific GitHub usernames can modify data
- **SQL Protection**: All queries validated to prevent injection attacks
- **User Tracking**: All operations logged with GitHub user context

## Example Usage

Once connected, you can ask your AI assistant:

- *"What tables are available in the database?"* → Uses `listTables`
- *"Show me all users created this month"* → Uses `queryDatabase` 
- *"Add a new product with name 'Widget' and price 29.99"* → Uses `executeDatabase` (if authorized)

## Transport Protocols

- **`/mcp`** - Modern streamable HTTP transport (recommended)
- **`/sse`** - Legacy Server-Sent Events transport (backward compatibility)

---

*Powered by Cloudflare Workers with PostgreSQL integration and GitHub OAuth*