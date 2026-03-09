# Generic Voices System

> **Superseded locally.** The voice frames are now individual slash commands: `/voice-authority`, `/voice-provocateur`, `/voice-logical`, `/voice-storyteller`, `/voice-business`. This doc is kept as reference for the public distribution (`public/`).

## What Are Generic Voices?

Generic voices are pre-built `.md` files containing a full 6-category linguistic fingerprint that agents read directly during content generation. Each voice is a distinct **frame** — a communication posture with its own strategy, not just a register or tone. They produce human-sounding output without requiring writing samples, API calls, RAG retrieval, or a custom voice profile.

**Business rationale**: Generic voices are a free on-ramp. Users who don't have writing samples (or don't write much) get immediate value — anti-AI output that sounds like a real person writing with intent. The experience demonstrates what voice-matching feels like, which naturally upsells custom profiles built from the user's actual writing.

Generic voices are **not the product**. They're a gateway. Custom profiles are always better because they capture the user's real patterns, vocabulary, and quirks. Generic voices capture a specific *posture* — good enough to beat AI detection and communicate with purpose, not good enough to sound like a specific person.

---

## Architecture

### How Generic Voices Fit the Skill Protocol

The existing Voice Emulation Protocol (SKILL.md) has 4 steps:

1. Load the voice profile (`get_voice_profile`)
2. Pull writing samples (`research`)
3. Write with rules
4. Anti-AI detection

Generic voices **skip steps 1-2 entirely**. Instead of calling the API, the agent reads a `.md` file from the `voices/` directory. The file contains the same 6-category fingerprint format that the API returns — diction, syntax, punctuation, rhetoric, discourse, idiolect — plus sentence distribution targets and pre-resolved Tier 2 decisions.

### File Location

```
~/.claude/skills/authors-voice/voices/
  authority.md        — short-form, teaches from experience
  provocateur.md      — short-form, contrarian engagement
  logical.md          — long-form, disassembles and rebuilds
  storyteller.md      — long-form, narrative-driven arguments
  business.md         — business comms, high-status brevity
```

### No API Dependency

Generic voices cost nothing. No `AV_API_KEY` required, no network calls, no database lookups. The agent reads a local file and applies the rules. This makes them ideal for:
- Users evaluating the skill before signing up
- Quick tasks where voice setup isn't worth the overhead
- Offline or rate-limited scenarios

---

## Voice File Schema

Every voice file follows this template:

```
# Frame Name
Posture description + when to use this voice.

## Diction
2-5 imperative rules about word choice.

## Syntax
2-5 imperative rules about sentence construction.

## Punctuation
2-5 imperative rules about orthographic mechanics.

## Rhetoric
2-5 imperative rules about argument-level patterns.

## Discourse
2-5 imperative rules about idea connection and flow.

## Idiolect
2-5 imperative rules about distinctive markers.

## Sentence Distribution
Target percentages for short/medium/long/very-long sentences.

## Tier 2 Decisions
Pre-resolved answers to voice-gated anti-AI checks.
(These vary per frame — business writing legitimately allows
brevity that would read as incomplete in essays.)

## Use-Case Constraints
Boundaries: what this voice is for and what it isn't.

## Upgrade Path
Single paragraph shown after task completion.
```

**Rule format**: Every rule is a single imperative sentence. Concrete enough that two agents reading the same rule would produce similar output. No vibes, no adjectives-as-instructions.

**No inline exemplars**: Static examples bloat context and become patterns agents over-index on. If a rule isn't sufficient without an example, the rule is too vague — rewrite the rule.

---

## The Five Frames

### Short-Form (Social Media, Threads, Posts)

#### 1. Authority (`authority.md`)

**Posture**: You've done the thing. You teach from experience, not theory. Credibility comes from specificity, not credentials.

**Key differentiators**:
- First-person experiential language: "I built", "I tested", "I shipped"
- Specificity IS the authority signal — numbers, names, dates
- Opens with conclusions, not context. The lesson goes first.
- Sentence distribution: 55% short (1-8 words), avg 9 words

**Best for**: Teaching posts, experience-based advice, "here's what I learned" threads.

---

#### 2. Provocateur (`provocateur.md`)

