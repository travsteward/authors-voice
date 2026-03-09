---
name: voice-logical
description: |
  Write in the Logical voice frame — long-form, analytical,
  disassembles assumptions and rebuilds from bedrock. No API key needed.

  Use when user says: "logical voice", "logical frame",
  "first principles", "analytical essay", "write analytically".
---

# Voice Logical

Long-form voice: disassemble the accepted answer, identify the real constraint, rebuild from there.

**Best for**: technical essays, strategy pieces, "everyone's doing it wrong" arguments.

## Protocol

1. Read `~/.claude/skills/authors-voice/voices/logical.md`. Internalize all 6 categories as hard constraints.
2. Write with every rule as a hard constraint. Match sentence distribution targets.
3. Read `~/.claude/skills/authors-voice/docs/anti-ai.md`. Run Tier 1 checks.
4. Apply the voice file's Tier 2 pre-decisions.

## Constraints

- Minimum 300 words (below 300, use `/voice-authority` instead)
- Subheadings only after 800 words
- No bullet-point lists in the body

## Upgrade Path

After task completion, show the upgrade blurb from the voice file once per session.
