# enzyme-bootstrap

This is a template vault. It doesn't impose structure — it bootstraps sources (email, transcripts) and enriches whatever's already here. Run all enzyme commands from the vault root.

If the pipelines aren't set up yet, run `/setup` to connect Gmail and Granola.

## Enzyme CLI

Use `enzyme petri` and `enzyme catalyze` for retrieval. These understand the vault's wikilink graph and entity relationships better than grep or file search.

```bash
enzyme petri                              # top entities with catalysts
enzyme petri -n 20                        # broader view
enzyme catalyze "fundraising timeline"    # semantic search
enzyme catalyze "design collaboration" -n 10
```

**When to use `catalyze` vs `grep`:**

The test: would these exact words appear in the vault?

**Use `grep` when you have a concrete anchor** — something that exists verbatim:
- People: "Sarah", `[[Dr. Chen]]`
- Tags: `#productivity`, `#enzyme/pmf`
- Links/titles: `[[On Writing Well]]`, `[[meeting notes]]`
- Dates: `[[2026-03-28]]`
- Proper nouns: places, companies, projects

**Use `catalyze` when you only have a theme or concept** — no anchor to grep:
- "What have I written about feeling stuck?" (no name, no tag, no title)
- "cost of care in algorithmic interfaces" (academic framing — vault won't use these words)
- "tension between efficiency and presence" (conceptual, not anchored)

Names and tags always appear verbatim. Abstract or academic language rarely does — vaults use personal, concrete phrasing.

**Use `petri`** for orientation — "What's in my vault?" / "Who are the key people?" / "What themes keep coming up?"

## Enzyme frontmatter constructs

Enzyme uses these optional frontmatter fields when present. They're not required — Enzyme works without them — but they strengthen the wikilink graph:

- **`people:`** — list of `"[[Name]]"` wikilinks. Builds the people graph.
- **`created:`** — a `"[[yyyy-mm-dd]]"` wikilink. Links into the date graph.

These can appear on any note, not just synced content. If the user has existing notes with people mentioned in body text but not in frontmatter (i.e. in emails or transcripts), offer to write a script that extracts them.

## Extracting entities

A key job of this template is noticing people (and other entities) mentioned in free text and promoting them to first-class vault objects. This happens in two ways:

1. **During sync** — the email and transcript pipelines auto-create `people/<Name>.md` stubs.

2. **On demand** — if the user has existing notes with names mentioned in body text, write a script to:
   - Scan notes for names that look like people (capitalized multi-word phrases, email attribution lines, transcript speaker labels)
   - Cross-reference against existing `people/` notes
   - Create stubs for new people with a `people:` frontmatter field
   - Optionally backfill `people:` frontmatter into the source notes

The goal is structured backlinks without manual tagging. The user's existing folder structure and naming conventions stay untouched — entity extraction adds to them, never reorganizes.

## Retrieval patterns

**Questions about a specific person** ("what's my history with Sarah?") — the name is a concrete anchor, so start with grep:

1. Check `people/` for their note (role, context, connections)
2. Grep for their name across the vault (search without brackets to catch both frontmatter `"[[Name]]"` and body `[[Name]]`)
3. Follow wikilinks from the results to related people and threads

**Thematic questions involving a person** ("what has Elena said about knowledge neighborhoods?") — the theme is the hard part, so use catalyze for the concept and grep to filter by person.

**Stale threads / follow-ups** ("what threads have gone quiet?") — grep for the user's threads, check the `created:` date or last message date, surface ones with no recent activity. Cross-reference with people notes to add context on why a follow-up might matter.

**Date-based cross-referencing** — dates are concrete anchors. Grep for a date to find notes, threads, or meetings from the same day.

## Vault formats (synced content)

These formats are what the sync pipelines produce. They don't constrain what the user's other notes look like.

### Emails (`emails/`)

One file per thread, named `yyyy-mm-dd subject.md`. Frontmatter has `created:` (date of first message) and `people:` listing all participants. Messages separated by `----`, chronological. Attribution: `**[[Person Name]]** [[yyyy-mm-dd]]` or `**me**` for the user.

### Transcripts (`transcripts/`)

One file per meeting, named `yyyy-mm-dd topic.md`. Frontmatter has `created:`, `people:`, and `date:`. AI notes first, then `---` divider, then raw transcript with named speakers.

### People (`people/`)

One note per person. Auto-created stubs have `people:` and `role:` frontmatter. The user fills in context over time.

### Tags

No categorical tags (`#email`, `#transcript`, `#people`). Tags should be topical — what content is *about*, not what kind of file it is.
