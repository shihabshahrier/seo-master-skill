# Content Strategy + Answer Engine Optimization (AEO/GEO)

The shift: AI engines don't want keyword-stuffed pages — they want **answers**
they can lift and cite. Write for extraction.

## The AEO weighting (what AI engines reward, 2026)

Roughly, in order of weight:
1. **FAQPage schema quality** (~20%)
2. **Answer-first formatting** (~19%) — direct answer in first 40–60 words
3. **Statistical / fact density** (~16%) — a concrete fact every 150–200 words
4. Clean heading hierarchy, semantic completeness, citations, freshness
- Backlinks and keyword density now weigh *less* than these.

Other measured signals:
- Self-contained passages of **134–167 words** are ~4.2× more likely to be cited.
- **15+ named entities** per page → ~4.8× higher AI-Overview selection.
- Pages combining text + image + (video) + schema → much higher selection.
- Fast First Contentful Paint (<0.4s) → ~3× ChatGPT citations.

## Answer-first writing (the core technique)

Every page and section: **state the answer in the first sentence**, then expand.

Bad: "LaTeX has many ways to handle images. Historically… eventually you'll want…"
Good: "**To add an image in LaTeX, load `graphicx` and use `\includegraphics[width=\linewidth]{file}`.** Then…"

Apply to: hero copy, every H2 section, FAQ answers, error-fix pages. The first
40–60 words are what gets quoted.

## Fact density

Drop a concrete, checkable fact every 150–200 words: a number, a command, a
version, a measured claim. "Compiles in 1–2 seconds." "80+ templates across 10
categories." "Free unlimited collaborators." AI engines prefer dense, specific
content over vague prose.

## Named entities

Mention real proper nouns relevant to the topic — products, standards, orgs,
formats, universities, packages. They are the anchors AI uses to judge topical
authority. (e.g. for LaTeX: IEEE, Springer, Nature, ACM, BibTeX, amsmath,
Beamer, TikZ, Overleaf, specific universities.)

## The four content types (and their SEO value)

| Type | Example | SEO/AEO value | Effort |
|------|---------|---------------|--------|
| **Quick guides** | "How to add images in X" | High | Low |
| **Problem/error fixes** | "X error: undefined control sequence — fix" | **Highest** (high-intent, low-competition, AI loves answer-first fixes) | Low |
| **Course / learning path** | "The Complete X Course" (8 lessons) | Medium (depth, dwell time, brand) | High |
| **Quickstart** | "Learn X in 30 minutes" | High (head-term magnet) | Medium |

### The "problems/errors" play — best ROI

The single highest-value content surface. People literally type error messages
into Google and ask ChatGPT. Build a `/learn/problems` hub:
- One page per common error/how-to. Title = the exact error/question users search.
- Structure: **answer-first "The fix" box** → why it happens → correct code
  example → 2–3 FAQs. Article + FAQPage schema.
- These are low-competition, high-intent, and convert ("try the fix in [tool]").
- Find the queries: the product's own error messages, "X error", "how to Y in X",
  autocomplete, People-Also-Ask, competitor's most-trafficked help pages.

### Content hub structure (the moat)

Market leaders win on a **learn/docs hub** (Overleaf has ~255K organic keywords
largely from its Learn hub). Build:
```
/learn                         hub index
  /learn/<topic>               quick guides (HowTo + FAQ schema)
  /learn/problems              error-fix index (ItemList)
  /learn/problems/<error>      per-error fix (Article + FAQ)
  /learn/course                multi-lesson course (Course schema)
  /learn/course/<lesson>       lesson (LearningResource + FAQ)
  /learn/<topic>-in-30-minutes quickstart (HowTo + FAQ, head-term)
```
Make it **data-driven**: lessons/guides/problems as typed data arrays, rendered
by one page component each. Adding content = append a data entry + 1 sitemap line.

### Courses — build as "learning paths"

Don't write 20 all-new deep lessons in one pass (accuracy risk, slow). Either:
- **Substantial** (~600–900 words/lesson): answer-first intro + 4–7 concept
  sections + runnable code + key-commands table + FAQs. Good default.
- **Textbook-depth** (1500+ words): better long-term, but verify accuracy
  carefully; do iteratively.

Reuse existing guides as lessons in a sequenced path with `Course` + `hasPart`.
Add prev/next nav, reading-time estimate, per-lesson `LearningResource` schema.

## Rendered output + interactivity (makes content visual)

Code-only lessons are flat. Add:
- **Rendered output** next to source (e.g. KaTeX for math: show `\frac{a}{b}`
  source AND the rendered fraction). Input→output pairing is what makes docs click.
- **Copy button** + **"Try in [tool]"** on code blocks (copies snippet, opens
  the editor/signup). Interactive, on-brand, drives conversion.
- Keep heavy render libs (KaTeX ~263KB) in a **code-split chunk** so only
  content-page visitors download them.

## Hard content rules

- Every claim true and yours. Never copy a competitor's stat.
- Answer-first, always.
- Visible FAQ + matching FAQPage schema (never schema-only).
- `dateModified` current.
- Internal-link between hub pages (guide ↔ related problem ↔ course lesson).
- End each content page with a relevant CTA into the product.
