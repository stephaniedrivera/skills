---
name: "docs-refresh"
description: "Rewrite, fix, or refresh an existing Descript Help Center article (Mintlify/MDX) — especially after a product change makes it stale. Use whenever the user asks to update, refresh, fix, or rewrite an existing article, or references a product change that may have made current docs inaccurate. Also use in automated/unattended mode as part of step 4 of the doc-pipeline, when docs-content-strategy's decision is to revise an existing article rather than write a new one. Do NOT use this to write a brand-new article (docs-writer), to decide which articles need updating in the first place (docs-content-strategy) — this skill assumes the target article is already identified."
---

# Help Center Refresher

Revises an existing Help Center article. Assumes the target article is already
identified — by the user, or by a coverage decision packet from `docs-content-strategy`
in the pipeline. This skill doesn't decide *whether* to revise; it does the revision.

**Fetch and follow, don't restate:**
- **Style Guide**: https://www.notion.so/descript/973a7cd96d1f488e85ea69f8bbe7cf9f
- **Help Center Article Template**: https://www.notion.so/descript/249abe2e1a508096a1c1d4f54ae98378

## What to do

1. **Pull the current article** (from the repo, or the branch if running inside the
   pipeline).
2. **Find what's now inaccurate** — steps, labels, UI descriptions, limits, plan
   gating — against whatever changed (a product spec, a feature summary from
   `project-brain-synthesis`, or the user's own description of the change).
3. **Rewrite only the affected sections**, keeping the rest of the article's
   structure intact, per the Template and Style Guide.
4. **Re-check frontmatter** via **docs-frontmatter** if the change affects what the
   article covers.
5. **Report what changed**, most-inaccurate first — a plain list of before/after,
   not a full re-diff.

## Lifecycle calls belong to the user

Whether a promo ended, a feature is deprecated, or users actually still do X: flag
it, don't guess. To **delete** a page: remove it from `docs.json` navigation, repoint
or remove any redirects whose destination was that page, then re-validate `docs.json`
as JSON.

## Pipeline context

When invoked from `docs-writer`'s pipeline-draft step, you'll be on an existing
feature branch — commit your changes there, same branch, don't create a new one.
Report back file paths and a one-line summary per file so `docs-writer` can pass that
along to `solo-verify`.
