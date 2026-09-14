---
name: "docs-frontmatter"
description: "Write, review, and score frontmatter for Descript Help Center (Mintlify MDX) articles — title, description, and sidebarTitle. Use whenever creating or auditing a page's frontmatter or its SEO/AI meta description: writing a new description, checking one against standards, scoring descriptions keep/review/rewrite, or bulk-auditing descriptions across many articles. Trigger on 'description', 'frontmatter', 'meta description', 'sidebarTitle', 'SEO description', or article metadata for Help Center / Mintlify pages — even if not phrased that way."
---

# Help Center frontmatter & descriptions

Owns the frontmatter for Descript Help Center articles (Mintlify MDX): `title`, `description`, `sidebarTitle`. Two modes: (1) write frontmatter for a new or edited article, (2) review or score existing descriptions — one at a time or as a bulk audit.

Pairs with **docs-writer** (prose + structure). When writing an article, draft the frontmatter — especially the `description` — **last**, after the body exists, so it's accurate and covers the whole page.

## Fields

```yaml
---
title: "Clean up your audio with Studio Sound"
description: "Reduce background noise and improve audio clarity in your recordings."
sidebarTitle: "Studio Sound"
---
```

- `title`: sentence case, SEO-driven, action + feature. Overviews are the exception: **[Feature name] overview**.
- `description`: one outcome-focused sentence — see the standard below.
- `sidebarTitle`: short label (feature name or 2–3 words).
- Optional: `icon`, `tag: "New"`, `deprecated: true`.
- **Feature-name capitalization** follows the Descript Capitalization Guide + feature name database (Notion, the living source): https://www.notion.so/descript/241abe2e1a5080a1ad8cd0c3fb7cd644 — capitalize branded features (Overdub, Studio Sound, Eye Contact, Underlord, Regenerate); lowercase generic nouns (projects, scenes, timeline, script). Fetch it when unsure.

## The description standard

The `description` becomes the page's **SEO meta description** *and* the context fed to the Help Center **AI assistant**. It does a different job than the intro — make it earn its slot.

Every description:

- **Accurate first (hard rule).** Matches what the article actually covers — right feature, right platform, no overclaiming. Inaccurate is worse than none: Google rewrites it and reader trust erodes. This is the failure a length-and-style pass misses.
- **One sentence, ~50–150 chars,** key point in the first ~120 (mobile truncates there). Never over 160; never a bare title restatement.
- **Matches intent, covers the whole page** — not one sub-section.
- **Leads with the primary search term**, naturally.
- **Plain, active, grammatical, zero typos.**
- **A concrete reason to click** — real action or value, not clickbait.

House voice:

- Verb-first (`Fix…`, `Add…`, `Import…`, `Create…`, `Review…`) or a short definition (`Quick Design turns…`, `The Media Library is…`).
- Name **Descript** where it reads naturally (`in Descript`, `to your Descript account`, `Descript exports`).
- Plain over salesy — cut trailing "by doing X / caused by Y / so that Z" clauses.
- Sentence case, ends with a period. No "you can", no "whether you're".

## Description vs. the article intro — don't repeat

The `description` and the article's **first body paragraph** must not say the same thing. They're read in different places and do different jobs: the description is the searchable one-liner seen before the click; the intro orients the reader who has already landed.

- The description is the **canonical outcome line** — keep it. Don't contort it to dodge overlap, and don't pad it to sound different from the intro.
- If the article's intro restates the description's sentence or its key phrasing, that's a defect in the **intro** — fix it there (see docs-writer), not by rewording the description.
- Only change the description for overlap if it's also inaccurate, over length, or a bare title restatement. Otherwise leave it alone and let the intro do the complementing.
- Sharing a few key terms across the two (for consistency and SEO) is fine; repeating the sentence is not.

## Self-check / scoring

Score every description against these; all must pass:

1. Accurate to the body — feature, platform, scope?
2. ≤160 chars, key point in the first ~120, not a bare title restatement?
3. Covers the whole page and matches searcher intent?
4. Primary search term present and early?
5. Plain, grammatical, no typos?
6. Concrete reason to click?
7. Not a duplicate of the first body paragraph? (If it overlaps, the usual fix is the intro, not the description — flag it for docs-writer.)

**Verdict:** fail #1 (accuracy) or #5 (typos/grammar) → **rewrite**. Fail any other → **tighten**. All pass → **keep**.

## Bulk audit mode

When auditing many descriptions at once (a section or the whole Help Center):

- **Read each article body** to judge accuracy and whole-page coverage — never score from the description alone.
- **Compare the description against the first body paragraph** for repetition — near-verbatim intros are the most common defect. When they duplicate, the fix is almost always to rewrite the intro (hand it to docs-writer), keeping the description.
- Report per page: score, verdict (keep/review/rewrite), the concrete issue(s), and a suggested fix in house voice for anything below keep.
- Sort worst-first. Flag **accuracy failures, typos, and description/intro duplication loudest** — those are what a length-and-style pass misses.

## Examples (weak → strong)

- `Descript's refund policy` → `Request a Descript refund within 48 hours of your invoice date.` *(bare title restatement)*
- `Publish a finished episode from Descript to Buzzsprout.` → `Publish a finished episode from Descript to Captivate.` *(wrong platform — accuracy fail)*
- `Quick Design creates a polished video with a single click.` → `Quick Design turns a raw script or transcript into a scene-based rough cut — a fast first draft to build on.` *(overclaimed the outcome)*
- `How the timeline works in Descript.` → `Fine-tune your composition's timing, visuals, and audio sync in Descript's timeline.` *(thin → concrete)*
- `Add custom fonts to your Descript drive.` → `Add custom fonts from Google Fonts or your own files to your Descript Drive.` *(thin → names the real sources)*
