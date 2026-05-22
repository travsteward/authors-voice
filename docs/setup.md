# Setup, Anchor Protocol, Multi-Register

Loaded on first run, when generating a new anchor, or when splitting a corpus by register. Not in context during normal writing sessions.

## Setup Flow

If `voice/anchor.md` doesn't exist or is empty, walk the user through setup:

1. **Get the anchor.** Two paths:
   - **Web tool** — user pastes 300-800 words at openwriter.io/writers-voice, copies the result block back. Parse `- XX% Author Name` rows and write to `voice/anchor.md` (lean: just the blend lines, no sub-bullets).
   - **In-agent (Anchor Protocol)** — if user has corpus on disk, run the protocol below to generate `voice/anchor.md` directly.

2. **Seed the corpus** — ask for 2-5 paragraphs, write each to `voice/corpus/sample-NNN.md` with `added: YYYY-MM-DD` frontmatter.
3. **Run Analysis Protocol** (see `docs/analysis.md`) — populates `stats.md`, `never-rules.md`, `fingerprints.md`, `status.md`.
4. **Optional: curate examples** — ask the user for 3-5 most-representative paragraphs, write to `voice/examples.md`.
5. **Optional: populate coined terms** — ask the user for any coined terms / proper-noun concepts they want preserved verbatim, write to `voice/coined-terms.md` as a bare bullet list.
6. **Report status** — read `voice/status.md` and tell the user their tier + what unlocks next.

## Anchor Protocol (in-agent equivalent of the web tool)

Generates `voice/anchor.md` (lean) and `voice/anchor-analysis.md` (rich).

1. Confirm corpus has ≥300 words. Below 300, route to web tool.
2. Read `voice/stats.md`. If missing, run **Analysis Protocol** first.
3. Read `catalog/anchor-prompt.md` (full stylometry rubric) and `catalog/author-hints.md` (curated training-data authors with prose features).
4. **Set aside conversational context.** Score the corpus on prose mechanics only — never on themes/topics.
5. **Per-sample register analysis.** For each sample, record word count, address mode, register, signature moves. Flag samples >25% volume. Cluster by register; if 2+ distinct registers appear, flag as multi-register corpus.
6. **Score with register-aware feature validation.** Apply the 8 dimensions from `catalog/anchor-prompt.md`. Match against author hints. Assign weights summing to 100. For each cited feature, verify ≥40% sample appearance OR ≥40% volume (if neither, drop the feature; if it was the strongest evidence, drop the author).
7. **Self-criticism pass.** Strip any thematic reasoning. Set `confidence` and `any_thematic_reasoning` flags.
8. **Write `voice/anchor.md`** — JUST the lean `- N% Author` lines. No headers, no sub-bullets.
9. **Write `voice/anchor-analysis.md`** — per-author features, per-sample table, register diversity, self-check, refresh notes. Human-facing only.
10. **If multi-register corpus detected**, recommend a Multi-Register Split (see below).
11. Report blend + confidence + caveats to user.

## Multi-Register Anchors

If the corpus spans multiple registers (e.g., third-person expository AND direct-you instructional), maintain a separate anchor per register: `voice/anchor-<context>.md` (e.g., `anchor-book.md`, `anchor-essay.md`, `anchor-tweets.md`). Same lean format. Each gets a paired `voice/anchor-<context>-analysis.md`.

**Multi-Register Split procedure:**

1. Identify registers from the per-sample analysis.
2. For each register, ask the user for a slug + one-line description.
3. Filter corpus to samples in that register.
4. Run the matcher on the subset (same `catalog/anchor-prompt.md` rubric, same variance checks).
5. Write `voice/anchor-<slug>.md` (lean) + `voice/anchor-<slug>-analysis.md` (rich).

**Apply-time anchor selection:** at write time, if multiple anchor files exist, pick by user's request context (explicit naming wins; project the user is working on wins next; ask if ambiguous; fallback to `voice/anchor.md`).
