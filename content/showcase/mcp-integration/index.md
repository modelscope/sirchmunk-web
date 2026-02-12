---
title: "MCP Integration — AI-to-AI Communication"
summary: "Seamless integration with Claude Desktop, Cursor IDE, and any MCP-compliant AI assistant."
links:
  - type: site
    url: https://modelcontextprotocol.io
date: 2026-02-05
---

Sirchmunk exposes its intelligent search capabilities through the Model Context Protocol (MCP), enabling zero-friction AI-to-AI communication. Any MCP-compliant AI assistant can invoke Sirchmunk's search as a native tool.

## Sirchmunk MCP in Cursor IDE

The demo below shows Sirchmunk running as an MCP tool inside Cursor IDE. The AI agent invokes Sirchmunk's deep search directly within the coding workflow — no context switching, no copy-paste. Results stream back in real time, complete with source citations and evidence.

![Sirchmunk MCP integration in Cursor IDE](Sirchmunk_mcp_cursor.gif "Sirchmunk MCP tool invoked inside Cursor IDE for intelligent local file search")

**Features:**
- stdio and HTTP transport modes
- Works with Claude Desktop, Cursor IDE, and more
- Search, directory scanning, and knowledge management as MCP tools
- Easy configuration via `mcp_config.json`
- Real-time streaming results with source citations
