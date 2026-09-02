# Track Record

Keeps an evidence log for each of your direct reports, so their review gets written from what actually happened rather than what you can dredge up the week it's due.

The companion to [main-character-moment](../main-character-moment), which does the same job for you.

## The problem

Everyone on your team is the main character of their own story. You're writing their review from memory.

And memory is a shocking witness. It keeps the last three weeks and the loudest three people, and bins the rest. The one who announces their work in a channel? Remembered. The one who quietly unblocks four people a week, reviews everyone's docs and onboards every new starter? Nothing. None of it ever made an artifact with their name on it.

That gap decides promotions.

## What it does

Reads back through the Slack you already have access to and keeps a private running canvas for each report. Every run appends a dated section with two parts.

**Wins.** Concrete moments with real evidence, written up in STAR format with a link back to the source. The Action line is always about that person specifically, because "we" is worthless six months later and it's the exact word that costs people promotions.

**Watch.** Up to three things you should actually do something about. Every item needs an observation, its evidence, and a suggested action. An observation without an action is gossip with a timestamp.

It hunts hardest for the invisible work. Answering someone's question, unblocking a stuck thread, reviewing a doc, onboarding a new starter. The announced wins find themselves. The glue work is what this is for.

## Tell your team before you run it

Something like: *"I keep a running note of what each of you has been working on so I'm not writing your review from memory. You can see yours any time."*

That sentence is the whole difference between the two things this could be. A log your team knows about is a recognition tool. The same log kept quietly is surveillance, and it'll be read that way the day someone finds out.

The skill is written to be shown. Nothing goes in a canvas that couldn't be said to the person's face.

## The rules it won't break

- **It never fabricates a result.** Thin evidence gets flagged, not inflated. You'll take this into a real conversation about someone's career, so it has to be a log you can trust
- **It never reads DMs between other people**, or private channels you're not in
- **It never measures activity.** Message counts, hours online, response times. Those measure presence rather than contribution, and they'll be wrong about the best person on your team
- **It never infers mood or intent** from how someone writes. It records what's observable and stops
- **It never ranks or compares your reports.** Each record stands on its own. Comparison is what calibration is for, and that needs humans in the room
- **Absence of evidence is a visibility finding, never a performance one.** If someone barely surfaces, that's a prompt to go ask them what they've been working on, and a warning that a review written from this would be unfair to them

## How to use it

**In Claude Code** — drop `SKILL.md` into `.claude/skills/track-record/SKILL.md` in your project or home directory. Needs Slack connected.

**In Slack** — paste the contents in as a Slackbot skill.

**Anywhere else** — it's plain text instructions. Paste it into whichever AI assistant you use, as long as it can read your Slack.

## Cadence

Fortnightly, timed just before your 1:1s, so you can say the win out loud to the person. Which is worth more to them than anything that happens at review time.

Monthly if your team is large or the pace is slower. Once more in full before review season, with the lookback set to the whole period.

Daily is noise with extra steps. At that frequency it stops being a wins log and starts being a monitoring tool.

## Make it yours

The tags (Impact, Collaboration, Leadership, Initiative, Growth, Glue) are a starting point, not gospel. Change them to match how your company actually talks about performance. Change the lookback window. Change the output format so it drops straight into whatever review template you're made to fill in.

---

© 2026 Samantha Raphael. Free to use and adapt, credit appreciated.
