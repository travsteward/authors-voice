# Voice Emulation Protocol

This is how agents write in the author's voice. Load the voice profile, pull samples, write with the profile as constraints, then run anti-AI detection to verify authenticity.

## API Base

All endpoints require `Authorization: Bearer $AV_API_KEY` header.

```
BASE_URL=https://api.authors-voice.com
```

## Step 1: Load the Voice Profile

Fetch the full linguistic fingerprint. Internalize all 6 categories — these are your behavioral rules, not suggestions.

```bash
curl -s "${BASE_URL}/api/voice/profiles/default" \
  -H "Authorization: Bearer $AV_API_KEY"
```

Returns JSON with `profile` field containing 6 categories (Diction, Syntax, Punctuation, Rhetoric, Discourse, Idiolect) + sentence distribution.

Use `?format=detailed` for profile ID and summary.

## Step 2: Pull Writing Samples

Search the author's writing for topic-matching examples. Read 2-3 result sets to absorb their actual phrasing, rhythm, and word choices. These samples are your ground truth — the profile rules describe patterns, the samples show them in action.

```bash
curl -s -X POST "${BASE_URL}/api/voice/research" \
  -H "Authorization: Bearer $AV_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"query": "topic keywords here", "category": "x"}'
```

Returns JSON: `examples[]` (content, wordCount, authenticity, docId), `totalExamples`, `matchQuality`.

**Always pass category**: Scope retrieval to the right writing style (`x`, `blog`, `email`, `newsletter`, etc.).

## Step 3: Write with Rules

Apply the voice profile rules as constraints while writing:
- **Diction**: Use the author's word register, not yours
- **Syntax**: Match their clause complexity and sentence patterns
- **Punctuation**: Use their interpolation marks (em-dash vs parentheses vs comma splices)
- **Rhetoric**: Match their epistemic stance (hedging vs assertion)
- **Discourse**: Follow their structural patterns (how they open, transition, close)
- **Idiolect**: Reproduce their signature quirks and recurring phrases
- **Sentence Distribution**: Mix short/medium/long sentences to match their percentages

## Step 4: Anti-AI Detection

Read `docs/anti-ai.md` and run all detection checks against your output.

---

## Generic Voice Frames

Voice frames are now separate slash commands. No API key needed — each reads a local `.md` file and applies it as behavioral constraints.

- `/voice-authority` — short-form, teaches from experience
- `/voice-provocateur` — short-form, contrarian engagement
- `/voice-logical` — long-form, analytical (formerly first-principles)
- `/voice-storyteller` — long-form, narrative-driven
- `/voice-business` — high-status brevity (formerly business-framed)

Full spec: `docs/generic-voices.md`
