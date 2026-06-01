---
name: seo-master
description: >
  Complete SEO + GEO (Generative Engine Optimization) + AEO (Answer Engine
  Optimization) playbook for 2026. Covers traditional search ranking, how
  AI engines (ChatGPT, Claude, Gemini, Perplexity, Copilot) discover/rank/cite
  pages, structured data, technical SEO, the title/meta system, content
  strategy, IndexNow, Google + Bing Search Console setup, competitor analysis,
  and a concrete implementation checklist. Use when optimizing any website for
  search engines and AI answer engines, planning content/SEO strategy, adding
  structured data, fixing titles/meta, getting cited by AI chatbots, or setting
  up Search Console / Webmaster Tools.
---

# SEO Master Skill (SEO + GEO + AEO, 2026)

Battle-tested playbook distilled from optimizing a real SaaS (letx.app, a
collaborative LaTeX editor) against AI-native competitors. Everything here is
applied knowledge, not theory.

## The one-paragraph mental model

In 2026 there are **two audiences**: classic search crawlers (Google, Bing) and
**AI answer engines** (ChatGPT, Claude, Gemini, Perplexity, Copilot). They
overlap but differ. SEO ranks you in a list of links. **GEO/AEO gets you cited
inside an AI's generated answer.** A solid SEO foundation is required for both;
AEO/GEO adds: answer-first writing, dense structured data (especially FAQPage),
fact density, freshness, and being present where each AI grounds its search.
Critically — **each AI uses a different search index**, so "getting indexed" is
not one task but several.

## How to use this skill

1. Read `references/ai-engines-and-indexes.md` FIRST — it determines *where* to
   optimize. Wrong index = invisible to that AI.
2. Run the audit in `references/implementation-checklist.md` against the site.
3. Fix in priority order: indexing reach → structured data → answer-first
   content → titles/meta → content moat.
4. Use `references/search-console-setup.md` for Google + Bing + IndexNow.
5. Use `references/structured-data.md` and `references/content-and-aeo.md` as
   copy/paste references while implementing.

## Reference files (read as needed)

| File | What's in it |
|------|--------------|
| `references/ai-engines-and-indexes.md` | Which index each AI uses, how each cites, per-engine tactics |
| `references/structured-data.md` | Every schema type with ready JSON-LD: FAQPage, HowTo, Course, SoftwareApplication, Article, Breadcrumb, ItemList, Product. Policy rules (fake ratings). |
| `references/titles-and-meta.md` | Title formula, the brand-suffix decision, meta description, OG/Twitter, canonical |
| `references/content-and-aeo.md` | Answer-first writing, fact density, FAQ pattern, content hubs, the "problems/errors" play, courses |
| `references/technical-seo.md` | Prerendering SPAs, sitemap, robots.txt + Content-Signals, llms.txt, code-splitting, Core Web Vitals, canonical/redirects |
| `references/search-console-setup.md` | Google Search Console, Bing Webmaster Tools + AI Performance, IndexNow (Cloudflare + script), submission steps |
| `references/competitor-and-positioning.md` | How to research competitors, find the open lane, off-site listicle strategy |
| `references/implementation-checklist.md` | Prioritized, copy-paste audit + execution checklist |

## Hard rules (learned the hard way)

- **Never fabricate `aggregateRating`/reviews in schema.** Google structured-data
  policy violation → manual action. Only use rating schema backed by visible,
  real on-page reviews. Use `featureList` / plain claims instead.
- **Never copy a competitor's stat.** ("20M users" belonged to Overleaf, not us —
  caught and removed.) Every claim must be true and yours.
- **Title ≤ 60 chars** or Google truncates. Long brand descriptors don't fit on
  content pages — use a short `| Brand` suffix there.
- **FAQ schema requires the FAQ to be visible on the page.** Schema-only FAQ =
  policy risk. Always render the questions AND emit JSON-LD.
- **Code-split heavy content** (course data, KaTeX, etc.) so the main JS bundle
  every visitor downloads stays small. Prerendered HTML keeps full content for
  SEO regardless.
- **IndexNow does NOT notify Google** — only Bing/Yandex/DuckDuckGo/etc. Google
  uses its own crawl + Search Console.

## The priority order (highest ROI first)

1. **Indexing reach** — be in Google, Bing, and Brave indexes; wire IndexNow.
   (Feeds ChatGPT via Bing, Claude via Brave.)
2. **Structured data** — FAQPage on every page that can carry it (~3.2× AI
   Overview citation). Plus SoftwareApplication/Product/Article/HowTo.
3. **Answer-first content** — direct answer in first 40–60 words; 134–167-word
   self-contained passages.
4. **Titles/meta** — consistent, keyword-first, ≤60 chars.
5. **Content moat** — guides/tutorials/error-fix hub targeting the long-tail
   the competitor monetizes.
6. **Off-site** — get listed in the "alternatives" listicles AI engines cite.
