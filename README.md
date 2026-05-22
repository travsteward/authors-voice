# authors-voice

> **Constructed voice** for AI agents. The skill-based path to 60–80% voice-matched writing — no API key required, no signup, no corpus upload to anyone.

Anchors the agent to a training-data author blend (matched at [openwriter.io/voice-match](https://openwriter.io/voice-match)), then the agent does best-effort extraction of NEVER rules + presentation fingerprints + sentence stats + coined terms + curated examples from a corpus you build up on your own disk. Pure markdown — no dependencies. Gets better as you add more samples.

For plugin and programmatic flows, an optional **paid API path** is documented under `docs/api/` — same anchor system, served as a hosted endpoint.

## Replaces

This skill replaces the older `writers-voice` skill and the legacy `voice-apply`, `voice-generate`, `voice-setup`, `voice-upload`, `voice-manage`, and `voice-automate` skills. They are now one trigger: `/authors-voice`.

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
A drop-in voice mode for any AI agent. When you ask for prose, the host agent dispatches a specialized writing sub-agent configured by your voice profile, so what comes back reads like you instead of like the model's default. Agent-agnostic markdown, no API key, no signup, your corpus stays on your disk. The engine of the system.

**2. [OpenWriter](https://openwriter.io) — the writing surface (recommended pairing)**
The skill is the brain; OpenWriter is the canvas. A document editor built for writing *with* an AI agent rather than around one — your voice profile loads automatically, every paragraph stays anchored as you draft, and the corpus you build here feeds straight back into the skill. **Free tier covers the full writing flow** ([openwriter.io](https://openwriter.io) — sign up with email). The skill and OpenWriter are designed as a pair; you can use either alone, but together they're the intended experience.

**3. Author's Voice API — paid, for plugin and programmatic flows**
A hosted endpoint that runs the same anchor system as the skill. Used by OpenWriter's right-click voice actions (rewrite / shrink / expand / insert), and by anyone wiring voice-matched output into their own workflow. Reach for this only when running a skill in-session isn't possible. Docs under `docs/api/`.

The free skill + free OpenWriter combo gets you ~95% of the way. The paid API is for surfaces where the skill can't run inline.

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

## History

The local-skill half of `/authors-voice` started life as the standalone `writers-voice` skill. Its full development history — every iteration of the anchor protocol, NEVER rules, fingerprints, and tier logic — lives in the archived [travsteward/writers-voice](https://github.com/travsteward/writers-voice) repo's git log. Useful reading if you want to see how the constructed-voice architecture evolved before it was unified here.

## Credits

Built on the negative-first voice profiling architecture from [Author's Voice](https://authors-voice.com). Pairs with [OpenWriter](https://openwriter.io), the writing surface for AI agents.
