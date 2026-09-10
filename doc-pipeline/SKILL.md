---
name: "doc-pipeline"
description: "Orchestrates the full documentation workflow for a new or updated Descript feature — from reading the project brain through coverage decision to a written, pushed Help Center draft. Use this whenever the user has a Linear project link and wants to go all the way from feature understanding to a draft article without manually invoking each skill. Trigger on 'run the doc pipeline', 'document this feature', 'start from the Linear project', or any time the user provides a Linear link and wants docs produced from it. Do NOT use this for standalone tasks — if you only need the feature summary use project-brain-synthesis directly; if you only need the coverage decision use content-strategy directly; if you only need to write use hc-writer directly."
---

# Doc Pipeline

Runs three skills in sequence to go from a Linear project link to a pushed Help
Center draft. Each skill does its own job — this skill sequences them and carries
output from one to the next.

```
1. project-brain-synthesis  → structured feature summary
2. content-strategy         → coverage decision packet
3. hc-writer / hc-refresh   → written, pushed branch
```

## Steps

1. **Get the Linear project link** from the user if not already provided.

2. **Run project-brain-synthesis.** Pass the Linear link. Collect the structured
   feature summary it produces. If it flags gaps or stops to ask the user a
   question, wait for that to resolve before continuing.

3. **Run content-strategy.** Pass the feature summary from step 2 as input.
   Collect the decision packet it produces. If it surfaces an ambiguity and asks
   the user to decide, wait for that to resolve before continuing.

4. **Run hc-writer and/or hc-refresh**, based on the decision packet from step 3:
   - **New article** → hc-writer
   - **Revise existing** → hc-refresh
   - **Both** → hc-writer first, then hc-refresh

5. **Report when done.** Once the branch is pushed, summarize:
   - Branch name
   - Files created or changed
   - Any open items carried forward from the decision packet

## Failure handling

If any skill stops — because a doc failed to load, a Linear link 404s, a Notion
entry isn't found, or a question needs answering — stop the pipeline at that point
and surface the issue clearly. Don't skip ahead or paper over it.
