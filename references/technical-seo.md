# Technical SEO

The plumbing that makes crawlers + AI engines able to read, render, and trust
the site.

## Prerendering / SSR for SPAs (critical for React/Vue/SPA)

A client-rendered SPA serves an empty `<div id="root">` — crawlers and many AI
fetchers see nothing. Fix: **prerender every public route to static HTML at
build time** so the served HTML already contains meta tags, structured data, and
visible content. No JS required to read the page.

Pattern (Vite + Puppeteer, what worked):
1. `vite build` produces the SPA + assets.
2. A `prerender.mjs` script reads routes from `sitemap.xml`, spins a local static
   server over `dist/`, and uses Puppeteer to render each route, waiting for
   Helmet meta + content to appear.
3. Write each route's full HTML to `dist/<route>/index.html`.
4. Mark `<div id="root" data-prerendered="true">` so the client hydrates instead
   of re-rendering.
5. Save the original empty `index.html` as `_spa_fallback.html` for unknown
   routes; serve the prerendered file per route.
6. Skip gracefully if Puppeteer/Chromium unavailable (e.g. some CI) — the SPA
   still works, just without prerendered HTML.

Wire into build: `"build": "vite build && node scripts/prerender.mjs"`.

Key: prerendering means **adding pages does NOT slow users** — each visit
downloads one page's HTML + the shared (code-split) JS, not all pages.

## Code-splitting (keep the main bundle small)

Heavy or rarely-needed content/libs must not ship in the bundle every visitor
downloads. Use `React.lazy()` + `Suspense` to split route groups.

```tsx
const CoursePage = lazy(() => import('./pages/CoursePage'));
// wrap <Routes> in <Suspense fallback={<Spinner/>}>
```

Result: the section's content + libs (e.g. KaTeX 263KB) become on-demand chunks,
loaded only when those routes are visited. Verify in the build output that the
main `index-*.js` size didn't grow and new named chunks appeared. Prerendered
HTML still has full content for SEO regardless of code-splitting.

## sitemap.xml

- List every public URL with `<loc>`, `<lastmod>`, `<changefreq>`, `<priority>`.
- Keep it current — it's the source of truth for prerender AND for IndexNow
  submission scripts.
- Exclude auth-gated/private routes.
- Reference it from robots.txt: `Sitemap: https://example.com/sitemap.xml`.
- Submit to Google Search Console + Bing Webmaster Tools.
- `priority`: home 1.0, key marketing/hubs 0.8–0.9, content pages 0.7.

## robots.txt + Content Signals (AI permissions)

Allow search engines + AI live-citation; block private routes; optionally refuse
training. Full skeleton:

```
User-agent: *
Content-Signal: search=yes,ai-input=yes,ai-train=no
Allow: /
Disallow: /editor/
Disallow: /projects
Disallow: /settings
Disallow: /admin/
Disallow: /auth/
Disallow: /api/

User-agent: Googlebot
Allow: /
User-agent: Bingbot
Allow: /

# AI live citation (allowed)
User-agent: OAI-SearchBot        # ChatGPT search
Content-Signal: search=yes,ai-input=yes
Allow: /
User-agent: ChatGPT-User
Content-Signal: ai-input=yes
Allow: /
User-agent: Claude-SearchBot
Content-Signal: search=yes,ai-input=yes
Allow: /
User-agent: Claude-User
Content-Signal: ai-input=yes
Allow: /
User-agent: PerplexityBot
Content-Signal: search=yes,ai-input=yes
Allow: /

# Training crawlers — allow citation, refuse training
User-agent: GPTBot
Content-Signal: ai-input=yes,ai-train=no
Allow: /
User-agent: ClaudeBot
Content-Signal: ai-input=yes,ai-train=no
Allow: /
User-agent: Google-Extended
Content-Signal: ai-input=yes,ai-train=no
Allow: /

# Aggressive scrapers — block
User-agent: CCBot
Disallow: /
User-agent: Bytespider
Disallow: /

Sitemap: https://example.com/sitemap.xml
```

## llms.txt (AI-readable summary)

Host `/llms.txt` — a Markdown brief for LLMs. Template:

```markdown
# brand.com

> One-line summary of what the product is and its key differentiator.

Longer paragraph: what it does, how it works, who it's for.

## Why brand vs competitor
- **Differentiator 1.** Explanation.
- **Differentiator 2.** Explanation.

## Key pages
- [Home](https://brand.com/): overview
- [Pricing](https://brand.com/pricing)
- [vs Competitor](https://brand.com/vs/competitor)
- [Templates/Docs/Learn](https://brand.com/learn)
```

Reference it in `<head>`:
`<link rel="alternate" type="text/markdown" href="/llms.txt" title="LLM-friendly summary" />`

## Core Web Vitals + speed (now an AI-citation factor)

- **FCP < 0.4s** correlates with ~3× ChatGPT citations. Speed is not just UX now.
- Watch INP (replaced FID), LCP, CLS.
- Levers: code-split, lazy-load images, preconnect to font/CDN origins, compress,
  cache static assets, avoid giant single bundles (split > 500KB chunks).
- Measure in PageSpeed Insights + Search Console Core Web Vitals report.

## Canonical, redirects, hygiene

- One canonical URL per page (absolute, consistent trailing slash).
- 301 (not 302) for permanent moves; avoid redirect chains.
- No duplicate content across URLs (e.g. don't have both `/x` and `/x/` indexable).
- HTTPS everywhere; HSTS.
- `<html lang="en">` set.
- Custom 404 that links back into the site.

## Per-page head checklist

- [ ] Unique `<title>` ≤ 60 chars
- [ ] Unique meta description
- [ ] Canonical (absolute)
- [ ] OG + Twitter tags + image
- [ ] JSON-LD (FAQPage and/or page-type schema) reflecting visible content
- [ ] Prerendered HTML contains all of the above without JS
- [ ] In sitemap.xml with current lastmod