**Posture**: Consensus is wrong. You see what others miss. Engagement comes from disagreement — the reader either nods hard or fires back.

**Key differentiators**:
- Opens with a claim that contradicts what the audience believes (non-negotiable)
- Sharp, confrontational verbs: "kills", "broke", "destroyed"
- Uses "everyone", "nobody", "always", "never" to draw hard lines
- Sentence distribution: 60% short (1-7 words), avg 8 words

**Best for**: Hot takes, contrarian threads, challenging industry consensus, engagement farming.

---

### Long-Form (Essays, Blog Posts, Articles)

#### 3. Logical (`logical.md`)

**Posture**: Most people copy solutions without examining the premise. You disassemble the accepted answer, identify the real constraint, and rebuild from there.

**Key differentiators**:
- Names the conventional assumption, then dismantles it
- Questions as structural pivots: "But why does everyone assume X?"
- One-sentence paragraphs for core insights
- Sentence distribution: wide range, 25/35/25/15 split

**Best for**: Technical essays, strategy pieces, "everyone's doing it wrong" arguments, Paul Graham-style thinking.

---

#### 4. Storyteller (`storyteller.md`)

**Posture**: Every argument is a narrative. You don't explain concepts — you show what happened. Real names, real stakes, real consequences.

**Key differentiators**:
- Opens with a scene or moment, never a thesis statement
- Sensory, concrete language: dates, names, dialogue fragments
- Lesson emerges from the story, not before it
- Sentence distribution: skews longer for narrative momentum (40% medium, 28% long)

**Best for**: Case studies, lessons learned, "here's what happened when..." posts, content that needs emotional resonance.

---

### Business Communication

#### 5. Business (`business.md`)

**Posture**: High-status. Brevity signals confidence. You don't over-explain because you don't need approval. Not chasing — deciding.

**Key differentiators**:
- Sentences max at 12 words. If it takes more, you're justifying yourself.
- First sentence is the ask, the decision, or the answer. Every time.
- No filler: no "just wanted to", no "I hope this finds you well"
- Sentence distribution: 60% short (1-6 words), avg 7 words

**Best for**: Emails, proposals, client replies, any business communication where status matters.

---

## Modified Protocol for Generic Voices

When using a generic voice, the agent follows this 5-step protocol instead of the standard 4-step Voice Emulation Protocol:

### Step 1: Select the Frame
Determine which frame matches the task. If the user specifies one, use it. If not, infer:

**Short-form (social media, threads, posts)**:
- Teaching / sharing experience → `authority.md`
- Challenging consensus / hot takes → `provocateur.md`

**Long-form (essays, blog posts, articles)**:
- Analytical / breaking down assumptions → `logical.md`
- Narrative / telling a story → `storyteller.md`

**Business communication**:
- Emails, proposals, client docs → `business.md`

If ambiguous, ask the user.

### Step 2: Load the Voice File
Read the selected `.md` file from `~/.claude/skills/authors-voice/voices/`. Internalize all 6 categories as behavioral constraints.

### Step 3: Write with Rules
Apply every rule from the voice file as a hard constraint. Match the sentence distribution targets. Use the Tier 2 pre-decisions to resolve anti-AI checks without guessing.

### Step 4: Anti-AI Detection (Tier 1)
Run the standard Tier 1 hard rules from the Voice Emulation Protocol:
- Em-dash density check (max 1 per 500 words)
- Contrastive formula scan ("It's not X, it's Y")
- Nuclear phrase kill list
- Copula avoidance ("serves as" → "is")
- Sycophantic filler removal

### Step 5: Anti-AI Detection (Tier 2)
Apply the voice file's pre-resolved Tier 2 decisions. These replace the "check the profile" step from the standard protocol — the voice file has already made these calls.

---

## Upgrade Path Messaging

After completing a task with a generic voice, the agent appends a single upgrade message. Rules:

1. **Post-task only** — deliver the content first, upgrade message after
2. **Single use per session** — show once, never repeat in the same conversation
3. **Factual, not comparative** — describe what custom profiles add, don't disparage generic output
4. **Not pushy** — one short paragraph, no follow-up questions about upgrading

The upgrade blurb is stored in each voice file's `## Upgrade Path` section.
