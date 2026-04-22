# enzyme-bootstrap

Your email and meeting notes, wikilinked and searchable by concept. Clone, run `/setup`, done.

## Why this exists

Most of what you know about the people you work with lives in email threads and call transcripts. But email is a black hole — you can search by keyword, not by concept. You can't ask "what's my history with Sarah" or "who introduced me to Marco" and get a real answer.

This vault fixes that. It syncs your Gmail threads and Granola meeting notes into Obsidian as wikilinked markdown, then indexes everything with [Enzyme](https://github.com/jshph/enzyme-cli) for semantic search. People, conversations, and ideas become a connected graph you can actually think with.

## Quick start

```bash
git clone https://github.com/jshph/enzyme-bootstrap.git
cd enzyme-bootstrap
claude          # open Claude Code
/setup          # connect your email and meetings
```

`/setup` detects what's on your machine, installs what's missing, and walks you through everything interactively. The only manual step is creating a Gmail app password (30 seconds, no Google Cloud project). No dev experience required.

## What it does

**Sync** — Gmail threads and Granola transcripts sync to markdown every 6 hours via cron. Only threads where you replied get synced (filters out newsletters and notifications).

**Link** — People are wikilinked across emails, transcripts, and people notes. `[[Sarah Chen]]` in an email thread connects to her people note and every other conversation she appears in.

**Search** — Enzyme indexes the full vault for semantic search. Ask "fundraising timeline" and it finds relevant threads, transcripts, and people notes even if those exact words don't appear.

## What the vault looks like

```
emails/                                     # one file per thread
├── 2026-03-28 API refactor timeline.md
├── 2026-04-05 demo follow-up.md
├── 2026-04-08 newsletter draft review.md

people/                                     # one note per person
├── Sarah Chen.md
├── Marco Reyes.md

transcripts/                                # Granola meeting exports
├── 2026-04-05 demo for Marco.md
├── 2026-03-28 call with Sarah about API refactor.md
```

### Email threads

```markdown
---
people:
  - "[[Sarah Chen]]"
  - "[[Priya Kapoor]]"
---

# API refactor timeline

**[[Sarah Chen]]** [[2026-03-28]]

Hey — quick update on the API refactor. We ended up going
with the gateway pattern instead of the mesh approach.
Fewer moving parts, and Marco mentioned Foundry's portfolio
companies have had better luck with gateways at our scale.

----

**me** [[2026-03-28]]

Good call on the gateway. What does the feature freeze
window look like?

----

**[[Priya Kapoor]]** [[2026-04-01]]

Sarah — jumping in here. First pass on the developer portal
wireframes is done. Main insight: people don't want a
dashboard, they want a feed.
```

### Meeting transcripts

```markdown
---
people:
  - "[[Marco Reyes]]"
date: "[[2026-04-05]]"
---

# Demo for Marco

Recorded Apr 5, 2026

## Notes

### Key takeaways
- Connection-surfacing without manual tagging is the differentiator
- Onboarding needs work — "catalyze" means nothing to new users

---

## Transcript

Me: Let me start with the vault exploration...

Marco Reyes: This is way stronger than last time. The way it
surfaces connections without requiring manual tagging — that's
the thing that'll sell.
```

## Searching

```bash
enzyme petri                              # who matters, what themes are trending
enzyme catalyze "fundraising timeline"    # conceptual search across everything
enzyme catalyze "design collaboration"    # finds threads + transcripts + people
```

## How it works

The setup is three pieces:

1. **Gmail → markdown** — A Python script (stdlib only, no pip installs) connects to Gmail via IMAP with an app password, groups messages by thread, and writes one markdown file per thread. Runs on a cron every 6 hours.

2. **Granola → markdown** — A script pulls meeting data via granola-cli, extracts AI notes and raw transcript, writes one file per meeting with participants in frontmatter. Same cron.

3. **Enzyme** — Indexes the vault's wikilink graph and content for semantic search. Re-indexes on every sync.

## Privacy

Everything runs locally on your machine. No cloud services, no intermediaries.

- Gmail connects via IMAP over SSL (same protocol as your phone)
- App password is stored in `.env` (gitignored) — revoke it anytime at [myaccount.google.com/apppasswords](https://myaccount.google.com/apppasswords)
- App passwords only grant IMAP read access — they can't change account settings or access other Google services
