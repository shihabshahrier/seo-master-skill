# Implementation Checklist (Prioritized)

Run top-to-bottom. Each phase is roughly highest-ROI first. Check the box, move on.

## Phase 0 — Audit (understand current state)

- [ ] List all public routes/pages.
- [ ] Grep every `<title>` — note length, consistency, duplicates, bare `— Brand`.
- [ ] Grep for existing JSON-LD / schema — what types, where.
- [ ] Check for prerendering (view-source on a route: is content there w/o JS?).
- [ ] Check robots.txt, sitemap.xml, llms.txt exist + current.
- [ ] Check main JS bundle size (build output) — any >500KB chunk warning?
- [ ] **Hunt fabrications**: fake ratings/reviews in schema, copied competitor
      stats, unverifiable claims. (Critical — these cause penalties.)
- [ ] Identify the market leader's content moat + your missing presence in
      "alternatives" listicles.

## Phase 1 — Indexing reach (do first; feeds all AI engines)

- [ ] Google Search Console: add property, verify, submit sitemap.
- [ ] Bing Webmaster Tools: add site (import from GSC), submit sitemap, enable
      AI Performance report.
- [ ] IndexNow: Cloudflare Crawler Hints toggle OR key file + `indexnow.mjs`.
- [ ] robots.txt: allow Google/Bing/Brave + AI citation bots; block private
      routes + aggressive scrapers; add Content-Signal lines; reference sitemap.
- [ ] llms.txt at root + `<link rel="alternate" type="text/markdown">`.
- [ ] Confirm Brave-crawlable (don't block its UA) for Claude reach.

## Phase 2 — Structured data (biggest content lever)

- [ ] Site-wide `@graph` (Organization + WebSite + SoftwareApplication) in root head.
- [ ] FAQPage (visible FAQ + JSON-LD) on home, about, pricing, comparison,
      templates, every content/hub page.
- [ ] Article schema on blog/guides; HowTo on tutorials/fixes; Course +
      LearningResource on courses; BreadcrumbList on deep pages; ItemList on
      indexes; Product/CreativeWork on assets.
- [ ] Remove ALL fabricated aggregateRating/review. Replace with featureList /
      real metrics.
- [ ] Shared `@id` so duplicate entities dedupe.
- [ ] Validate in Rich Results Test + Schema validator.

## Phase 3 — Answer-first content + titles

- [ ] Rewrite hero + key H2s answer-first (answer in first 40–60 words).
- [ ] Thread the positioning wedge (speed/privacy/niche) through copy.
- [ ] Standardize titles: keyword-first, ≤60 chars, consistent suffix
      (full descriptor on marketing, `| Brand` on content). No bare `— Brand`.
- [ ] Unique meta descriptions, answer-first, 140–160 chars.
- [ ] Add fact density (a concrete fact every 150–200 words) + 15+ named entities
      on key pages.

## Phase 4 — Content moat (the long game)

- [ ] `/learn` hub: quick guides (HowTo+FAQ).
- [ ] `/learn/problems`: error/how-to fixes (answer-first "The fix" + FAQ) —
      **highest-ROI content**.
- [ ] `/learn/course`: multi-lesson course (Course schema, prev/next).
- [ ] `/learn/<topic>-in-30-minutes`: quickstart targeting the head term.
- [ ] Rendered output + copy/"Try in tool" buttons on code/examples.
- [ ] Data-driven (typed arrays + one renderer per type); code-split the section.
- [ ] Every new page → add to sitemap.

## Phase 5 — Comparison + off-site

- [ ] `/vs/<competitor>` pages for each major rival (answer-first + table +
      wedge + FAQ schema). Targets "X alternative" AI queries.
- [ ] Reconcile any pricing/claim inconsistencies across pages.
- [ ] Off-site (manual): AlternativeTo, Product Hunt, Reddit, G2/Capterra,
      Awesome-lists, niche/.edu directories. Keep a `SEO_SUBMISSIONS.md` with
      paste-ready copy.

## Phase 6 — Verify + monitor

- [ ] Build green (typecheck + bundler); confirm main bundle didn't bloat
      (content/libs in split chunks).
- [ ] Prerender ran; each route's HTML has meta + content.
- [ ] Deploy; spot-check live routes (view-source).
- [ ] Run IndexNow; submit/refresh sitemaps in GSC + Bing.
- [ ] Monitor: GSC Performance, Bing AI Performance, Core Web Vitals.
- [ ] Monthly: query ChatGPT/Claude/Perplexity target prompts; track citations.

## Engineering hygiene (while implementing)

- Client-only changes for a marketing site; don't touch backend/admin unless needed.
- Commit in logical chunks with clear messages; don't commit local agent dirs
  (e.g. `.claude/`).
- Verify typecheck + build after each batch.
- Reusable `<SEO>` component for title/meta/OG/canonical/JSON-LD.
- Data-driven content (arrays) so non-devs can extend by adding entries.
