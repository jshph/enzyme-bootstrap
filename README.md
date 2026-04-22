# enzyme-bootstrap

Template Obsidian vault that syncs your Gmail and meeting notes into wikilinked markdown, then makes it all searchable with [Enzyme](https://github.com/jshph/enzyme-cli).

Your email threads, Granola transcripts, and people notes become a connected knowledge graph — searchable by concept, not just keyword.

## What you get

```
your-vault/
├── emails/          # one file per thread, newest first
│   ├── 2026-03-28 API refactor timeline.md
│   ├── 2026-04-05 demo follow-up.md
│   └── ...
├── people/          # one note per person, cross-linked
│   ├── Sarah Chen.md
│   ├── Marco Reyes.md
│   └── ...
├── transcripts/     # meeting notes + raw transcript
│   ├── 2026-04-05 demo for Marco.md
│   └── ...
├── CLAUDE.md        # tells Claude how to work with the vault
└── .claude/
    └── commands/
        └── setup.md # /setup skill — connects your email and meetings
```

## Quick start

1. Clone this repo into your Obsidian vaults directory
2. Open it in Claude Code
3. Run `/setup`

That's it. `/setup` walks you through everything:

- **Gmail** — creates a Gmail app password (no Google Cloud project needed), writes a Python sync script using only standard library modules, sets up a cron
- **Granola** — installs granola-cli, writes a transcript sync script, adds to the same cron
- **Enzyme** — installs and indexes the vault for semantic search

No dev experience required. The only manual step is creating a Gmail app password, which takes 30 seconds.

## What the sync produces

### Email threads

Only threads where you replied get synced. One file per thread, all participants listed:

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

----

**me** [[2026-03-28]]

Good call on the gateway. What does the feature freeze
window look like?

----

**[[Sarah Chen]]** [[2026-03-29]]

End of April, assuming we freeze feature work during the
migration window.
```

### Meeting transcripts

Granola-style: AI notes up top, raw transcript below.

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
- The connection-surfacing without manual tagging is the differentiator
- Onboarding needs work — "catalyze" means nothing to new users

---

## Transcript

Me: Let me start with the vault exploration...

Marco Reyes: This is way stronger than last time.
```

## Searching the vault

Once Enzyme is set up:

```bash
enzyme petri                              # what's in here, who matters
enzyme catalyze "fundraising timeline"    # conceptual search
enzyme catalyze "design collaboration"    # finds related threads + transcripts
```

## Privacy

- Everything runs locally. No cloud services, no intermediaries.
- Gmail connects via IMAP over SSL (same as your phone).
- App password is stored in `.env` (gitignored) and can be revoked anytime.
- The app password only grants IMAP read access — it can't change account settings or access other Google services.
