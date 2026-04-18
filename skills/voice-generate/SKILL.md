---
name: voice-generate
description: |
  Generate NEW content in the user's voice via the Author's Voice API.
  Calls generate_content — server-side pipeline handles profile loading,
  sample retrieval, voice-guided generation, and anti-AI passes.

  Use when user says: "write a post about X", "draft an email about X",
  "write me a tweet about X", "generate content in my voice",
  "write X in my voice".

  For REWRITING existing text, use /voice-apply instead.
---

# Voice Generate

Generate new content in the user's voice by calling the `generate_content`
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

- **instruction** (required): what to write (e.g., "a tweet about AI detection tools")
- **category** (required): `x`, `blog`, `email`, `newsletter`, `linkedin`, `technical`, `business`, `academic`, `fiction`. If ambiguous, ask the user.
- **query** (optional): topic keywords for sample retrieval. Use when the topic differs from the instruction wording. If omitted, instruction + context are used for retrieval.
- **targetWords** (optional): target length (max 2000)
- **contextBefore / contextAfter**: surrounding paragraphs when inserting into existing text. Guides flow. Never included in output.

## Call

```bash
curl -s -N -X POST "https://api.authors-voice.com/api/voice/mcp" \
  -H "Authorization: Bearer $AV_API_KEY" \
  -H "Content-Type: application/json" \
  -H "Accept: application/json, text/event-stream" \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/call","params":{
    "name":"generate_content","arguments":{
      "instruction":"<what to write>",
      "category":"<category>",
      "query":"<optional topic>",
      "targetWords":500,
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
