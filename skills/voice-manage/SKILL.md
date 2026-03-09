---
name: voice-manage
description: |
  List, update, retag, or delete writing samples. Manage connections
  to Google Drive and Notion.

  Use when user says: "list my content", "list my docs", "delete content",
  "retag document", "manage connections", "disconnect drive",
  "update categories", "voice content".
---

# Voice Manage

CRUD operations on the writing corpus and connection management.

## Gate Check

Read `~/.claude/skills/authors-voice/local/state.md`.
- No `apiKey: active` → "Run `/voice-setup` first." Stop.

## API Key

Read from `~/.claude/skills/authors-voice/local/config.md`.
Fallback: config.md → $AV_API_KEY → ~/.openwriter/config.json.

## Operations

| Task | Tool | Notes |
|------|------|-------|
| List documents | `list_content` | Optional `category` filter |
| Retag a document | `update_content` | Change categories or authenticity |
| Delete a document | `delete_content` | Permanent — confirm with user first |
| Check connections | `list_connections` | Shows Google Drive, Notion status |
| Disconnect provider | `disconnect` | Revokes OAuth. Reconnect at authors-voice.com |

## After Changes

- If documents were deleted or retagged: consider running `/voice-setup` to rebuild profile
- Update `~/.claude/skills/authors-voice/local/state.md` with new doc count

## Tools Reference

For tool signatures: read `~/.claude/skills/authors-voice/docs/tools.md`.
For troubleshooting: read `~/.claude/skills/authors-voice/docs/troubleshooting.md`.
