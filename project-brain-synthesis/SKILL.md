---
name: project-brain-synthesis
description: Reads a Linear project's linked Notion 'project brain' database and synthesizes a structured feature summary from it. Use this whenever the users needs a summarized understanding of the new feature that's shipping. Whenever the user says 'read the project brain for X', or as step 2 of the weekly-release-doc-pipeline right after release-pipeline-trigger finds the Linear link(s). Do NOT use this for writing the Help Center article itself (hc-writer) or for deciding article coverage (hc-content-strategy) — this skill only produces the summary those steps consume.
---
# Project Brain Synthesis

## Steps
0. Ask for the Linear project if one hasn't been provided. You need either a Linear project link or a project name to proceed — don't guess or search broadly.
1. Get the Linear project. Fetch it via Linear:get_project. Pull its description.
2. Find the project-brain Notion link. 
3. Match this Linear project to its row using, in order of preference:
   - a Notion URL already present on the Linear project (fastest, most reliable)
   - the project name, matched against the database's title/name property If neither matches cleanly, list the closest candidates and ask the user to confirm rather than guessing.
4. Pull all linked/nested docs (specs, design notes, decision logs, Slack thread recaps if pasted in, etc.) via Notion:notion-fetch / Notion:notion-query-data-sources. If the total content is large, read it in chunks and summarize as you go rather than truncating silently.
5. Also pull the Linear project's own issues/sub-issues (Linear:list_issues scoped to the project) — these often carry scope and status details the Notion docs don't, especially near ship date.
6. Synthesize a structured summary, same shape used elsewhere in the docs pipeline so downstream skills can consume it directly:
  - Feature name:
  - What changed:        (1-3 sentence plain-English summary)
  - New capabilities:    (bullet list of what users can now do)
  - Affected UI areas:   (where in the product this shows up)
  - Caveats/limitations: (known issues, edge cases, plan restrictions)
  - Related features:    (existing features this connects to or changes)
  - Sources:             (links to every doc/issue this summary drew from)
7. Flag any gaps (do not presume and don't fill missing info). If the project brain is thin (e.g. no caveats documented, no affected-UI-areas info), say so explicitly in the summary rather than inferring plausible-sounding detail.

## Multiple Linear projects in one run

If release-pipeline-trigger passed more than one project link, produce one summary per project, clearly labeled, and run the rest of the pipeline once per feature. Don't merge unrelated features into a single summary.
