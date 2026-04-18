---
name: voice-apply
description: |
  Rewrite existing content in the user's voice via the Author's Voice API.
  Calls rewrite — server-side pipeline handles profile loading, sample
  retrieval, voice-guided rewrite, and anti-AI passes.

  Use when user says: "rewrite in my voice", "polish in my voice",
  "apply voice", "voice this", "polish this".

  For NEW content (no source text), use /voice-generate instead.
---

# Voice Apply

Rewrite the user's existing text in their voice by calling the `rewrite`
endpoint. The server does the voice work — you gather inputs, call the API,
return the result. Do not attempt to emulate the voice yourself.

## Gate Check

Read `~/.claude/skills/authors-voice/local/state.md`.
- No `apiKey: active` → "Run `/voice-setup` first." Stop.
- No `profile: active` → "Run `/voice-setup` to build your profile." Stop.

## API Key

Read from `~/.claude/skills/authors-voice/local/config.md`.
Fallback: config.md → `$AV_API_KEY` → `~/.openwriter/config.json`.

## Inputs to Collect

- **content** (required): the text to rewrite
- **category** (required): `x`, `blog`, `email`, `newsletter`, `linkedin`, `technical`, `business`, `academic`, `fiction`. If ambiguous, ask the user.
- **mode** (default `rewrite`): `rewrite` | `shrink` | `expand` | `custom`
- **inputType** (default `ai-assisted`): `human` (author's own writing — preserve quirks), `ai` (generic AI — rewrite from scratch), `ai-assisted` (mixed)
- **contextBefore / contextAfter**: surrounding paragraphs when editing inside a larger document. Guides flow. Never included in output.

## Call

```bash
curl -s -N -X POST "https://api.authors-voice.com/api/voice/mcp" \
  -H "Authorization: Bearer $AV_API_KEY" \
  -H "Content-Type: application/json" \
  -H "Accept: application/json, text/event-stream" \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/call","params":{
    "name":"rewrite","arguments":{
      "content":"<text>",
      "mode":"rewrite",
      "category":"<category>",
      "inputType":"ai-assisted",
      "contextBefore":"<optional>",
      "contextAfter":"<optional>",
      "format":"plaintext"
    }}}'
```

## Response

Response is SSE. Find the line starting `data: `, parse the JSON, extract:

```
result.content[0].text
```

Return that text verbatim. The server has already run retrieval, profile
application, and anti-AI passes — no further editing.

If the text parses as JSON, extract `.content` or `.text` from it (defensive
fallback — most responses are plain text).

## Errors

- `401` → API key invalid. Run `/voice-setup`.
- `404 profile not found` → profile not built. Run `/voice-setup`.
- `429` → rate limit. Wait and retry.
- Timeout (>30s) → retry once, then surface the error.

## Full Protocol

See `~/.claude/skills/authors-voice/docs/protocol.md` for endpoint reference.
