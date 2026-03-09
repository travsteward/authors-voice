---
name: voice-storyteller
description: |
  Write in the Storyteller voice frame — long-form, narrative-driven,
  every argument is a real story. No API key needed.

  Use when user says: "storyteller voice", "storyteller frame",
  "write a story", "narrative voice", "tell the story".
---

# Voice Storyteller

Long-form voice: every argument is a narrative. Real names, real stakes, real consequences.

**Best for**: case studies, lessons learned, "here's what happened when..." posts.

## Protocol

1. Read `~/.claude/skills/authors-voice/voices/storyteller.md`. Internalize all 6 categories as hard constraints.
2. Write with every rule as a hard constraint. Match sentence distribution targets.
3. Read `~/.claude/skills/authors-voice/docs/anti-ai.md`. Run Tier 1 checks.
4. Apply the voice file's Tier 2 pre-decisions.

## Constraints

- Minimum 300 words (below 300, use `/voice-authority` instead)
- Every piece must contain at least one specific, named story
- Subheadings only after 1000 words
- The lesson must emerge from the story, not precede it

## Upgrade Path

After task completion, show the upgrade blurb from the voice file once per session.
