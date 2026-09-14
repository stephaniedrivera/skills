# Doc Pipeline — Skills README

End-to-end documentation pipeline for Descript Help Center content.
Start here before editing any skill.

## Pipeline overview

```
1. project-brain-synthesis   → feature summary from Linear + Notion
2. docs-content-strategy     → coverage decision (new / revise / both)
3. docs-writer / docs-refresh → branch + draft
4. solo-verify               → draft PR + Ask Solo accuracy check
5. docs-crosslink            → inbound + outbound links
6. unslop                    → remove AI writing patterns
7. anchovy                   → Descript brand voice
8. docs-frontmatter          → title, description, sidebarTitle (runs last)
```

Invoke via `doc-pipeline` with a Linear project link. Each skill can
also be invoked standalone — see individual skill files for details.

## Skill files

| Skill | File | Job |
|---|---|---|
| `doc-pipeline` | doc-pipeline/SKILL.md | Orchestrator — runs 1–8 in sequence |
| `project-brain-synthesis` | project-brain-synthesis/SKILL.md | Reads Linear + Notion project brain |
| `docs-content-strategy` | docs-content-strategy/SKILL.md | Decides new / revise / both |
| `docs-writer` | docs-writer/SKILL.md | Writes new articles + creates branch |
| `docs-refresh` | docs-refresh/SKILL.md | Revises existing articles |
| `docs-review` | docs-review/SKILL.md | Quality / structure review |
| `solo-verify` | solo-verify/SKILL.md | Accuracy check via Ask Solo |
| `docs-crosslink` | docs-crosslink/SKILL.md | Adds inbound + outbound links |
| `docs-frontmatter` | docs-frontmatter/SKILL.md | title, description, sidebarTitle |

`unslop` and `anchovy` are organization-level skills — edit them in
their own skill files, not here.

## Tool dependencies

Each skill requires specific MCP connectors. Make sure these are
connected before running the pipeline.

| Step | Requires |
|---|---|
| `project-brain-synthesis` | **Linear** (get_project, list_issues), **Notion** (notion-fetch, notion-ai-search) |
| `docs-content-strategy` | **Mintlify** (checkout, read, search) for repo access |
| `docs-writer` | **Mintlify** (checkout, write_page, save), **Git** (branch creation via Mintlify) |
| `docs-refresh` | **Mintlify** (checkout, read, edit_page, save) |
| `solo-verify` | **Ask Solo** (search_product_context), **GitHub** (open draft PR, post review comments) |
| `docs-crosslink` | **Mintlify** (read, edit_page), **GitHub** (post PR comment) |
| `docs-frontmatter` | **Mintlify** (read, edit_page) |
| `unslop` | None (in-context pass) |
| `anchovy` | **Notion** (fetch Descript Voice doc) |

> **Note:** GitHub MCP does not appear to load in standard Claude chat
> sessions — it loads in Claude Code. If solo-verify can't open a PR,
> switch to Claude Code or use the Mintlify save + manual PR flow.
> This is a known gap — track in Linear if it blocks the pipeline.

## Known gaps (as of 2026-09-14)

- **GitHub MCP** not available in Claude chat sessions — affects
  solo-verify (draft PR + inline comments). Workaround: run the
  pipeline in Claude Code, or open the PR manually after docs-writer
  pushes the branch.
- **Mintlify branch creation** is handled via Mintlify MCP checkout,
  not a git CLI — branch naming (`docs/<slug>`) is enforced by
  docs-writer's instructions but Mintlify auto-generates the branch
  name. May need reconciliation.
- **Pipeline pause points** not yet decided — run `doc-pipeline` end
  to end first, then add gates where needed based on real usage.
- **Slack trigger / automation** not yet built — pipeline is manually
  invoked. Automation design is parked pending pipeline stabilization.
