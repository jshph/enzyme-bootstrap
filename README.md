# enzyme-bootstrap

If you already have a structured vault — people notes, meeting logs, backlinks — you know the maintenance problem. Every new thread or meeting means updating summaries, adding cross-references, keeping the graph current. You either do it manually or let it rot.

Enzyme keeps that map for you. It understands PKM structure — wikilinks, frontmatter, entity relationships — so your AI doesn't have to reconstruct context by reading every file. Fewer tokens burned on catch-up, and answers that draw connections across your vault instead of just searching it.

This repo gives your vault the pieces Enzyme needs: agent instructions that respect your existing structure, and pipelines to bring in sources like email if you want them.

## Try it

```bash
# copy the agent instructions into your vault
cp -r enzyme-bootstrap/.claude your-vault/
cp enzyme-bootstrap/CLAUDE.md your-vault/

# install enzyme
curl -fsSL enzyme.garden/install.sh | bash
cd your-vault && enzyme init && enzyme refresh

# ask
claude
```

```
catch me up on what I've been thinking about recently
```

```
what's my history with Sarah?
```

```
what threads have gone quiet that I should follow up on?
```

## Starting from scratch?

```bash
git clone https://github.com/jshph/enzyme-bootstrap.git
cd enzyme-bootstrap && claude
/setup
```

`/setup` connects Gmail and Granola, writes sync scripts, and schedules a cron. The sample data in this repo shows what the output looks like.

## Privacy

Everything runs locally. No cloud services, no intermediaries. Gmail connects via IMAP over SSL; app password stored in `.env` (gitignored), revocable anytime.
