---
name: voice-upload
description: |
  Upload writing samples from Drive, Notion, URLs, or raw markdown.
  Handles authenticity tagging and category assignment.

  Use when user says: "import from drive", "import from notion",
  "upload writing sample", "import content", "bulk import",
  "add my writing", "import samples".
---

# Voice Upload

Upload writing samples to build or strengthen the voice corpus.

## Gate Check

Read `~/.claude/skills/authors-voice/local/state.md`.
- No `apiKey: active` → "Run `/voice-setup` first to get an API key." Stop.

## API Key

Read from `~/.claude/skills/authors-voice/local/config.md`.
Fallback: config.md → $AV_API_KEY → ~/.openwriter/config.json.

## Import Workflow

Read `~/.claude/skills/authors-voice/docs/import.md` and follow it completely.

### Critical Rules (always enforced):
1. **Always tag authenticity** — ask the user per document: Human, AI, AI-Assisted
2. **Never assume human-written** — many users have mixed corpora
3. **Never bulk-import without confirmation** per document
4. **Always assign categories** — email, x, linkedin, blog, fiction, technical, business, academic, newsletter
5. **Wrong categories corrupt voice quality** — when in doubt, ask

## Tools Reference

For tool signatures and parameters: read `~/.claude/skills/authors-voice/docs/tools.md`.

## After Import

- Run `/voice-setup` to rebuild the profile if this is new content
- Update `~/.claude/skills/authors-voice/local/state.md` with new doc count
