---
name: "docs-frontmatter"
description: "Write, review, and score frontmatter for Descript Help Center (Mintlify MDX) articles — title, description, and sidebarTitle. Use whenever creating or auditing a page's frontmatter or its SEO/AI meta description: writing a new description, checking one against standards, scoring descriptions keep/review/rewrite, or bulk-auditing descriptions across many articles. Trigger on 'description', 'frontmatter', 'meta description', 'sidebarTitle', 'SEO description', or article metadata for Help Center / Mintlify pages — even if not phrased that way."
---

# Help Center frontmatter & descriptions

Owns the frontmatter for Descript Help Center articles (Mintlify MDX): `title`, `description`, `sidebarTitle`. Two modes: (1) write frontmatter for a new or edited article, (2) review or score existing descriptions — one at a time or as a bulk audit.

Pairs with **docs-writer**. Draft frontmatter **last**, after the body exists.

## Fields

```yaml
---
title: "Clean up your audio with Studio Sound"
description: "Reduce background noise and improve audio clarity in your recordings."
sidebarTitle: "Studio Sound"
---
```

- `title`: sentence case, SEO-driven, action + feature. Exception: **[Feature name] overview**.
- `description`: one outcome-focused sentence — see the standard below.
- `sidebarTitle`: feature name or 2–3 words.
- Optional: `icon`, `tag: "New"`, `deprecated: true`.
- **Capitalization** — fetch the living source: https://www.notion.so/descript/241abe2e1a5080a1ad8cd0c3fb7cd644. Branded features cap (Overdub, Studio Sound, Underlord); generic nouns don't (projects, scenes, timeline).

## Description standard

The `description` is the SEO meta description and the AI assistant's context for this page. Every description must:

1. **Accurate** — right feature, right platform, no overclaiming. Inaccurate is worse than none.
2. **≤160 chars**, key point in the first ~120. Never a bare title restatement.
3. **Covers the whole page**, not one sub-section.
4. **Primary search term early**, naturally.
5. **Grammatical, no typos.**
6. **Concrete reason to click** — real action or outcome.

Voice: verb-first (`Fix…`, `Add…`, `Create…`) or short definition (`Quick Design turns…`). Name Descript where natural. Sentence case, ends with a period.

**Don't repeat the intro.** If the intro duplicates the description, fix the intro — not the description.

## Self-check

Fail #1 or #5 → **rewrite**. Fail any other → **tighten**. All pass → **keep**.

## Bulk audit mode

Read each article body before scoring — never judge from the description alone. Report per page: verdict (keep/tighten/rewrite), the issue, and a suggested fix. Sort worst-first; flag accuracy failures and description/intro duplication loudest.

## Examples

- `Descript's refund policy` → `Request a Descript refund within 48 hours of your invoice date.` *(bare title restatement)*
- `Quick Design creates a polished video with a single click.` → `Quick Design turns a raw script into a scene-based rough cut — a fast first draft to build on.` *(overclaimed → accurate)*
