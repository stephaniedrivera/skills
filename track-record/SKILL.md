---
name: track-record
description: Builds and maintains an evidence log for each of the user's direct reports from Slack activity they already have access to. Concrete wins written up in STAR format, the invisible glue work that never gets announced, and a short watch list of things the manager should act on. Keeps one private running canvas per report so performance reviews, promotion cases and recognition get written from evidence instead of memory. Use whenever the user mentions writing a performance review, calibration, promo packet, 1:1 prep, giving feedback, recognising someone on their team, "what has X been working on", or worries they'll forget what a report did. Also use for retrospective asks like "how did my team do this quarter", even if they never name this skill.
---

# Track Record

Everyone on your team is the main character of their own story. You're writing their review from memory.

And memory is a shocking witness. It keeps the last three weeks and the loudest three people, and bins the rest. The one who announces their work in a channel? Remembered. The one who quietly unblocks four people a week, reviews everyone's docs and onboards every new starter? Nothing. None of it ever made an artifact with their name on it.

Then review season arrives and you're reconstructing eight months of somebody's career from a vague memory of last Tuesday. Very normal. Very healthy.

That gap decides promotions. This skill goes and finds the evidence while it still exists.

It works in any role and any organisation. Nothing here assumes a particular team, tool stack, or job title.

## Before the first run

Tell your team. Something like: *"I keep a running note of what each of you has been working on so I'm not writing your review from memory. You can see yours any time."*

That sentence is doing a lot of work. A log your team knows about is a recognition tool. The same log kept quietly is surveillance, and it'll be read that way the day someone finds out. If the user hasn't said it yet, tell them to before the first run. Once, plainly, then drop it.

## What it reads

**Slack, always.** Everything runs through the Slack MCP, and Slack on its own is enough for a full run. Only what the manager already has access to. Go looking for the fingerprints each report left behind:

- Their messages, and the threads they were central to
- Praise and reactions from other people. Someone saying "this saved us" is the strongest evidence there is, and it's the kind a report never logs about themselves
- **Huddle notes they're tagged in.** Gold. Huddles are where the decisions actually happen and the notes record who committed to what
- Canvases, docs and files they created and shared
- **The glue work.** Hunt for this on purpose: answering someone else's question, reviewing a doc, unblocking a stuck thread, onboarding a new starter, catching a problem early. It never gets announced, it never shows up in a status update, and it's usually the difference between a good report and an excellent one. This is the whole reason the skill exists

**Calendar and documents, if they're connected.** Events with real notes attached, and docs the report created and shared. Both are optional garnish. If either isn't connected, or a lookup throws an error, skip it, say so in the footer, and carry on. A missing connector never blocks the run.

### What it never reads

Hard limits, not preferences.

- DMs between other people. Ever. Not even if the manager technically has export access
- Private channels the manager isn't a member of
- Anything the manager doesn't already have access to in the normal course of their job

### What it never measures

- Message counts, hours active, response times, or any activity metric standing in for contribution. A quiet week isn't a bad week
- Tone, mood, sentiment or engagement, inferred from how someone writes
- Anything producing a ranking or a comparison between reports

Asked to do any of this, decline and say why. It measures presence rather than contribution, and it'll be wrong about the best person on the team.

## Steps

### Phase 1 — Scope

This file is the skill **definition**. Instructions only. Never write logged evidence into it.

Output lives in **one private canvas per direct report**, titled **Track Record — [Name]**. One canvas per person. Never a single shared team document, because mixing several people's records into one file is both a mess and a leak waiting to happen.

On the first run the canvases won't exist. Ask for the reports' names, create one canvas each with the Slack MCP's canvas creation tool (`slack_create_canvas`), and keep the file_id or link for each so every future run appends to the same place.

Lookback window:

- Default to everything since the last dated section in that report's canvas
- If the log is brand new, default to the last 4 weeks
- Only ask if the user wants a different window, or wants to narrow to one project or person. Otherwise just go

### Phase 2 — Gather

Bulk searches, capped, per report. Enough signal to write honestly, not a forensic audit of somebody's working life.

- **Slack search.** Around six per report across distinct angles: shipped work, praise and reactions, problem-solving, leadership and initiative, cross-team help, and one pass dedicated to the glue work
- **Huddle notes** they're tagged in, since those carry decisions that never made it into a channel
- **Calendar.** One pass over the window. Pull the text from events that have notes or a description. Skip the empty invites
- **Documents and canvases.** What they created and shared in the window, top five or so
- If a source isn't available, skip it and say so in the footer

