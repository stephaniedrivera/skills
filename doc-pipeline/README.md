# Doc Pipeline — Skills README

End-to-end documentation pipeline for Descript Help Center content.
Start here before editing any skill.

## Architecture

Everything runs in Claude (Cowork scheduled task). Mintlify is the repo
interface and PR host — it has no automation role.

```
TRIGGER
Cowork scheduled task (Tue 5am / 2pm CT, Wed 5am CT)
        ↓
CLAUDE — full pipeline
  1.  Slack → find PM-PMM post (channel C07P1PQ757S)
  2.  Extract items into four buckets:
        A. Last week ships (catch-up)
        B. This week ships (pre-ship)
        C. Next week ships (early prep)
        D. Experiments (hidden article + support note)
  3.  Check for existing branch: docs/<linear-project-id>-<slug>
  4.  project-brain-synthesis  → feature summary
  5.  docs-content-strategy    → coverage decision
  6.  docs-writer / docs-refresh → branch + draft
  7.  solo-verify              → accuracy check + auto-fix
  8.  docs-crosslink           → inbound + outbound links
  9.  unslop                   → remove AI writing patterns
  10. anchovy                  → Descript brand voice
  11. docs-frontmatter         → title, description, sidebarTitle
  12. Open draft PR via Mintlify MCP
  13. DM Stephanie (U04RH7BA1CP) on Slack
        ↓
YOU
  Review draft PR → assign yourself → mark ready → CI runs → merge
```

## Skill files

| Skill | File | Job |
|---|---|---|
| `doc-pipeline` | doc-pipeline/SKILL.md | Orchestrator — runs steps 1–13 |
| `project-brain-synthesis` | project-brain-synthesis/SKILL.md | Reads Linear + Notion project brain |
| `docs-content-strategy` | docs-content-strategy/SKILL.md | Decides new / revise / both |
| `docs-writer` | docs-writer/SKILL.md | Writes new articles + creates branch |
| `docs-refresh` | docs-refresh/SKILL.md | Revises existing articles |
| `solo-verify` | solo-verify/SKILL.md | Accuracy check via Ask Solo, auto-fixes |
| `docs-crosslink` | docs-crosslink/SKILL.md | Adds inbound + outbound links |
| `docs-frontmatter` | docs-frontmatter/SKILL.md | title, description, sidebarTitle |

`unslop` and `anchovy` are organization-level skills.

## Branch naming convention

```
docs/<linear-project-id>-<feature-slug>
```

Example: `docs/fdd77828-regenerate-smooth-jump-cuts`

The Linear project ID is the short hex string from the project URL. Using it
as a prefix makes branch existence checks deterministic and every branch
traceable back to Linear.

## Tool dependencies

| Step | Requires |
|---|---|
| Slack trigger + DM | **Slack** MCP |
| project-brain-synthesis | **Linear** (get_project, list_issues), **Notion** (notion-fetch, notion-ai-search) |
| docs-content-strategy | **Mintlify** (checkout, read, search) |
| docs-writer / docs-refresh | **Mintlify** (checkout, write_page, edit_page, save) |
| solo-verify | **Ask Solo** (search_product_context), **Mintlify** (read, edit_page, save) |
| docs-crosslink | **Mintlify** (search, read, edit_page, save) |
| unslop / anchovy | In-context pass — no external tools |
| docs-frontmatter | **Mintlify** (read, edit_page, save), **Notion** (capitalization guide) |
| PR creation | **Mintlify** (save, mode: pr) |

## Mintlify settings

- `createDraftPrByDefault: true` — all PRs open as drafts automatically.
  Mark ready for review manually in GitHub when you want CI to run.

## Cowork task

See `cowork-doc-pipeline-task.md` for the full task spec to paste into
Cowork. Schedule: Tuesday 5am CT, Tuesday 2pm CT, Wednesday 5am CT.

## Known gaps (as of 2026-09-14)

- **GitHub MCP** requires admin permission in the Descript org — not
  available for standard users. All branch and file operations go through
  Mintlify MCP instead. solo-verify reports in chat rather than as inline
  PR comments.
- **PR assignee** — Mintlify's save tool doesn't support setting a PR
  assignee. Assign yourself manually in GitHub after the PR opens.
- **Pipeline pause points** — not yet decided. Run end to end first, then
  add gates based on real usage.
- **Mintlify .mintignore behavior** — unconfirmed whether the workflow
  agent bypasses .mintignore. Ticket to Mintlify support drafted but not
  yet sent. Relevant if skills are ever moved into the repo.
- **Anchovy quality on Mintlify** — not tested. Anchovy stays in Claude
  for now. Revisit if Mintlify automation credit usage becomes a priority.

## Archived

- `mintlify-workflows.md` — obsolete. Mintlify automations were evaluated
  but all pipeline steps stayed in Claude due to Mintlify's agent lacking
  access to Linear, Notion, and Ask Solo. The broken link check is already
  covered by the existing CI linter.
