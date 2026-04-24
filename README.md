# enzyme-bootstrap

A template for turning your Obsidian vault into something you can search by concept, not just keyword. Clone it, run `/setup`, and it bootstraps sources like email and meeting notes — without touching the structure you already have.

## What this is (and isn't)

This is **not** an opinionated vault structure. If you already have folders, notes, daily pages, or your own conventions — they stay. This template adds:

1. **Source pipelines** — scripts that sync Gmail threads and Granola meeting transcripts into your vault as markdown, if you don't already have them
2. **Entity extraction** — Claude writes scripts to scan your notes for people mentions and extract them into `people/` notes with backlinks
3. **Semantic search** — [Enzyme](https://enzyme.garden) indexes everything for conceptual queries ("what's my history with Sarah", "conversations about funding")

The sample data in this repo shows what the output looks like. `/setup` clears it and connects your real sources.

## Quick start

```bash
git clone https://github.com/jshph/enzyme-bootstrap.git
cd enzyme-bootstrap
claude          # open Claude Code
/setup          # connect your email and meetings
```

Already have a vault? Clone this into a subfolder or copy the `.claude/` directory and `CLAUDE.md` into your vault root. The pipelines write to `emails/` and `transcripts/` — if those folders already exist with your own content, the sync scripts won't overwrite anything.

## Enzyme constructs

Enzyme uses a few optional frontmatter fields when they're present. These aren't required — Enzyme works without them — but they make the wikilink graph and entity relationships richer:

- **`people:`** — list of `"[[Name]]"` wikilinks. Enzyme uses this to build a people graph across notes.
- **`date:`** — a `"[[yyyy-mm-dd]]"` wikilink. Links the note into the vault's date graph.
- **`created:`** — ISO date string. Used by Enzyme for recency ranking.

```yaml
---
people:
  - "[[Sarah Chen]]"
  - "[[Priya Kapoor]]"
date: "[[2026-03-28]]"
created: 2026-03-28
---
```

You can add these to any note — not just synced emails. If you have existing notes with people mentioned in the body but not in frontmatter, Claude can write a script to extract them.

## What the pipelines produce

### Email threads (`emails/`)

One file per thread. Only threads where you replied get synced (filters out newsletters and noise). Messages in chronological order, separated by `----`.

### Meeting transcripts (`transcripts/`)

One file per meeting. AI notes section first (decisions, action items), then raw transcript with named speakers.

### People notes (`people/`)

Stubs auto-created for anyone who appears in 3+ threads. Start as minimal frontmatter — you flesh them out over time with context about who they are and how you know them.

## Searching

```bash
enzyme petri                              # who matters, what themes are trending
enzyme catalyze "fundraising timeline"    # conceptual search across everything
enzyme catalyze "design collaboration"    # finds threads + transcripts + people
```

## Privacy

Everything runs locally. No cloud services, no intermediaries.

- Gmail connects via IMAP over SSL (same protocol as your phone)
- App password stored in `.env` (gitignored) — revoke anytime at [myaccount.google.com/apppasswords](https://myaccount.google.com/apppasswords)
- App passwords only grant IMAP read access — they can't change account settings or access other Google services
