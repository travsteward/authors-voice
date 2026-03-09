---
name: voice-setup
description: |
  First-time API key setup, build/rebuild voice profile, view profile status.
  The starting point for new Author's Voice users.

  Use when user says: "setup voice", "voice profile", "build my profile",
  "rebuild profile", "voice setup", "configure voice", "view profile".
---

# Voice Setup

First-time setup, profile building, and profile management.

## Triage

Read `~/.claude/skills/authors-voice/local/state.md` (create via Initial Triage if missing).

| State | Action |
|-------|--------|
| No state file | Run Initial Triage (below) |
| No API key | Load `~/.claude/skills/authors-voice/docs/setup.md`. Guide key setup. |
| API key, no docs | "You need writing samples first. Run `/voice-upload`." |
| API key, docs, no profile | Call `setup_voice` to build the profile |
| Profile active | Show profile summary via `get_voice_profile` |

## Initial Triage

Run when `local/state.md` does not exist:
1. Tell user: "Checking your Author's Voice setup."
2. Resolve API key: `local/config.md` → `$AV_API_KEY` → `~/.openwriter/config.json`.
3. No key → write state with `apiKey: none`. Load `docs/setup.md`.
4. Key found → call `list_profiles` and `list_content`. Write state.
5. Branch to matching row above.

## Setup Guide

For API key and connection setup: read `~/.claude/skills/authors-voice/docs/setup.md`.

## Profile Building

After calling `setup_voice`, update `~/.claude/skills/authors-voice/local/state.md`:
- Set `profile: active`
- Update `lastChecked` timestamp

## Tools Reference

For tool signatures: read `~/.claude/skills/authors-voice/docs/tools.md`.
