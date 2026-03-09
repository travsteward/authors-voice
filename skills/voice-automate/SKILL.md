---
name: voice-automate
description: |
  Wire Author's Voice API endpoints into automated pipelines.
  Build integrations that call apply_voice or generate_content programmatically.

  Use when user says: "automate voice", "voice API", "voice integration",
  "build voice pipeline", "programmatic voice", "voice automation",
  "wire up voice API".
---

# Voice Automate

Build automated pipelines that call the Author's Voice API programmatically.

## When to Use This vs /voice-apply

- `/voice-apply` — the agent writes content itself using voice rules as constraints
- `/voice-automate` — build CODE that calls the API endpoints for automated pipelines

## API Endpoints

Base URL: `https://api.authors-voice.com/api/voice/mcp`
Protocol: JSON-RPC over HTTP, SSE responses.

### apply_voice (rewrite existing content)
```json
{"jsonrpc":"2.0","id":1,"method":"tools/call","params":{
  "name":"apply_voice","arguments":{
    "content":"text to rewrite",
    "mode":"rewrite|shrink|expand|custom",
    "category":"x|blog|email|...",
    "inputType":"human|ai|ai-assisted",
    "contextBefore":"...", "contextAfter":"..."
  }
}}
```

### generate_content (create new content)
```json
{"jsonrpc":"2.0","id":1,"method":"tools/call","params":{
  "name":"generate_content","arguments":{
    "instruction":"what to write",
    "query":"topic for retrieval",
    "category":"x|blog|email|...",
    "targetWords":500
  }
}}
```

### research (retrieve samples)
```json
{"jsonrpc":"2.0","id":1,"method":"tools/call","params":{
  "name":"research","arguments":{
    "query":"topic query",
    "category":"x|blog|email|..."
  }
}}
```

## Auth Header

```
Authorization: Bearer $AV_API_KEY
Content-Type: application/json
Accept: application/json, text/event-stream
```

## Existing Automation Example

`~/.claude/skills/tweet-writer/polish.js` — calls `apply_voice` with category `x`, mode `rewrite`, intensity `moderate`. Reference implementation for CLI automation.

## Full Tool Reference

Read `~/.claude/skills/authors-voice/docs/tools.md` for all 18 tools.
