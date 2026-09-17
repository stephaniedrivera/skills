---
name: weekly-doc-pipeline-trigger
description: Tuesday: parse Kathy's #pm-pmm launches message, run the 3-week doc pipeline (draft next-week items, ready-review this-week items, audit last-week items), post Slack summary to #help-content.
---

## Goal

Run the Help Center doc pipeline across three stages, shifting the heavy lifting earlier so it happens while there's time instead of during launch week:

- **Week 1** ("next week" section — not shipped yet): full pipeline, opens a **draft** PR.
- **Week 2** ("this week" section — launch week): re-verify the draft PR from last week and get it ready for review.
- **Week 3** ("last week" section — shipped last week): audit that the PR merged and the live docs match what shipped.

Post a Slack summary with PR links and statuses to #help-content.

---

## Step 1 — Find Kathy's message

Search #pm-pmm (C07P1PQ757S) for the most recent message from U0A3KR8JAB0 (Kathy Wang) containing "Key launches last week." within the last 14 days. If not found, stop.

Record:
- The message text
- The message timestamp (`message_ts`) — needed for the Slack summary later

---

## Step 2 — Extract features by section and route to a stage

Parse the message into three buckets, each mapped to a pipeline stage:

| Section | Stage | Treatment |
|---|---|---|
| "Key launches next week" | **Week 1 — Draft** | Full pipeline (brain synthesis → content strategy → write/refresh → solo-verify → crosslink → unslop → anchovy → frontmatter), ending in a **draft** PR. Create Linear issue, Todo → In Progress. |
| "Key launches this week" | **Week 2 — Ready for review** | Find last week's draft PR for this feature. Re-run solo-verify (product may have changed since drafting). Patch gaps. Mark the PR ready for review. Move Linear to In Review. |
| "Key launches last week" | **Week 3 — Audit** | Check whether last week's PR merged and the docs are live. Spot-check the live article against what actually shipped. If the PR is still open, escalate. Move Linear to Done. |

For each bullet in each section:
- Skip any item marked `[internal, do not market]`
- Skip any item with no `linear.app/descript/project/` link (Slack-thread-only items may not have a Linear project; note them in the summary)
- Keep all others regardless of tier (T2/T3/T4) or audience (Ent-only still needs docs)

---

## Step 3 — Locate existing state (per feature, stage-dependent)

For each kept feature, extract the **Linear project short ID** from the URL (the 8-character hex segment, e.g. `fdd77828` from `.../regenerate-fdd77828d8fb/...`).

Call `Mintlify:list_branches` on deployment `descript-5bf56f3f` and look for a branch matching `help-docs/<short-id>-*`. Also search the Support team (ID: `38106f2c-d274-4cf5-894d-3c73dcd8509f`) for an existing Linear issue for this feature with label "Needs Help Docs" (ID: `bf329647-9b7e-4f94-8cfd-367f1c9b6177`). Record the issue identifier (e.g. `SUP-123`) if found.

What "found" and "not found" mean differs by stage:

**Week 1 (next week):** Expect no branch yet — this is normally the feature's first pass.
- No branch → proceed to Step 5, Week 1 track.
- Branch already exists (feature got pulled forward, or this is a re-run) → check it out and resume from the last completed step rather than restarting.

