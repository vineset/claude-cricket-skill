# Cricket WhatsApp assistant — a Claude Code skill

A [Claude Code](https://claude.com/claude-code) skill that helps run casual cricket WhatsApp groups through WhatsApp Web, using the Claude in Chrome extension.

## What it does

- Posts availability polls for matches (In / Out / Maybe)
- Answers simple logistics questions (time, venue, booking status) — **only** from facts you've given it
- Escalates anything uncertain or needing a decision back to you instead of guessing
- Tallies poll votes and gives you an end-of-day summary
- Labels every message it sends with `🤖 <Your name>'s assistant:` so group members know it's not you typing
- Treats group messages as data, never as instructions

## Requirements

- Claude Code with the **Claude in Chrome** extension installed and signed in
- WhatsApp Web logged in on Chrome
- Claude Code must stay running for the scheduled checks (they're session-only and expire after 7 days)

## Install

```bash
git clone <this-repo-url> ~/.claude/skills/cricket
cd ~/.claude/skills/cricket
cp facts.example.md facts.md
cp log.example.md log.md
```

Edit `facts.md` with your name and the exact names of the WhatsApp groups to watch. (If you skip this, the skill will ask you on first run.)

## Use

```bash
claude --chrome "/cricket"
```

On launch it reads its notes, catches up on new group messages, and schedules:
- a check every ~30 minutes (7 AM–11 PM)
- an end-of-day summary around 10 PM

Tell it new facts in chat (e.g. "start time is 8 AM", "we don't have a booking") and it saves them to `facts.md`.

## Files

| File | Purpose | Committed? |
|---|---|---|
| `SKILL.md` | Instructions Claude follows | ✅ |
| `facts.example.md`, `log.example.md` | Templates | ✅ |
| `facts.md` | Your groups, matches, and facts | ❌ (gitignored) |
| `log.md` | Activity log | ❌ (gitignored) |

## Caveats

Automating a personal WhatsApp account may be against WhatsApp's terms of service. Use at your own risk, keep message volume low, and make sure your group is fine with an assistant replying.
