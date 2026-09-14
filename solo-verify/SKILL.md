---
name: "solo-verify"
description: "Verify factual and UI claims in a drafted or revised Help Center article against the actual product, using Ask Solo (search_product_context) as the source of truth. Use this whenever a Help Center draft needs an accuracy pass before publishing, or whenever the user asks to fact-check or verify an article against the codebase."
---

# Solo verify

Checks every checkable claim in a Help Center draft against Ask Solo
(`Ask Solo:search_product_context`) before the article is considered ready to
publish. This is a factual accuracy pass, not a style pass.

## Steps

0. The input is a handoff from docs-writer containing a Mintlify branch name and a
   list of file paths (relative to `help-center/`). Check out the branch via
   Mintlify (`Mintlify:checkout`) and read each file. If the handoff is missing,
   the branch doesn't exist, or a file path can't be found, stop and notify the
   user before proceeding.
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
5. **Apply fixes and report.** Don't just flag issues — act on them:
   - **Contradicted (fix is clear)** — apply the fix directly to the file via
     Mintlify `edit_page`. Don't ask; just fix it.
   - **Contradicted (fix is unclear)** — flag in the report as "needs human
     confirmation." Don't guess.
   - **Unverifiable** — flag in the report. Don't invent a fix.

   After applying fixes, save to the branch via `Mintlify:save` with a clear
   commit message listing what was corrected. Then output the verification report
   in chat. A draft with remaining unresolved items (unclear contradictions or
   unverifiables) should not be published without the human reviewing them — say
   so explicitly.

Contradicted claims about plan/pricing gating or destructive actions ("can't be
undone") are always high-severity — flag these first, regardless of how minor they'd
otherwise seem, since getting them wrong misleads users about cost or risk.

## Output

```
### Verification: [article title]
Branch: [Mintlify branch name]
Confirmed: [count] claims
Fixed: [count] — applied directly to branch
Needs human review: [count] — listed below

#### Fixed
- Claim: "[original]" → Fixed to: "[corrected]"

#### Needs human review
- Claim: "[exact claim]"
  Issue: [what Ask Solo shows]
  Action needed: [what the human needs to confirm or decide]
```

Discard the Mintlify session when done — do not save or push anything beyond
the corrections already committed.
