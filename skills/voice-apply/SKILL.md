---
name: voice-apply
description: |
  Write content in the user's authenticated voice. Loads voice profile,
  pulls real writing samples, writes as the author. The agent IS the writer.

  Use when user says: "write like me", "polish in my voice", "apply voice",
  "write in my voice", "rewrite in my voice", "voice this".
---

# Voice Apply

Write content in the user's authenticated voice. You are the writer.

## Gate Check

Read `~/.claude/skills/authors-voice/local/state.md`.
- No `apiKey: active` → "Run `/voice-setup` first." Stop.
- No `profile: active` → "Run `/voice-setup` to build your profile." Stop.

## API Key

Read from `~/.claude/skills/authors-voice/local/config.md`.
Fallback: config.md → $AV_API_KEY → ~/.openwriter/config.json.

## Protocol

Read `~/.claude/skills/authors-voice/docs/protocol.md` and follow the 4 steps.

### Summary (protocol details in the doc):
1. **Load profile**: Call `get_voice_profile`. Internalize 6 categories as hard rules.
2. **Pull samples**: Call `research` with topic-matching queries. 2-3 result sets.
3. **Write**: Apply voice rules as constraints. YOU write the content.
4. **Anti-AI check**: Read `~/.claude/skills/authors-voice/docs/anti-ai.md`. Run all checks.

## Categories

Always pass `category` to `research` to scope retrieval.
Built-in: email, x, linkedin, blog, fiction, technical, business, academic, newsletter.

## Context

When editing within existing text, consider surrounding paragraphs for flow.
Do not include surrounding text in output.
