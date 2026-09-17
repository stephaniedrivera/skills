---
name: docs-writer
description: "Write a new Descript Help Center article (Mintlify/MDX), including creating the feature branch it belongs on. Use once docs-content-strategy has already decided a new article is needed (as opposed to revising an existing one, or both) — this skill assumes that call is made, not making it itself. Trigger when the user asks to write, draft, or create a new Help Center article. This skill only writes new articles — for revising an existing article use docs-refresh, for reviewing a draft use docs-review, for frontmatter use docs-frontmatter, for deciding what articles should exist use docs-content-strategy."
---

# Help Center Writer

Writes new Help Center articles, starting from creating the branch they'll live on.
**Assumes `docs-content-strategy` has already decided a new article is needed** — this
skill doesn't make that call, it acts on it. If the decision was "both" (new article
+ revise existing), this skill handles the new-article half; `docs-refresh` handles
the rest.

Nothing else lives here — revising existing articles, reviewing drafts, frontmatter,
and IA decisions are each their own skill (see the description above). Invoke them;
don't reimplement them here.

**Fetch and follow, don't restate:**
- **Style Guide**: https://www.notion.so/descript/973a7cd96d1f488e85ea69f8bbe7cf9f
- **Help Center Article Template**: https://www.notion.so/descript/249abe2e1a508096a1c1d4f54ae98378
- **Capitalization Guide + feature name database**: https://www.notion.so/descript/241abe2e1a5080a1ad8cd0c3fb7cd644
- **Writing Guidelines**: https://www.notion.so/descript/4b0b93da3e9a485baf336281ba13ab17
- **Descript Voice**: https://www.notion.so/descript/ae07929a5c6a4fe4928f83fb42f5e9d7

These are the living source of truth. If you cannot access one for any reason, stop
the task and notify the user — don't write from memory of what it used to say.

## Task: Write an article

0. **Take docs-content-strategy's decision as your brief.** Before creating anything,
   use its output for: the target article title, Section/Category placement (for
   `docs.json` nav), and any outbound crosslinks it identified. Don't re-derive
   these yourself or invent a different title — if something's missing from the
   decision (e.g. no Section given), ask rather than guessing.
1. **Create the feature branch, if one doesn't already exist.** Repo uses
   sparse-checkout on `descriptinc/descript`, content under `help-center/`.

   Branch naming: `help-docs/<short-id>-<linear-issue-id>-<slug>`, where
   `short-id` is the 8-character hex Linear **project** short ID (e.g. `fdd77828`
   from `linear.app/descript/project/regenerate-fdd77828d8fb`), `linear-issue-id`
   is the Support-team **issue** identifier tracking the doc work (e.g. `SUP-123`),
   and `slug` is the feature name kebab-cased. Example:
   `help-docs/fdd77828-SUP-123-regenerate-smooth-jump-cuts`. Including the issue ID
   auto-links the PR to the Linear issue; including the project short ID keeps the
   branch traceable to the feature and makes existence checks deterministic.

   If you weren't handed a branch name (e.g. running standalone rather than inside
   the weekly pipeline, which usually creates the branch itself before calling
   you), and no `linear-issue-id` is available yet, ask for one rather than
   guessing at a placeholder — a branch created without the issue ID won't
   auto-link, and a later rename is more disruptive than asking now.

   If a branch matching `help-docs/<short-id>-*` already exists, check it out
   instead of creating a duplicate — report this to the user.
2. **Structure, voice, capitalization, components** — fetch the Article Template
   and Style Guide above and follow them exactly.
3. **Output Mintlify MDX.**
4. **MDX compatibility pass** — confirm the document uses only Mintlify-compatible
   MDX (per the Style Guide's component rules) before committing.
5. **Commit and push.** Commit to the branch with a clear message, then push it.
   Output the following handoff for solo-verify:
   ```
   Branch: help-docs/<short-id>-<linear-issue-id>-<slug>
   Files: [list of file paths created or modified, relative to help-center/]
   ```
   Do not open a PR — that's solo-verify's job.
