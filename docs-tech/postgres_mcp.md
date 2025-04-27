# PostgreSQL MCP Server

## Overview

A Model Context Protocol server that provides read-only access to PostgreSQL databases. This server enables LLMs to inspect database schemas and execute read-only queries.

## Components

### Tools

-   **query**: Execute read-only SQL queries against the connected database
    *   Input: `sql` (string): The SQL query to execute
    *   All queries are executed within a READ ONLY transaction

### Resources

The server provides schema information for each table in the database:

-   **Table Schemas** (`postgres://<host>/<table>/schema`): JSON schema information for each table. Includes column names and data types. Automatically discovered from database metadata.

## Configuration

### Usage with Claude Desktop

To use this server with the Claude Desktop app, add the following configuration to the "mcpServers" section of your `claude_desktop_config.json`:

**Docker**

When running docker on macos, use `host.docker.internal` if the server is running on the host network (eg localhost). Username/password can be added to the postgresql url with `postgresql://user:password@host:port/db-name`.

```json
{
  "mcpServers": {
    "postgres": {
      "command": "docker",
      "args": [
        "run",
        "-i",
        "--rm",
        "mcp/postgres",
        "postgresql://host.docker.internal:5432/mydb"
      ]
    }
  }
}
```

**NPX**

```json
{
  "mcpServers": {
    "postgres": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-postgres",
        "postgresql://localhost/mydb"
      ]
    }
  }
}
```

Replace `/mydb` with your database name.

### Usage with VS Code

For quick installation, use one of the one-click install buttons below...

Install with NPX in VS Code Install with NPX in VS Code Insiders

Install with Docker in VS Code Install with Docker in VS Code Insiders

For manual installation, add the following JSON block to your User Settings (JSON) file in VS Code. You can do this by pressing `Ctrl + Shift + P` and typing `Preferences: Open User Settings (JSON)`.

Optionally, you can add it to a file called `.vscode/mcp.json` in your workspace. This will allow you to share the configuration with others.

Note that the `mcp` key is not needed in the `.vscode/mcp.json` file.

**Docker**

Note: When using Docker and connecting to a PostgreSQL server on your host machine, use `host.docker.internal` instead of `localhost` in the connection URL.

```json
{
  "mcp": {
    "inputs": [
      {
        "type": "promptString",
        "id": "pg_url",
        "description": "PostgreSQL URL (e.g. postgresql://user:pass@host.docker.internal:5432/mydb)"
      }
    ],
    "servers": {
      "postgres": {
        "command": "docker",
        "args": [
          "run",
          "-i",
          "--rm",
          "mcp/postgres",
          "${input:pg_url}"
        ]
      }
    }
  }
}
```

**NPX**

```json
{
  "mcp": {
    "inputs": [
      {
        "type": "promptString",
        "id": "pg_url",
        "description": "PostgreSQL URL (e.g. postgresql://user:pass@localhost:5432/mydb)"
      }
    ],
    "servers": {
      "postgres": {
        "command": "npx",
        "args": [
          "-y",
          "@modelcontextprotocol/server-postgres",
          "${input:pg_url}"
        ]
      }
    }
  }
}
```

## Building

**Docker:**

```bash
docker build -t mcp/postgres -f src/postgres/Dockerfile .
```

## License

This MCP server is licensed under the MIT License. This means you are free to use, modify, and distribute the software, subject to the terms and conditions of the MIT License. For more details, please see the LICENSE file in the project repository.
