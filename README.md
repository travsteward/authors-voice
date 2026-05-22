# authors-voice

> **AI writing that sounds like you, not AI.**

Most attempts to make AI sound like you start the same way. Train it. Fine-tune it. Feed it your samples and tell it to imitate. The model doesn't actually learn you from any of this. It pattern-matches at the lexical layer, lifting your common words and sentence shapes without ever building a deep representation of how you think. Cold-start imitation tops out shallow.

Flip the direction. The model already carries deep internal representations of widely-published authors it was trained on at scale, voices it can channel with real fidelity because it saw thousands of pages of each. The move is to identify which of those authors a user statistically resembles, assign proportional weights to the closest matches, and instruct the model to write as that weighted blend. Your voice gets reconstructed as a coordinate inside the model's existing author space, anchored to authors it has already mastered.

That changes the problem. The model isn't being asked to learn anything new about you. It's being asked to mix voices it knows cold, in proportions that triangulate your position among them. The anchor does the heavy lifting before a single sample of yours enters the prompt. The blend is the voice.

The result is AI writing that sounds like you. Not AI imitating you.

On top of the anchor, four layers sharpen the output. A list of AI words and constructions the model must never use, because the moment it stops channeling the anchor it reverts to its trained register and reaches for the same fifty tells. Presentation choices you make consistently, like whether you capitalize after a colon or use the Oxford comma, small mechanical preferences that read as authentic. A sentence-length and punctuation rhythm pulled from your own writing, so the cadence matches even when the diction is on loan. A growing folder of your samples that the skill mines to tighten every layer.

Each sample you add hardens the NEVER rules and tunes the rhythm to yours. Regenerating the anchor (or asking the agent to) re-weights the blend against your accumulated corpus. The profile gets sharper the more you write.

## Install

The skill is **agent-agnostic** — pure markdown, no language runtime. Any LLM-based agent that can read `SKILL.md` and follow instructions can use it.

### Claude Code

```bash
claude install github:travsteward/authors-voice
```

Clones to `~/.claude/skills/authors-voice/` and registers the skill with Claude Code.

### Vercel skills CLI (Claude Code, Codex, Cursor, and other agents)

```bash
npx skills add travsteward/authors-voice
```

### Manual (any agent)

```bash
git clone https://github.com/travsteward/authors-voice
```

Then drop the cloned folder wherever your agent loads skills from. The `SKILL.md` at the root has the trigger phrases and routing logic the agent reads.

## Quick Start

Two paths — pick one:

