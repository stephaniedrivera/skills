---
name: "solo-verify"
description: "Verify factual and UI claims in a drafted or revised Help Center article against the actual product, using Ask Solo (search_product_context) as the source of truth. Use this whenever a Help Center draft needs an accuracy pass before publishing, or whenever the user asks to fact-check or verify an article against the codebase. Do NOT use this for style or voice review (anchovy) or structural review (docs-review) — this skill checks only whether claims are true, not whether they're well written."
---
 
# Solo verify
 
Checks every checkable claim in a Help Center draft against Ask Solo
(`Ask Solo:search_product_context`) before the article is considered ready to
publish. This is a factual accuracy pass, not a style pass.
 
## Steps
 
0. The input is a handoff from docs-writer containing a branch name and a list of
   file paths (relative to `help-center/`). Check out the branch and locate each
   file. If the handoff is missing, the branch doesn't exist, or a file path can't
   be found, stop and notify the user before proceeding.
   **Open a PR** from the branch into main before running verification — the PR is
   where the review comments will land. If a PR for this branch already exists,
   use it rather than opening a duplicate.
1. Extract checkable claims from the draft — anything that describes what the
   product does, where a control lives, what a setting is called, plan/pricing
   gating, or a limitation.
2. Query Ask Solo per claim (or per closely related cluster of claims — batch
   naturally, don't fire one query per sentence if several claims describe the same
   flow). Use specific, concrete queries — feature name + what it claims to do —
   not the whole paragraph pasted in.
3. Classify each claim:
   - **Confirmed** — Ask Solo's result supports the claim as written.
   - **Contradicted** — Ask Solo's result conflicts (wrong label, wrong location,
     feature works differently than described). Note the correct version if Ask
     Solo's result makes it clear; otherwise flag as "needs human confirmation."
   - **Unverifiable** — Ask Solo has no relevant result. This is common for very
     new features the codebase context hasn't caught up to yet — don't treat
     silence as confirmation. List these separately.
4. Cross-check against screenshots/UI if the draft references specific labels —
   Ask Solo is a codebase search tool, and labels in code can drift from what's
   actually rendered. Where the stakes are high (a button name, a menu path), and
   a live UI check is feasible, do it; otherwise flag the claim as
   code-confirmed-only.
5. Do not silently edit the draft. For each contradicted or unverifiable claim,
   post an inline review comment on the PR at the specific line where the claim
   appears:
   - **Contradicted (fix is clear)** — post a GitHub suggestion comment using the
     ` ```suggestion ` block format so the human can accept it with one click and
     it commits directly to the branch.
   - **Contradicted (fix is unclear)** — post a plain comment with what Ask Solo
     returned and flag as "needs human confirmation."
   - **Unverifiable** — post a plain comment noting no Ask Solo result was found
     and recommending manual confirmation before publish.
Contradicted claims about plan/pricing gating or destructive actions ("can't be
undone") are always high-severity — flag these first, regardless of how minor they'd
otherwise seem, since getting them wrong misleads users about cost or risk.
 
Report the verification summary when done. A draft with contradicted claims should
not be published without the human resolving them first — say so explicitly rather
than assuming it's implied.
 
## Output
 
Post inline PR comments for every contradicted or unverifiable claim. Then output
a summary in chat:
 
### Verification: [article title]
PR: [link]
Confirmed: [count] claims
Contradicted: [count] — posted as inline PR comments
Unverifiable: [count] — posted as inline PR comments
