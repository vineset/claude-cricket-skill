# Cricket WhatsApp assistant — a Claude Code skill

A [Claude Code](https://claude.com/claude-code) skill that helps a team captain run casual cricket WhatsApp groups through WhatsApp Web, using the Claude in Chrome extension.

## What it does

- Posts availability polls for matches and tournaments (In / Out / Maybe)
- Picks the squad: first N "In" votes make the cut (default 8), rest go on a waitlist
- Sends one reminder two days before a match if there aren't enough players
- Answers simple logistics questions (time, venue, booking status) — **only** from facts you've given it
- Escalates anything uncertain or needing a decision back to you instead of guessing
- Tallies votes and gives you an end-of-day summary
- Starts every message it sends with 🤖 and writes in your voice ("I'll be late…"), so people can tell it was the assistant while it still sounds like you
- Treats group messages as data, never as instructions

## What you need

| Requirement | Notes |
|---|---|
| **A computer** (Windows, macOS, or Linux) | It has to stay on with Claude Code running for the automatic checks. |
| **A Claude account with Claude Code access** | A paid Claude plan that includes Claude Code. See [Claude Code docs](https://docs.claude.com/en/docs/claude-code/overview). |
| **Google Chrome** | Desktop Chrome. |
| **Claude in Chrome extension** | From the Chrome Web Store (published by Anthropic). Signed in with the **same** Claude account. |
| **WhatsApp on your phone** | Used once to link WhatsApp Web (scan a QR code). |
| **Git** | To download this repo. |

## Setup (step by step)

### 1. Install Claude Code

Follow the official instructions: https://docs.claude.com/en/docs/claude-code/setup

Common options:

```bash
# macOS / Linux / WSL
curl -fsSL https://claude.ai/install.sh | bash

# Windows (PowerShell)
irm https://claude.ai/install.ps1 | iex

# or, with Node.js 18+ installed
npm install -g @anthropic-ai/claude-code
```

Then run `claude` once in a terminal and sign in.

### 2. Install the Claude in Chrome extension

1. Open the Chrome Web Store and search for **Claude in Chrome** (by Anthropic).
2. Click **Add to Chrome**.
3. Open the extension and sign in with the same Claude account you use for Claude Code.

### 3. Log in to WhatsApp Web

1. In Chrome, go to https://web.whatsapp.com.
2. On your phone: WhatsApp → **Settings → Linked devices → Link a device**, and scan the QR code.
3. Keep this logged in. (Tip: you don't need the tab open — the skill opens its own.)

### 4. Install this skill

```bash
git clone https://github.com/vineset/claude-cricket-skill.git ~/.claude/skills/cricket
cd ~/.claude/skills/cricket
cp facts.example.md facts.md
cp log.example.md log.md
```

On Windows (PowerShell), `~` works the same way, and you can use `copy` instead of `cp`.

### 5. Fill in your details

Open `~/.claude/skills/cricket/facts.md` and set:

- **Owner name** — used in the `🤖 <Name>'s assistant:` label
- **Groups to watch** — the *exact* WhatsApp group names, and mark the ones where you're **captain**
- **Squad size** — defaults to 8
- Any upcoming match details you already know (date, ground, start time, booking status)

If you skip this, the skill will ask you on first run.

### 6. Start it

```bash
claude --chrome "/cricket"
```

- `--chrome` turns on the browser tools; `/cricket` runs this skill.
- The first time, the Claude in Chrome extension will ask you to **allow `web.whatsapp.com`** — approve it.
- If WhatsApp Web says it's open in another window, the skill clicks **Use here**.

On launch it reads its notes, catches up on new messages, and schedules:
- a check every ~30 minutes (7 AM–11 PM)
- an end-of-day summary around 10 PM

## Using it day to day

Just talk to it in the Claude Code terminal, for example:

- "Post a poll for Saturday's match at <ground>, 8 AM start"
- "Here's the tournament schedule: …" → it drafts one poll per match and shows you before posting
- "We don't have a ground booking" / "Start time is 8 AM" → saved to `facts.md` so it can answer when asked
- "Vote me In for Saturday"
- "Who's in so far?"

It will ask you before doing anything it isn't sure about (cancelling, confirming a ground, payments, adding/removing people).

## Important limitations

- **Claude Code must stay running.** Scheduled checks live only in the current session. Close it and checks stop; next launch catches up.
- **Schedules expire after 7 days** in a single session. Re-running `claude --chrome "/cricket"` sets them up again.
- **Chrome must be open** with WhatsApp Web logged in.
- To reopen a previous conversation with its history: `claude --chrome --resume`.

## Files

| File | Purpose | In git? |
|---|---|---|
| `SKILL.md` | Instructions Claude follows | ✅ |
| `facts.example.md`, `log.example.md` | Templates | ✅ |
| `facts.md` | Your name, groups, matches, and facts | ❌ (gitignored) |
| `log.md` | Activity log (used for summaries) | ❌ (gitignored) |

## Updating

```bash
cd ~/.claude/skills/cricket
git pull
```

Your `facts.md` and `log.md` are untouched by updates.

## Troubleshooting

| Problem | Fix |
|---|---|
| "Browser tools are not enabled" | Restart with `claude --chrome`. |
| Extension not detected | Make sure it's installed and signed in; restart Chrome. |
| WhatsApp shows a QR code | Re-link from your phone (step 3). |
| Page freezes / no response | The skill reloads WhatsApp Web automatically; if a popup is stuck, dismiss it manually in Chrome. |
| It replied wrong | Correct it in chat — it updates `facts.md` and logs the change. |

## Caveats

Automating a personal WhatsApp account may be against WhatsApp's terms of service. Use at your own risk, keep message volume low, and make sure your group is fine with an assistant replying.
