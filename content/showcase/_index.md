---
title: Showcase
description: "See Sirchmunk in action — MCP integration in Cursor IDE and the Web UI."
type: landing

sections:
  - block: hero
    content:
      title: Sirchmunk in Action
      text: 'See how Sirchmunk powers intelligent search — from AI coding assistants to standalone web interfaces.'
      primary_action:
        icon: brands/github
        text: View on GitHub
        url: "https://github.com/modelscope/sirchmunk"
      secondary_action:
        text: Read the Technical Report
        url: ../blog/technical-deep-dive/
    design:
      no_padding: true
      spacing:
        padding: [0, 0, 0, 0]
        margin: [0, 0, 0, 0]
  - block: markdown
    content:
      title: "MCP Integration in Cursor IDE"
      subtitle: "AI-to-AI Communication via the Model Context Protocol"
      text: |
        Sirchmunk integrates into AI coding assistants as an MCP tool. The demo below shows an AI agent in **Cursor IDE** invoking Sirchmunk's deep search directly within the coding workflow — no context switching, no copy-paste. Results stream back in real time with source citations and evidence.

        <img src="/media/Sirchmunk_mcp_cursor.gif" alt="Sirchmunk MCP integration in Cursor IDE" style="width:100%; border-radius:8px; margin:1rem 0;" />
    design:
      spacing:
        padding: ['3rem', '1rem', '3rem', '1rem']
  - block: markdown
    content:
      title: "Web UI — Real-Time Chat & Search"
      subtitle: "A Full-Featured Interface for Intelligent Document Search"
      text: |
        Sirchmunk's Web UI provides a chat-based interface for intelligent document search. Ask questions in natural language and receive real-time streaming answers with inline search logs, source citations, and evidence highlights.

        <img src="/media/Sirchmunk_Web.gif" alt="Sirchmunk Web UI demo" style="width:100%; border-radius:8px; margin:1rem 0;" />
    design:
      spacing:
        padding: ['3rem', '1rem', '6rem', '1rem']
---
