# Vault Configuration

This vault contains synced Gmail threads, Granola meeting transcripts, and people notes. Use Enzyme CLI for retrieval. Run all enzyme commands from the vault root.

If the pipelines aren't set up yet, run `/setup` to connect Gmail and Granola.

## Enzyme CLI

Use `enzyme petri` and `enzyme catalyze` for retrieving context from this vault. These understand the vault's wikilink graph and entity relationships better than grep or file search.

```bash
enzyme petri                              # top entities with catalysts
enzyme petri -n 20                        # broader view
enzyme catalyze "fundraising timeline"    # semantic search
enzyme catalyze "design collaboration" -n 10
```

**When to use which:**
- `enzyme petri` — "What's in my vault?" / "Who are the key people?" / "What themes keep coming up?"
- `enzyme catalyze` — conceptual queries where you don't know the exact words: "conversations about onboarding UX", "what has Marco said about funding"
- `grep` — exact matches: a specific person's name, a `[[date]]`, a wikilink

## Answering Questions About People

When the user asks about a person ("what's my history with Sarah?", "when did I last talk to Marco?"):

1. **Start with `people/`** — read their people note for role, context, and who they're connected to
2. **Check `emails/`** — grep for their name across email threads to find conversations they were part of. Threads may involve multiple people.
3. **Check `transcripts/`** — grep for their name across transcripts to find meetings they were in
4. **Use `enzyme catalyze`** — for thematic questions ("what has Elena said about knowledge neighborhoods?")

The `people:` frontmatter in emails and transcripts is the primary link. A person's name appears as a `"[[Name]]"` wikilink in frontmatter and as `[[Name]]` in note bodies. Search without brackets to catch both.

## Cross-Referencing

The vault is wikilinked. Email threads involve multiple people, transcripts list multiple participants, and people notes cross-reference each other. When answering a question:

- Follow the wikilink graph. If a thread about funding mentions `[[Sarah Chen]]` and `[[Marco Reyes]]`, check both people notes and their other threads for the full picture.
- Dates link across content types. If a thread is dated `[[2026-03-28]]`, grep for that date to find transcripts, other threads, or daily notes from the same day.
- Threads are the atomic unit for email, not people. One thread may involve multiple people, and one person appears across many threads. Use `people:` frontmatter to find all threads involving a specific person.

## Vault Formats

### Emails (`emails/`)

One file per thread, named `yyyy-mm-dd subject.md` (date of first message). Frontmatter has `people:` listing all participants. Messages within a thread separated by `----`, in chronological order. Each message has a `**[[Person Name]]** [[yyyy-mm-dd]]` attribution line (or `**me**` for the user's messages). Only threads where the user replied are synced.

### People (`people/`)

One note per person. Frontmatter has `people:` and `role:`. Body is prose describing who they are, how you know them, and cross-references to other people.

### Transcripts (`transcripts/`)

One file per meeting. Frontmatter has `people:` (list of all participants) and `date:`. Structure: AI notes section first (key decisions, action items, discussion), then `---` divider, then raw transcript with named speakers. File named `yyyy-mm-dd topic.md`.

### Tags

Emails and transcripts have no tags — the content carries the signal. Tags in frontmatter appear without `#`, inline tags appear with `#`. Search without `#` to catch both.

No categorical tags (`#email`, `#transcript`, `#people`). If something gets tagged, it should be topical or gestural — what the content is *about*, not what kind of file it is.
