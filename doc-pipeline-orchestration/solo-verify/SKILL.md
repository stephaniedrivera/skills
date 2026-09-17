---
name: solo-verify
description: "Verify factual and UI claims in a drafted or revised Help Center article against the actual product, using Ask Solo (search_product_context) as the source of truth. Use this whenever a Help Center draft needs an accuracy pass before publishing, or whenever the user asks to fact-check or verify an article against the codebase."
---

# Solo verify

Checks every checkable claim in a Help Center draft against Ask Solo
(`Ask Solo:search_product_context`) before the article is considered ready to
publish. This is a factual accuracy pass, not a style pass.

## Steps

0. The input is one of two things:
   - **(a) A fresh handoff** from docs-writer/docs-refresh: a Mintlify branch name
     and a list of file paths (relative to `help-center/`).
   - **(b) A re-verify request** against a branch that was already drafted and
     verified earlier (e.g., the Week 2 "launch week" pass of the weekly doc
     pipeline, re-checking a draft PR from the week before). Here you'll get just
     a branch name, with no fresh file-path handoff. Check out the branch via
     `Mintlify:checkout`, then determine which `help-center/` files it actually
     touches yourself — via `Mintlify:diff` against the base branch, or
     `Mintlify:list_nodes`/the branch's existing PR file list — rather than
     stopping for a missing handoff.

   In either case, check out the branch via `Mintlify:checkout` and read each
   file. If the branch doesn't exist, or no files can be determined either from
   a handoff or from inspecting the branch, stop and notify the user before
   proceeding.

   **On a re-verify (case b), treat every claim as needing a fresh check** — don't
   just diff against the prior report. If the article was drafted before the
   feature shipped, claims that came back "Unverifiable" last time are the
   highest-priority ones to re-check first: Ask Solo's index should now reflect
   the shipped code, so what was unverifiable before may be confirmable now.

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
     silence as confirmation. List these separately. If this is a re-verify pass
     (case b) and a claim is still unverifiable after the feature has supposedly
     shipped, say so explicitly — that's a signal worth flagging louder than a
     first-pass unverifiable, since the shipped code should be indexed by now.
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
   - **Fix is bigger than a claim-level edit** (a whole flow changed, multiple
     sections need restructuring) — flag it as "needs docs-refresh," rather than
     forcing a patchy claim-by-claim edit. Whoever invoked you (the doc-pipeline,
     or the weekly trigger's Week 2 "patch gaps" step) is responsible for handing
     that off to docs-refresh; this skill doesn't do rewrites itself.

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
Mode: first pass / re-verify
Confirmed: [count] claims
Fixed: [count] — applied directly to branch
Needs human review: [count] — listed below
Needs docs-refresh: [count] — listed below

#### Fixed
- Claim: "[original]" → Fixed to: "[corrected]"

#### Needs human review
- Claim: "[exact claim]"
  Issue: [what Ask Solo shows]
  Action needed: [what the human needs to confirm or decide]

#### Needs docs-refresh
- Claim/section: "[what's affected]"
  Why a claim-level fix isn't enough: [reason]
```

Discard the Mintlify session when done — do not save or push anything beyond
the corrections already committed. (This does not affect the branch's PR state —
marking a PR ready for review or merging it is a separate action taken by whoever
invoked this skill, not by solo-verify itself.)
