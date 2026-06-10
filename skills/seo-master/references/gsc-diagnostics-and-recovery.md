# GSC Diagnostics & Recovery Playbook

Field-tested 2026-06 on letx.app: 3-month GSC export showed 3.94% blended CTR
and "position collapse" — root causes were architectural, not content. This file
is the method for diagnosing a site from Search Console data and the recovery
recipes for the failure patterns we actually hit.

## 1. How to read a GSC export (the segmentation method)

Blended sitewide metrics LIE. Always split before concluding anything:

1. **Brand vs non-brand queries.** letx.app blended: 3.94% CTR, pos ~6.6.
   Reality: brand 6.5% CTR @ pos 2.5; non-brand **0.31% CTR @ pos ~24**.
   The growth problem was invisible in the blended number.
2. **Striking distance cohort** — pages at pos 4–15 with high impressions and
   near-zero CTR. These are snippet problems (title/meta/rich-result), not
   ranking problems. Fix the snippet, not the content.
3. **Page-2 graveyard** — pos >15 with high impressions. These are content-depth
   problems (you're thinner than whoever owns page 1). Rewrite/expand; titles
   won't save them.
4. **Country split.** Find the market with the most impressions and worst CTR —
   that's the unlock (letx: US = 26% of impressions @ 0.68% CTR while
   Bangladesh sat at 16.7%). Localized authority (links, copy) is the lever.
5. **Device split as a CWV smell.** Desktop pos 12.3 vs mobile 4.3 + a desktop
   CLS flag = the CWV penalty is measurable in rankings.
6. **Slash-pair detection** (high-signal!): aggregate Pages.csv by
   `url.rstrip('/')` — if the same path appears twice (with/without trailing
   slash), ranking signals are split. letx had **102 pairs splitting half of
   all impressions**.

### Interpretation traps
- **Mix shift ≠ penalty.** Launching many new pages drops blended position/CTR
  mechanically (new URLs enter at pos 10–40 and add low-CTR impressions). Don't
  accept a "Navboost is punishing you" narrative without segmenting first.
- **Irrelevant-query pollution**: a brand that collides with a generic phrase
  ("LetX" ↔ "let x =" algebra homework) permanently pollutes the impression
  denominator. Aggregate CTR becomes a vanity metric — set CTR goals per query
  cohort, not sitewide. Mitigate by pairing brand + category everywhere
  ("LetX — Collaborative LaTeX Editor") so the Knowledge Graph disambiguates.

## 2. The prerendered-SPA canonical trap (CRITICAL pattern)

If a React SPA is prerendered (Puppeteer baking pages to `route/index.html`):

- A static `<link rel="canonical">` in `index.html` gets baked into **every**
  prerendered page → every page claims to be a duplicate of the homepage.
- react-helmet then injects a second canonical at runtime → **two conflicting
  canonicals** in the served HTML. Google ignores both or picks the wrong one.
- Symptom set in GSC: "Page with redirect" coverage errors, "Crawled — currently
  not indexed", duplicated slash/non-slash rows in Pages, weak rankings sitewide.

**Fix recipe:**
1. Remove canonical + og:url from the static `index.html` shell entirely.
2. Centralize URL normalization in the SEO component (one function builds the
   canonical from origin + path; all call sites go through it).
3. Post-process in the prerender script: keep exactly ONE canonical per page
   (prefer the helmet one, force correct href), align og:url, dedupe static
   social tags shadowed by helmet versions, inject a fallback canonical when a
   page never rendered the SEO component.
4. **Verify what the crawler receives**: `curl <url> | grep canonical` on the
   deployed site. Runtime DevTools inspection lies — the baked HTML is what
   counts. This single verification found the whole bug class.

## 3. Trailing-slash unification

