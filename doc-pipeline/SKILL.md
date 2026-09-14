---
name: "doc-pipeline"
description: "Orchestrates the full documentation workflow for a new or updated Descript feature — from reading the project brain through coverage decision to a written, verified, crosslinked, and brand-voiced Help Center draft. Use this whenever the user has a Linear project link and wants to go all the way from feature understanding to a draft article without manually invoking each skill. Trigger on 'run the doc pipeline', 'document this feature', 'start from the Linear project', or any time the user provides a Linear link and wants docs produced from it. Do NOT use this for standalone tasks — if you only need the feature summary use project-brain-synthesis directly; if you only need the coverage decision use docs-content-strategy directly; if you only need to write use docs-writer directly."
---

# Doc Pipeline

Runs seven skills in sequence to go from a Linear project link to a verified,
crosslinked, brand-voiced Help Center draft PR. Each skill does its own job —
this skill sequences them and carries output from one to the next.

```
1. project-brain-synthesis    → structured feature summary
2. docs-content-strategy      → coverage decision packet
3. docs-writer / docs-refresh → written, pushed branch
4. solo-verify                → accuracy check, draft PR, inline suggestions
5. docs-crosslink             → inbound and outbound crosslinks
6. unslop                     → remove AI writing patterns
7. anchovy                    → Descript brand voice
8. docs-frontmatter           → title, description, sidebarTitle on final content
```

## Steps

1. **Get the Linear project link** from the user if not already provided.

2. **Run project-brain-synthesis.** Pass the Linear link. Collect the structured
   feature summary it produces. If it flags gaps or stops to ask the user a
   question, wait for that to resolve before continuing.

3. **Run docs-content-strategy.** Pass the feature summary from step 2 as input.
   Collect the decision packet it produces. If it surfaces an ambiguity and asks
   the user to decide, wait for that to resolve before continuing.

4. **Run docs-writer and/or docs-refresh**, based on the decision packet from step 3:
   - **New article** → docs-writer
   - **Revise existing** → docs-refresh
   - **Both** → docs-writer first, then docs-refresh

5. **Run solo-verify.** It opens the draft PR, checks every factual and UI claim
   against Ask Solo, and posts inline suggestion comments on contradicted claims.
   Surface the verification summary to the user before continuing.

6. **Run docs-crosslink.** Pass the draft PR and file paths from solo-verify.
   It scans for inbound and outbound crosslink opportunities, applies them to the
   branch, and posts a summary comment on the PR.

7. **Run unslop.** Pass the current file(s) on the branch. It removes AI writing
   patterns — filler phrases, passive voice, over-hedging — and commits the
   cleaned version to the branch.

8. **Run anchovy.** Pass the unslopped file(s). It applies Descript's brand voice
   and commits the final version to the branch.

9. **Run docs-frontmatter.** Pass the final file(s). It writes `title`,
   `description`, and `sidebarTitle` against the finished article content and
   commits to the branch.

10. **Report when done:**
   - Branch name and draft PR link
   - Files created or changed
   - Verification summary (confirmed / contradicted / unverifiable counts)
   - Crosslinks added (inbound / outbound counts)
   - Any open items carried forward from the decision packet

## Failure handling

If any skill stops — because a doc failed to load, a Linear link 404s, a Notion
entry isn't found, or a question needs answering — stop the pipeline at that point
and surface the issue clearly. Don't skip ahead or paper over it.
