---
name: "docs-crosslink"
description: "Find and add crosslink opportunities between a new or revised Help Center article and the rest of the help-center directory. Use after solo-verify has confirmed accuracy, as step 5 of doc-pipeline, or any time the user asks to find crosslink opportunities for an article. Identifies both inbound links (existing articles that should link to this one) and outbound links (places in this article that should link elsewhere). Do NOT use this to write new content or verify factual claims — this skill only adds links."
---

# Docs Crosslink

Finds and adds crosslink opportunities between the target article and the rest of
the Help Center. Runs after solo-verify — content should be accurate before links
are added. This skill adds links; it does not write or verify content.

## Steps

0. The input is the draft PR and file paths from solo-verify's handoff. If either
   is missing, stop and notify the user.
1. **Scan the help-center directory** for articles that are topically related to
   the new or revised article — look for shared feature names, UI surfaces, and
   workflows from the feature summary.
2. **Identify outbound opportunities** — places in the target article that reference
   a concept, feature, or workflow that has its own dedicated article. These should
   link out rather than re-explain.
3. **Identify inbound opportunities** — existing articles that mention this feature
   or workflow but don't yet link to the target article. These should gain a link.
4. **Apply the links:**
   - Link at the point of use, not just at first mention — if a concept appears in
     an intro and again in a step where the user needs to act on it, link both. If
     the same concept appears multiple times in the same paragraph, once is enough.
   - Use the feature or concept name as link text, never "click here" or "this article."
   - Edit the relevant file on the same branch at the most natural mention of the
     concept, whether that's the new article or an existing one.
5. **Commit the changes** to the same branch with a clear message
   (`docs: add crosslinks for <feature> (SUP-XXXX)`).
6. **Post a comment on the draft PR** listing what was linked, inbound and outbound,
   so the human can review the additions alongside the rest of the verification
   comments.

## Output

```
## Crosslinks: [article title]
Outbound: [count] links added
- "[link text]" → [target article path]

Inbound: [count] links added
- [source article path] → "[link text]"
```

If no crosslink opportunities are found, say so explicitly — don't add links for
the sake of it.
