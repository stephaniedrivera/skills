---
name: "content-strategy"
description: "Evaluate a feature summary from project-brain-synthesis and decide whether to write a new Help Center article, revise existing articles, or both — then hand that decision to hc-writer and/or hc-refresh. Use this skill whenever a feature summary exists and the next question is what to do with it documentationally. Trigger on 'what do we need to document', 'do we need a new article', 'what needs updating', 'coverage decision', or any time project-brain-synthesis has just run and the writing step hasn't started yet. Do NOT use this for general information architecture, content audits, or restructuring work unrelated to a specific feature — that is broader IA strategy work handled separately."
---
# Content-strategy
Takes a feature summary from project-brain-synthesis and decide the documentation approach. Output a decision packet that goes directly to hc-writer, hc-refresh, or both. This skill decides; it does not write.

## What to do
0. The input is a structured summary from project-brain-synthesis — feature name, new capabilities, affected UI areas, related features, caveats. If no summary is present, stop and ask for one before proceeding.
1. Search the live repo for existing articles that touch this feature. Look across the help-center/ directory and docs.json using the feature name, related UI surfaces, and workflow terms from the summary.
2. Classify each match:
   - Needs content update — instructions or UI descriptions are now wrong or incomplete given the new feature.
   - Needs crosslink only — content is still accurate, but should reference the new feature.
   - No relation — discard.
3. Decide:
   - New article: no existing page owns this job, or absorbing it would force an existing page to cover two unrelated jobs.
   - Revise existing: one or more matches need a content update and can absorb the new feature cleanly.
   - Both: the feature needs a new standalone article AND touches existing pages that need updating or crosslinking. This is the common case for a hub feature or a significant capability addition.
   - If the fit is ambiguous (two equally plausible existing homes, or genuinely unclear whether this warrants a new article) flag it. Don't pick silently. Surface the options and the tradeoff so the human can decide.
4. Output the decision packet (see format below) and hand it to:
   - hc-writer if the decision includes a new article
   - hc-refresh if the decision includes revising existing articles
   - Both, in that order, if the decision is "both"

## Decision packet format
Coverage decision: [Feature name]
Decision: new article / revise existing / both
Target article(s): [title(s) + repo path — existing or proposed]
Section/Category: [nav placement per docs.json]
Reasoning: [1-2 sentences tied to the evidence above]
Crosslinks: [inbound / outbound, if any]
Open items: [gaps, ambiguities, or none]

The packet is the handoff — everything hc-writer and hc-refresh need is in it. Don't proceed to drafting yourself.