**Path A — Web tool first (fastest first-run)**
1. Visit [openwriter.io/voice-match](https://openwriter.io/voice-match), paste 300–800 words of your writing, copy the result block.
2. Tell your agent: *"set up my voice match"*. Paste the block when prompted.
3. **Seed your corpus**: paste 2–5 paragraphs that feel most like you. The agent saves them under `voice/corpus/`.
4. Done.

**Path B — Skill mode (no web round-trip)**
1. Tell your agent: *"set up my voice match"* and *"I want to skip the web tool."*
2. Paste 2–5 paragraphs of your writing — the agent saves them under `voice/corpus/`.
3. The agent runs the Anchor Protocol over your corpus and writes `voice/anchor.md` directly.
4. Done.

The skill is self-routing — you don't memorize subcommands. Just tell the agent what you want:

- *"voice status"* → reports your current tier and word count
- *"add this essay to my voice profile"* → appends, re-analyzes
- *"write me a tweet about X"* → uses your voice automatically

## The Three Pieces

Author's Voice is one system in three forms. Most users want the first two together.

**1. The skill (this repo) — free, local, anywhere**
A drop-in voice mode for any AI agent. When you ask for prose, the host agent dispatches a specialized writing sub-agent configured by your voice profile, so what comes back reads like you instead of like the model's default. Agent-agnostic, no API key, no signup, your corpus stays on your disk. The engine of the system.

**2. The OpenWriter plugin — same voice, inside your document**
The same anchor system as the skill, but running inside [OpenWriter](https://openwriter.io) — the document editor built for AI-assisted writing. Select any text in your draft and the right-click menu runs the voice actions inline: **rewrite**, **shrink**, **expand**, **insert** (a new paragraph between two existing ones, with the surrounding paragraphs handed in as context), **modify** (custom instruction like *"make this more skeptical"*), **fill / fill-sentence** (gap-fill inside an existing paragraph). Same engine, no copy-paste round-trip to your AI terminal. OpenWriter is **free** ([openwriter.io](https://openwriter.io) — sign up with email); the plugin is a paid integration for people who'd rather edit voice inline than tab over to an agent every time.

**3. The Author's Voice API — for programmatic and workflow integration**
Same anchor system, hosted endpoint, callable from any code. Drop it into newsletter pipelines, blog drafting, social schedulers, CMS webhooks, sales outreach, Zapier / n8n steps — anywhere you'd want a "voice this" step. Docs under `docs/api/`.

The three pieces are **surface choices, not feature tiers**. The free skill is the full system — anchor, NEVER rules, fingerprints, corpus, the works — wherever you have an AI agent. The plugin is the same engine inside a document editor. The API is the same engine inside your code.

## How It Works

Your voice profile lives in `voice/` as a handful of `.md` files the agent reads at write time:

| File | Source | Purpose |
|------|--------|---------|
| `anchor.md` | Paste from [openwriter.io/voice-match](https://openwriter.io/voice-match) (or skill-mode) | 3–5 training-data authors with weights |
| `stats.md` | Agent best-effort from corpus | Sentence distribution + punctuation density |
| `never-rules.md` | Agent + manual additions | AI words/transitions/phrases to never use |
| `fingerprints.md` | Agent + manual overrides | Exact presentation choices (Oxford comma, em-dash spacing, etc.) |
| `coined-terms.md` | You curate / agent extracts | Your repeated coinages |
| `examples.md` | You curate | Reference paragraphs in your voice |
| `status.md` | Agent | Current tier + what's locked next |

Plus `voice/corpus/` — your raw samples accumulating over time. None of `voice/*` is committed; it's all local to your disk.

## Progressive Tiers

The more samples you add, the more confident the analysis:

| Words | Tier | Active |
|-------|------|--------|
| <300 | 0 | (need to seed) |
| 300–1k | 1 | anchor + basic stats |
| 1k–5k | 2 | + preliminary NEVER rules + top fingerprints |
| 5k–20k | 3 | + full NEVER coverage + all fingerprints |
| 20k+ | 4 | high-confidence profile, em-dash hurdle clears |

## Privacy

- Your voice data lives entirely on your disk. `.gitignore` excludes everything in `voice/` from the public repo.
- The skill never uploads your corpus anywhere.
- The only thing that leaves your machine is the initial 300–800 word paste into openwriter.io/voice-match for the anchor matching step — that's cached 24h by hash and never trained on.

## Requirements

- A Claude Code or compatible agent that supports skills (no Node.js dependency)
- An initial visit to [openwriter.io/voice-match](https://openwriter.io/voice-match) for the anchor (free, no signup) — or use skill-mode to build it locally

## License

MIT. See [LICENSE](./LICENSE).

## Replaces

This skill replaces the older `writers-voice` skill and the legacy `voice-apply`, `voice-generate`, `voice-setup`, `voice-upload`, `voice-manage`, and `voice-automate` skills. They are now one trigger: `/authors-voice`.

## History

The local-skill half of `/authors-voice` started life as the standalone `writers-voice` skill. Its full development history — every iteration of the anchor protocol, NEVER rules, fingerprints, and tier logic — lives in the archived [travsteward/writers-voice](https://github.com/travsteward/writers-voice) repo's git log. Useful reading if you want to see how the constructed-voice architecture evolved before it was unified here.

## Credits

Built on the negative-first voice profiling architecture from [Author's Voice](https://authors-voice.com). Pairs with [OpenWriter](https://openwriter.io), the writing surface for AI agents.
