# Brief format

Two variants: **DM** and **channel**. The profile's `deliver_to` decides which. They share a structure and differ in what detail is safe to include.

## Design constraints

The PM reads this on a phone, before coffee, between the alarm and the first meeting.

**The first line must survive being the only line read.** Put the single most important thing there. If nothing is important, say that there — "nothing needs you today" is a complete and valuable brief.

**Sections are ordered by what someone must do, not by data category.** Escalate first, because it's the only part with a deadline. Areas and customers are supporting context and come after.

Never pad a section to look thorough. An empty Watch lane means a good day, and printing "no items" is more honest than promoting a Noted item to fill the space.

Per-ticket detail, full cluster membership and the area-by-area table go in the **thread**.

---

# DM variant

Private, so it can carry named accounts and tier labels.

```
*Ticket Radar — <Day DD Mon>*
<One sentence: the single thing that matters, or "Nothing needs you today.">

*Escalate today*
• *<Theme in plain words>* — <n> tickets, <n> accounts, since <when>
  <What's actually broken, one line.> <Why it clears the bar: rule met / regression / Nx baseline.>
  <Account names, tier in brackets.> <ticket links>

*Watch*
• *<Theme>* — <n> tickets, up from <baseline> typical. <Why it's not escalation yet.>

*Aging*
• <n> tickets open past <threshold> days with no reply. Oldest: <n> days, <account>. <link>

*Your areas*
<Area>: <n> new (typical <n>) · <Area>: <n> new (typical <n>)

*Who we heard from*
<n> accounts. Notable: <account [tier]> — <one line on what they raised>.

_<Coverage line.>_
```

## Worked example — DM

```
*Ticket Radar — Tue 18 Aug*
Invoice PDFs have been failing since Friday's release — 7 tickets, 4 accounts, two of them Enterprise.

*Escalate today*
• *Invoice PDF downloads return blank files* — 7 tickets, 4 accounts, since Fri 15 Aug
  Customers click download and get a 0-byte file. Started within a day of the 14 Aug billing
  release, so it reads as a regression. Clears the escalation rule three times over.
  Northwind [Enterprise], Contoso [Enterprise], Barton [Growth], Vale [Growth]. #48213 #48227 #48231

*Watch*
• *Saved payment methods disappearing on mobile* — 3 tickets, up from ~0 typical. All Android,
  all this week. Under the bar on account count, but the trend is one direction.

*Aging*
• 4 tickets open past 7 days with no reply. Oldest: 12 days, Fabrikam [Enterprise], card decline
  loop they've now chased twice. #47905

*Your areas*
Checkout: 4 new (typical 5) · Billing: 11 new (typical 3) · Payment Methods: 3 new (typical 2)

*Who we heard from*
9 accounts. Notable: Northwind [Enterprise] — third billing ticket in ten days, worth a check-in
independent of the PDF bug.

_Covered 08:00 Mon – 08:00 Tue across Zendesk. 41 tickets read. Service Cloud not connected._
```

Notice what this does. The headline names the problem and its blast radius in one sentence. Each escalation states *why it qualified*, so the PM can argue with the reasoning rather than taking a verdict on faith. Baselines appear inline as "typical N", which is what turns "11 new on Billing" from a number into an alarm. And the Northwind observation is a pattern across tickets that no single ticket would have revealed — that kind of connection is most of the value here.

---

# Channel variant

Same signal, wider room. Three things change, and the reasons matter more than the rules.

**Address the PM by name.** In a channel, `@mention` them in the first line. A brief with no owner is a brief nobody acts on, and in a shared channel the diffusion of responsibility is real.

**Drop commercial detail.** No tier labels, no "our biggest account", no revenue framing. A channel usually has a wider audience than the PM's mental model of it — contractors, new joiners, occasionally a guest account — and account value is the detail most likely to cause a problem if it travels. Say "4 accounts" rather than "4 accounts including two Enterprise".

**Let ticket links do the identifying.** Engineering needs to reach the actual ticket to debug, and the link does that job without putting customer names on a channel wall. When `name_customers` is true (the PM explicitly overrode the default), names are fine — otherwise refer to volume and link out.

The upside of this variant is real: engineering sees the escalation at the same moment the PM does, so the fix starts without a relay step. That's worth the coarser detail.

```
*Ticket Radar — <Day DD Mon>* · <@PM>
<One sentence: the single thing that matters, or "Nothing needs the team today.">

*Escalate today*
• *<Theme>* — <n> tickets, <n> accounts, since <when>
  <What's broken, one line.> <Why it clears the bar.>
  <ticket links>

*Watch*
• *<Theme>* — <n> tickets, up from <baseline> typical.

*Aging*
• <n> tickets open past <threshold> days with no reply. Oldest <n> days. <link>

*Areas*
<Area>: <n> new (typical <n>) · <Area>: <n> new (typical <n>)

_<Coverage line.> Full detail in thread._
```

## Worked example — channel

```
*Ticket Radar — Tue 18 Aug* · @sam
Invoice PDFs have been failing since Friday's release — 7 tickets across 4 accounts.

*Escalate today*
• *Invoice PDF downloads return blank files* — 7 tickets, 4 accounts, since Fri 15 Aug
  Customers click download and get a 0-byte file. First report landed within a day of the
  14 Aug billing release, so it reads as a regression rather than a long-standing gap.
  #48213 #48227 #48231

*Watch*
• *Saved payment methods disappearing on mobile* — 3 tickets, up from ~0 typical. Android only.

*Aging*
• 4 tickets open past 7 days with no reply. Oldest 12 days, a card decline loop. #47905

*Areas*
Checkout: 4 new (typical 5) · Billing: 11 new (typical 3) · Payment Methods: 3 new (typical 2)

_Covered 08:00 Mon – 08:00 Tue across Zendesk. 41 tickets read. Full detail in thread._
```

The regression framing survives into the channel version on purpose — it's the sentence that tells an engineer where to start looking, and it names a release rather than a customer.

---

# Quiet day

Same in both variants, minus the mention in DM.

```
*Ticket Radar — Wed 19 Aug*
Nothing needs you today. 6 new across your three areas (typical 7). Nothing aging past 7 days.

_Covered 08:00 Tue – 08:00 Wed across Zendesk. 6 tickets read._
```

Three lines. Resist expanding it — this format is what teaches the reader that a long brief means something real.

# Still learning

For the first week in an area, or any area with fewer than 7 rows of history, replace "typical N" with an explicit note rather than a fabricated comparison:

```
*Your areas*
Checkout: 4 new · Billing: 11 new · Payment Methods: 3 new
_Baselines still building — day 3 of 7. Trend comparisons start Friday._
```

It's tempting to compare against two days and call it a trend. Don't. The first spurious "up 300%" costs more trust than a week of honest silence buys back, and trust is the entire product here.