### Phase 3 — Synthesize

Two sections per report. Both linked to evidence, and neither of them invented.

**Wins, 1 to 5 per run.** Moments with something real behind them, not a shapeless fog of activity.

"Was across the migration" isn't a win. "Traced the migration failure to an expired service token and had it moving again inside an hour" is a win. If it wouldn't survive being read aloud to the report themselves, it isn't one.

Write each in STAR format:

- **Situation.** The context or the problem
- **Task.** What needed to happen, and why it landed on them
- **Action.** What *this person* did. Not the team. "We" is worthless six months from now, and it's the exact word that costs people promotions
- **Result.** The measurable or observable outcome

Tag each loosely with one or more of: Impact, Collaboration, Leadership, Initiative, Growth, Glue.

Link the source every time. A claim with a link on it is worth ten without.

**Watch, 0 to 3 per run.** Things the manager should actually do something about. This section has rules and they're strict.

- Every item states an **observation**, its **evidence**, and a **suggested manager action**. All three, every time. An observation without an action is gossip with a timestamp
- Frame it as a situation, never a verdict on the person. "Has raised the same blocker in three separate threads with no resolution", not "seems frustrated". "Took on the deploy rota on top of their project work for five weeks", not "at risk of burnout"
- **Never infer an internal state.** Not mood, motivation, commitment or intent. Write what's observable and stop
- **Absence of evidence is a visibility finding, never a performance finding.** If a report barely surfaces, the honest line is *"Very little of their work is visible in the channels I have access to. Worth asking what they've been working on, and worth fixing, because a review written from this would be unfair to them."* It's never *"low output"*. The quietest person on a team is frequently the one holding it together
- If nothing genuine surfaces, write nothing. An empty Watch list is a perfectly good result and the default expectation on most runs

**Never rank, score, rate or compare reports against each other.** Not in the canvas, not in the chat summary, not if asked directly. Each person's record stands on its own. Comparison is what calibration is for, and it needs a human in the room.

### Phase 4 — Output

Append a new dated section to each report's canvas. Newest on top, directly under the title. Never overwrite what's already there. Never write output into this skill definition.

Keep the chat summary short: who was logged, how many wins each, and flag any Watch items so they don't get missed. Don't reproduce the whole canvas back into the conversation.

## Output format

```
## [Month Day, Year]

Sources checked: [Slack / huddle notes / calendar / shared documents, and anything skipped]

### Wins

#### [Short punchy title] — [Tag(s)]
- Situation: [context/problem]
- Task: [what needed to happen, and why them]
- Action: [what this person did]
- Result: [measurable/observable outcome]
- Source: [link to thread, huddle note, meeting, or doc]

[repeat per win]

### Watch

- **[Observation, stated as a situation]**
  - Evidence: [link and one line of context]
  - Suggested action: [what the manager could do about it]

[omit this heading entirely if there's nothing genuine to put in it]

---
[Older entries remain below, oldest at the bottom]
```

If nothing solid surfaces for someone, say so plainly rather than manufacturing a moment:

> Nothing concrete surfaced for [Name] this period. That's a visibility signal, not a performance one. Worth asking them directly what they've been working on.

## Key principles

- **Written from evidence, not memory.** The entire point. If it isn't linked, it doesn't go in
- **Never fabricate a result.** If the outcome isn't measurable or observable from what's there, say so plainly instead of inventing a number. A log the manager can't trust is worse than no log at all, because they'll take it into a real conversation about someone's career
- **Their work, not the team's.** The Action line is about this person. Collective language is the fastest way to make a win useless at review time
- **Hunt the invisible work hardest.** The announced wins will find themselves
- **Watch items get an action or they don't get written**
- **Absence of evidence is about visibility, never value**
- **No rankings, no scores, no comparisons between people**
- **Append-only.** New section on top every run. Never delete or rewrite what came before
- **Your team knows it exists.** See the top of this file. It's the whole difference between a recognition tool and a monitoring one
- **Written to be shown.** Assume every report reads their own canvas one day, because they should be able to. Nothing goes in that couldn't be said to their face

## Suggested cadence

Fortnightly, timed just before 1:1s, so the wins are still warm enough to say out loud. Which is worth more to the person than anything that happens at review time.

Monthly works if the team is large or the pace is slower. Run it once more in full before review or calibration season, with the lookback set to the whole period.

Daily is noise with extra steps. At that frequency it stops being a wins log and starts being a monitoring tool.
