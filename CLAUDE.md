# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Build and Run Commands

```bash
# Install dependencies
pnpm install

# Build the project
pnpm build

# Run the client (requires a MCP server script path)
node build/index.js <path_to_server_script>
```

## Environment Setup

- Requires Node.js >= 22
- Uses pnpm as package manager
- Create a `.env` file with `ANTHROPIC_API_KEY`
- Optionally set `ANTHROPIC_MODEL` (defaults to `claude-3-5-sonnet-20241022`)

## Architecture

This is a Model Context Protocol (MCP) client that connects Claude to MCP servers.

**Core flow:**
1. `MCPClient` initializes both Anthropic SDK and MCP SDK clients
2. `connectToServer()` spawns a subprocess to run an MCP server (.js or .py file) via stdio transport
3. The client discovers available tools from the MCP server and converts them to Anthropic tool format
4. `processQuery()` sends user queries to Claude, handles tool calls by delegating to the MCP server, and returns combined responses
5. `chatLoop()` provides an interactive REPL interface

**Key dependencies:**
- `@anthropic-ai/sdk` - Claude API client
- `@modelcontextprotocol/sdk` - MCP client implementation using stdio transport