- Decide direction by **what the host does**, not by preference. Cloudflare
  Pages serves prerendered routes as directories and 308-redirects non-slash →
  slash. Fighting that (e.g. blindly copying Next.js `trailingSlash: false`
  advice) creates redirect loops. **Framework advice does not transfer across
  render models — detect the stack first.**
- Then make everything agree with that one form: canonicals, og:url, sitemap
  `<loc>`s, RSS links, and internal `<a href>`s (rewrite hrefs at prerender
  time so crawlers never hop through the 308).
- A sitemap full of URLs that redirect = every entry wasted; this shows up as
  GSC "Page with redirect" against sitemap URLs.

## 4. CTR surgery (what still works in 2026)

- **FAQ rich results are dead for normal sites** (gov/health only since 2023).
  FAQPage schema is still worth it — for AI-engine citation, not SERP stars.
  Don't promise CTR from it.
- The realistic SERP CTR levers now:
  - **Title rewrite** on the striking-distance cohort: keyword-first, concrete
    benefit, ≤60 chars, `| Brand` suffix. "LaTeX Error: Missing $ inserted" →
    "Missing $ Inserted LaTeX Error — Cause & Fast Fix | LetX".
  - **Meta = conversion copy ending in a CTA** ("Open it in LetX now."). For
    DB-driven pages, build the meta from a template: first ~90 chars of the DB
    description + fixed benefit+CTA tail — and AUDIT THE DB DATA (we found
    template descriptions that were literal lorem-ipsum-grade junk feeding
    straight into metas).
  - **ItemList schema with image thumbnails** on gallery/collection pages —
    image-rich list presentation is the visual-SERP play for template/product
    queries.
  - `aggregateRating` without real, visible on-page reviews = manual-action
    risk. Still true, still tempting, still no.
- Two of your pages ranking pos 5–6 for the same query is **double SERP
  presence, not cannibalization** — keep both and differentiate; merge only
  when neither ranks.
- Verify "duplicates" before merging: two near-identical slugs may be genuinely
  distinct products (awesome-cv vs awesome-cv-resume = multi-page vs one-page).

## 5. CLS quick wins (desktop ranking drag)

- **Web-font swap reflow** is the usual culprit: define a fallback
  `@font-face` with `size-adjust` / `ascent-override` / `descent-override` /
  `line-gap-override` matched to the web font (Inter ≈ 107.4% / 90.2% / 22.48%
  / 0% over Arial), add it to the font stack. Plus `preconnect` to font CDNs.
- **SVG `<img>` needs explicit width/height attributes** — SVGs have no
  intrinsic size; CSS height alone still shifts layout.
- Containers with `aspect-ratio` (Tailwind `aspect-[3/4]`) already reserve
  space — check before "fixing" images that aren't the problem.
- CrUX is a 28-day window: the GSC CLS flag clears slowly after the fix ships.

## 6. Verification discipline (do these before AND after)

```bash
# what does the crawler actually get?
curl -s URL | grep -o '<link[^>]*canonical[^>]*>'        # exactly one? right URL?
curl -sI URL-without-slash | head -3                      # which way does the host redirect?
curl -s ORIGIN/sitemap.xml | grep -o '<loc>[^<]*' | grep -cv '/$'   # 0 = all match slash form
curl -s URL | grep -c 'property="og:title"'               # 1, not 2 (helmet + static dupes)
```

Re-run the GSC export ~4 weeks after shipping; compare the striking-distance
cohort (position + CTR per page) against the saved baseline, not blended totals.

## 7. Reality-check targets

- 15–20% CTR is achievable **per target query at pos 1–3 in low-competition
  niches** (we measured 26% on one), never sitewide when brand-collision noise
  pollutes the denominator.
- Canonical/slash consolidation effects: 1–2 weeks. Position lift on
  striking-distance pages: 2–4 weeks. Page-2 → page-1 via content depth:
  4–8 weeks + usually needs links (.edu outreach for academic niches: one
  university LaTeX-guide link ≈ top-3 for that university's template query).