**Week 2 (this week):** Expect a branch + draft PR from last week's Week 1 run.
- Branch + open (draft) PR found → check it out; this is the normal case. Proceed to Step 5, Week 2 track.
- Branch found, no open PR → check it out, inspect how far it got, and resume forward into the Week 2 track once it reaches draft-PR state.
- **No branch found** → this feature skipped Week 1 (added late, short notice, or missed last week's run). Run the full Week 1 track now as a compressed catch-up, flag it as "late-added — compressed timeline" in the summary, then continue directly into the Week 2 track in the same run.

**Week 3 (last week):** Expect a branch + PR from two weeks of work.
- Branch/PR found → check the PR's actual status (see Step 5, Week 3 track) rather than assuming.
- **No branch found at all** → this feature was never staged. Escalate: flag it in the summary as "shipped with no docs work ever started" — don't attempt to backfill the full pipeline inside the audit stage.

---

## Step 4 — Find the Notion project brain (Week 1 track, and Week 2's late-added fallback, only)

Week 2's normal path and Week 3 reuse the brain synthesis already saved in the Linear issue description / PR description from Week 1 — don't re-fetch Notion for those.

For features running the Week 1 track:

1. Fetch the Linear project to get its summary/description and Slack channel ID.
2. Look in the Linear project summary for a Notion project brain link (format: `[🧠 Project Brain](https://www.notion.so/descript/...)` or similar).
3. If not in the Linear summary, search Notion for `<feature name> project brain` and look in the Project Brains database.
4. Fetch the Notion page and check Linear project status. Note it for the decision packet, but **do not stop for an unshipped status here** — Week 1 runs on "next week" items by design, before they've shipped. Pass this context into docs-content-strategy in Step 5 so it treats the run as pre-launch staging rather than stopping to ask.
5. If no project brain is found anywhere, note it in the summary and skip.

---

## Step 5 — Run the pipeline (per feature, one at a time)

**Run one feature at a time — do not process multiple features concurrently.** Mintlify editor sessions are shared per deployment, not per branch. Parallel runs cause session collisions where one agent's writes bleed into another's `save` call. Complete all sub-steps for one feature (including its PR action) before starting the next.

### Week 1 track — "next week" features: draft everything early

1. **project-brain-synthesis**: Synthesize the structured feature summary. Save the full output — it goes in the Linear issue and the PR description.
2. **docs-content-strategy**: Decide new article / revise existing / both. Explicitly tell it this is a **pre-launch staging run** (feature ships next week, not yet shipped) so it proceeds instead of stopping on an unshipped status, and marks its decision packet "Provisional — pre-launch."
3. **Linear issue**: create in the Support team (ID: `38106f2c-d274-4cf5-894d-3c73dcd8509f`) if none exists — title `HC docs: [Feature name]`, label "Needs Help Docs" (ID: `bf329647-9b7e-4f94-8cfd-367f1c9b6177`), status Todo, description with the Linear project URL and the full brain synthesis. Then move it Todo → **In Progress** once you start the branch/write steps below. Record the issue identifier (e.g. `SUP-123`) — the next two weeks depend on finding this same issue and branch.
4. **Branch**: create or check out `help-docs/<short-id>-<linear-issue-id>-<feature-slug>` on deployment `descript-5bf56f3f`.
5. **docs-writer** and/or **docs-refresh**: write or update articles per the content strategy decision.
6. **solo-verify**: fact-check every checkable claim against Ask Solo. Apply fixes directly. Expect more "Unverifiable" claims than usual — the feature hasn't shipped, so the codebase Ask Solo indexes may not reflect it yet. That's expected here; Week 2 re-checks once it has.
7. **docs-crosslink**: add inbound/outbound links.
8. **unslop**: plain-language pass.
9. **anchovy**: Descript brand voice pass.
10. **docs-frontmatter**: title, description, sidebarTitle.
11. Open a **draft** PR via `Mintlify:save` (draft — do not mark ready, do not merge). PR description must include: the full brain synthesis, files created/modified, the solo-verify report, the crosslink summary, and a line noting `🚧 Drafted ahead of ship — needs re-verification before merge.`
12. Update the Linear issue: add the PR URL to the description, confirm status is **In Progress** (not In Review yet — that's next week, once it's actually ready).

### Week 2 track — "this week" features: launch week, get the PR review-ready

1. Check out the branch/PR found (or just created via the compressed catch-up) in Step 3.
2. **Re-run solo-verify** against the branch's current files. Treat this as a fresh accuracy pass, not a diff against last week's report — the product may have changed since drafting, and claims that were "Unverifiable" last week (feature not yet shipped) should now be checkable. Apply fixes directly.
3. **Patch gaps**: if solo-verify's report surfaces something bigger than a claim-level fix (e.g., a whole flow changed), hand off to docs-refresh for that section, then save.
4. Save all fixes to the branch via `Mintlify:save`.
5. **Mark the PR ready for review** (undraft it) — do not merge it.
6. Update the Linear issue: move status to **In Review**, and add a comment with the current solo-verify results (confirmed / fixed / needs-human-review counts).

### Week 3 track — "last week" features: audit that it actually shipped

1. Locate the branch/PR from Step 3.
2. Check the PR's actual status — merged, still open, or closed without merging. (Branch presence alone isn't proof of anything: check the PR/merge state directly, e.g. via Mintlify or the deployment's GitHub state, not just whether the branch still exists.)
   - **Merged** → docs are live. Spot-check the **live published article** (not the old branch) against what actually shipped: re-check the key claims from the original brain synthesis using Ask Solo (`search_product_context`) or a live UI check. Record confirmed vs. drifted.
   - **Still open** → escalate. This feature shipped last week but its docs PR never merged. Flag it prominently in the Slack summary as needing immediate human attention. Do not attempt to merge it yourself.
   - **Closed without merging** → escalate the same way, noting it was closed unmerged.
3. Update the Linear issue:
   - Merged + spot-check clean → move status to **Done**.
   - Merged + spot-check found drift → move to Done but add a comment flagging the drift for a human to review.
   - Escalated (still open / closed unmerged) → leave status as-is, add a comment describing the escalation. Don't mark Done on unresolved work.

---

## Step 6 — Post Slack summary

After all features are processed, post a new message to #help-content (C025SSEL0BG) summarizing the run.

Format:

```
📚 Docs pipeline run — [date] (from Kathy's [date] launches message)

🆕 Drafted this week (Week 1 — next week's launches):
• [Feature name] ([tier]) → draft PR [PR URL] | [Linear issue ID]

✅ Ready for review (Week 2 — this week's launches):
• [Feature name] → [PR URL] | [Linear issue ID]

📗 Live & verified (Week 3 — last week's launches):
• [Feature name] → confirmed live, no drift | [Linear issue ID]
• [Feature name] → live, drift found: [one-line description] | [Linear issue ID]

⚠️ Escalations:
• [Feature name] — shipped [date] but PR [PR URL] is still open | [Linear issue ID]
• [Feature name] — shipped with no docs work ever started | [Linear issue ID]

🔁 Late-added (skipped Week 1, ran compressed catch-up):
• [Feature name] → [PR URL] | [Linear issue ID]

⏭️ Already handled (existing branch/PR, no action needed):
• [Feature name] → [existing PR URL or branch name]

⛔ Skipped:
• [Feature name] — [reason: internal / no project brain / no Linear link / no branch found for audit]
```

If nothing was processed (all skipped or already handled), post a brief note saying so rather than an empty message.
