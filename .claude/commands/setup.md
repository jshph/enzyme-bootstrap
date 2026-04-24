# /setup — Connect your email and meetings to your vault

You are setting up automated pipelines that turn the user's Gmail and Granola meetings into searchable, wikilinked markdown in this Obsidian vault. Walk the user through each step interactively. Do the work — don't dump instructions.

## Step 0: Check what's installed

Run these checks silently:

```bash
which brew && echo "brew: yes" || echo "brew: no"
python3 --version 2>&1 || echo "python3: no"
which node && echo "node: yes" || echo "node: no"
which npm && echo "npm: yes" || echo "npm: no"
which enzyme && echo "enzyme: yes" || echo "enzyme: no"
```

Python 3 is the only hard requirement for email sync — it's pre-installed on every Mac. If somehow missing, install via `brew install python` or direct download from python.org.

Node/npm is only needed if the user wants Granola transcript sync.

## Step 1: Clear sample content

This vault ships with sample data to show the format. Before syncing real content, remove the sample files:

```bash
rm -f emails/*.md
rm -f people/*.md
rm -f transcripts/*.md
```

Tell the user: "I'm clearing the sample data so your real email and meetings can take its place. The folder structure and formats stay the same."

## Step 2: Gmail app password

The user needs a Gmail app password. This is the only manual step — no Google Cloud project, no OAuth credentials, no API setup.

Tell the user:

