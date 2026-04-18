# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/), and this project adheres to [Semantic Versioning](https://semver.org/).

## [0.1.0] - 2026-04-18

First versioned release. Refactored from agent-writes-with-rules to API-only.

### Added
- `/voice-generate` — generate new content in the user's voice via `generate_content` API
- Inline curl invocation pattern — skills call the API directly instead of emulating voice from profile rules
- Explicit API-call protocol doc at `docs/protocol.md`
- README, CHANGELOG, and version metadata in SKILL.md frontmatter

### Changed
- `/voice-apply` now calls `apply_voice` API directly (previously asked the agent to write from profile rules, which did not meet quality bar)
- Tool count: 17 (down from 19 — removed `research` and `evaluate_voice_match` from documented surface)
- `docs/protocol.md` rewritten around the two API endpoints (`apply_voice`, `generate_content`) — no more 4-step agent-writes protocol

### Removed
- `/voice-authority`, `/voice-provocateur`, `/voice-logical`, `/voice-storyteller`, `/voice-business` — the five generic voice frames moved to the `openwriter` skill, where they belong
- `voices/` directory — frame files live in openwriter now
- `docs/generic-voices.md` — frame spec lives in openwriter now
- `research` tool from documented surface — was only useful for the deprecated agent-writes flow
- `evaluate_voice_match` tool from documented surface — was only useful for the deprecated self-eval regen loop
