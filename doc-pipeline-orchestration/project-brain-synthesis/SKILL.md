---
name: "project-brain-synthesis"
description: "Reads a Linear project's linked Notion 'project brain' database and synthesizes a structured feature summary from it. Use this whenever the user needs a summarized understanding of a new feature that's shipping. Trigger when the user says 'read the project brain for X', names a feature or Linear project to document, or kicks off the writing workflow for a release. Do NOT use this for writing the Help Center article itself (hc-writer) or for deciding article coverage (content-strategy) — this skill only produces the summary those steps consume."
---

# Project Brain Synthesis

## Steps

0. **Ask for the Linear project** if one hasn't been provided. You need either a
   Linear project link or a project name to proceed — don't guess or search broadly.
1. **Get the Linear project.** Fetch it via `Linear:get_project`. Pull its description.
2. **Find the project-brain Notion link.**
3. **Match this Linear project to its Notion project brain row**, in order of preference:
   - A Notion URL already present on the Linear project (fastest, most reliable)
   - The project name, matched against the database's title/name property
   If neither matches cleanly, list the closest candidates and ask the user to
   confirm rather than guessing.
4. **Pull all linked/nested docs** (specs, design notes, decision logs, Slack thread
   recaps if pasted in, etc.) via `Notion:notion-fetch` /
   `Notion:notion-query-data-sources`. If the total content is large, read it in
   chunks and summarize as you go rather than truncating silently.
5. **Pull the Linear project's own issues/sub-issues** (`Linear:list_issues` scoped
   to the project) — these often carry scope and status details the Notion docs
   don't, especially near ship date.
6. **Synthesize a structured summary:**
   - Feature name:
   - What changed: (1-3 sentence plain-English summary)
   - New capabilities: (bullet list of what users can now do)
   - Affected UI areas: (where in the product this shows up)
   - Caveats/limitations: (known issues, edge cases, plan restrictions)
   - Related features: (existing features this connects to or changes)
   - Sources: (links to every doc/issue this summary drew from)
7. **Flag any gaps — do not fill them.** If the project brain is thin (e.g. no
   caveats documented, no affected UI areas info), say so explicitly rather than
   inferring plausible-sounding detail.
8. **Hand off** the summary to `content-strategy` for the coverage decision.