> To sync your email, I'll connect directly to Gmail over IMAP (the same protocol your phone uses to check mail). A few things worth knowing:
> 
> **What stays where:**
> - Everything runs locally on your machine. Emails go from Gmail → your computer → markdown files in this vault. No cloud services, no intermediaries, no data leaves your machine.
> - Your app password gets saved to a `.env` file in this vault folder, which is gitignored — it won't end up in version control.
> - The connection to Gmail uses SSL (the same encryption as the Gmail app on your phone).
> 
> **About the app password:**
> - An app password is a separate 16-character password that only works for IMAP access — it can't change your Google account settings, access your Drive, or do anything besides read email.
> - You can revoke it anytime at myaccount.google.com/apppasswords and the sync just stops. Nothing else is affected.
> 
> **To create one:**
> 1. Go to https://myaccount.google.com/security
> 2. Make sure 2-Step Verification is on (enable it first if it's not)
> 3. Go to https://myaccount.google.com/apppasswords
> 4. Create an app password — name it whatever you want (e.g. "vault sync")
> 5. Copy the 16-character password it gives you
> 
> Paste it here when you're ready.

**Stop and wait for the user to provide the app password.** Do not proceed until they give it to you. If they have questions or hesitations, answer them honestly — don't rush past this step.

Once they provide it, also ask for their Gmail address if you don't already know it.

## Step 3: Write the email sync script

Create `scripts/sync-email.py` — a single Python script using only standard library modules (`imaplib`, `email`, `os`, `re`, `datetime`, `json`). No pip installs needed.

The script should:

### Connect and fetch
1. Connect to `imap.gmail.com:993` via SSL using the user's email + app password (read from `.env`)
2. Select `[Gmail]/All Mail`
3. On first run, fetch the last 6 months of email using IMAP's `SINCE` date filter
4. On subsequent runs, fetch only emails since the last sync date. Track the last-sync date in `.sync-state` (a single ISO date string)
5. Use Gmail's `X-GM-THRID` extension to get thread IDs for each message
6. Batch fetch messages (IMAP supports multi-UID fetch) — process recent threads first by sorting on the most recent message date per thread

### Filter to real conversations
7. Group messages by thread ID (`X-GM-THRID`)
8. **Only keep threads where the user replied** — at least one message in the thread has the user's email address in the `From` header. This filters out newsletters, notifications, and one-way inbound mail.
9. Parse each message's sender name, date, subject, and plain-text body using the `email` module
10. Strip email signatures (lines after `--`, common signature patterns like "Sent from my iPhone")
11. Strip quoted reply chains (`>` prefixed lines, `On ... wrote:` blocks, forwarded headers)

### Write threads to markdown
12. For each thread, write `emails/<yyyy-mm-dd> <subject>.md` where the date is the first message in the thread:

```markdown
---
created: 2026-03-28
people:
  - "[[Person A]]"
  - "[[Person B]]"
---

# Subject line

**[[Person A]]** [[2026-03-28]]

First paragraph of the email body.

Second paragraph, preserving natural line breaks between paragraphs.

----

**me** [[2026-03-28]]

The user's reply. Multi-paragraph body flows naturally.

Second paragraph of the reply.

----

**[[Person A]]** [[2026-03-29]]

Next message in the thread.
```

**Format rules:**
- `created:` frontmatter is the date of the first message in the thread (ISO format, no wikilink)
- `people:` frontmatter lists everyone in the thread (except the user) as `"[[Name]]"` wikilinks
- Messages within a thread in chronological order (oldest first)
- `----` separates messages
- `**[[Person Name]]** [[yyyy-mm-dd]]` attribution line for each message
- `**me**` (lowercase, no wikilink) for the user's messages
- Multi-paragraph bodies preserved with blank lines between paragraphs
- Cross-reference: if a name in the body matches an existing file in `people/`, wikilink it as `[[Name]]`

13. Create a `people/<Name>.md` stub for anyone who appears in 3+ threads and doesn't have a people note yet:

```markdown
---
people: "[[Person Name]]"
role: ""
---

# Person Name
```

### Credentials handling
Store the email and app password in `.env` at the vault root (gitignored). The script reads from there. Create the `.env` file and add `.env` to `.gitignore`.

```
GMAIL_ADDRESS=user@gmail.com
GMAIL_APP_PASSWORD=xxxx xxxx xxxx xxxx
```

## Step 4: Test the sync

Run the script and show the user what it produced:

```bash
python3 scripts/sync-email.py
```

Then `ls emails/` to show them what landed. Read one of the email files to verify formatting looks right. Ask if they want to adjust anything (different time window, specific senders to exclude, etc.).

## Step 5: Transcript pipeline (Granola)

**Ask:** "Do you use Granola for meeting notes? If not, I'll skip this."

If yes, and npm is available:

### Install granola-cli

```bash
npm install -g granola-cli
```

If npm isn't available and brew is:
```bash
brew install node && npm install -g granola-cli
```

If neither: tell the user to install Node.js from https://nodejs.org (LTS download, standard installer, no terminal needed).

### Authenticate

```
! granola auth
```

**Stop and wait for confirmation.**

### Write the transcript sync script

Create `scripts/sync-transcripts.py` that:

1. Shells out to `granola meeting list --format json` to get recent meetings
2. For each new meeting not already in `transcripts/`, exports via `granola meeting export <id> --format json`
3. Extracts: title, date, attendee names, AI notes (notes_markdown), transcript
4. Writes `transcripts/<yyyy-mm-dd> <title>.md`:

```markdown
---
people:
  - "[[Person A]]"
  - "[[Person B]]"
date: "[[2026-03-28]]"
---

# Meeting title

Recorded Mar 28, 2026
Participants: [[Person A]], [[Person B]], me

## Notes

### Key decisions
- Decision one

### Action items
- Person does thing

### Discussion notes
Summary prose.

---

## Transcript

Person A: What they said.

Me: What I said.
```

5. Creates `people/<Name>.md` stubs for new attendees

## Step 6: Enzyme

### Install

```bash
curl -fsSL enzyme.garden/install.sh | bash
```

This downloads the binary, the embedding model, and auto-installs the Claude Code plugin. No brew, npm, or pip needed.

### Initialize

```bash
enzyme init
enzyme refresh
```

## Step 7: Schedule it

Use CronCreate to set up a single cron that runs every 6 hours:

```bash
cd /path/to/vault && python3 scripts/sync-email.py && python3 scripts/sync-transcripts.py && enzyme refresh
```

(Skip the transcript script if Granola wasn't set up.)

## After setup

Tell the user:
- "Your email syncs every 6 hours. [Meeting notes too, if Granola was set up.]"
- "Run `enzyme catalyze 'some concept'` to search across everything."
- "New people auto-appear in `people/` as they show up in your email threads."

Run `enzyme petri` and show them what their vault looks like.
