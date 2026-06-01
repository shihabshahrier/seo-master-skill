# Titles, Meta, and Head Tags

## Title tag — the formula

`<Primary keyword / value prop> — <Brand>, <Descriptor>`  (≤ 60 chars)

- **Value/keyword first.** Lead with what the user searches, not the brand.
- **Brand + descriptor suffix**, consistent across the site (this is what
  Overleaf does: every page ends `— Overleaf, Online LaTeX Editor`). Consistency
  builds the brand-as-entity signal AI engines reward.
- **≤ 60 characters** or Google truncates with "…". Count it.

### The brand-suffix decision (important nuance)

Two title styles, used deliberately:

1. **Marketing/top pages** (home, about, pricing, compare): full descriptor.
   `About Us — Brand, Online Collaborative X Editor`
2. **Content pages** (guides, tutorials, error fixes, deep articles): the full
   descriptor blows past 60 chars. Use a short suffix instead:
   `How to Add Citations and a Bibliography in LaTeX | LetX`  (54 chars ✓)
   `How to Add Citations ... — LetX, Online Collaborative LaTeX Editor`  (~85 ✗ truncated)

Rule: **keyword-rich + `| Brand`** on content pages; **full descriptor** on a
handful of marketing pages. Never leave a page with a bare `— Brand` and no
descriptor (inconsistent), and never let a title exceed 60.

### Add the year for freshness queries

"... 2026" in title/description/URL slug increases CTR and AI-citation
likelihood for time-sensitive topics ("best X 2026").

### Question-format titles for AEO

For pages answering a question, put the question in the title:
`What Is X? — Brand` or `How to Fix Y in Z | Brand`. Matches how people and AI
phrase queries.

## Meta description

- 140–160 chars. Not a ranking factor for position, but drives CTR and is often
  what AI snippets/previews pull.
- **Lead with the answer/value.** First sentence should stand alone.
- Include the primary keyword naturally, once.
- Each page unique. No duplicates.

## Canonical

- Every page: `<link rel="canonical" href="https://example.com/exact-url" />`
- Absolute URL, no trailing-slash inconsistency, no query params for the
  canonical version.

## Open Graph + Twitter (social/preview + some AI previews)

```html
<meta property="og:type" content="website" />
<meta property="og:site_name" content="Brand" />
<meta property="og:title" content="..." />
<meta property="og:description" content="..." />
<meta property="og:url" content="https://example.com/page" />
<meta property="og:image" content="https://example.com/og-image.png" />
<meta name="twitter:card" content="summary_large_image" />
<meta name="twitter:title" content="..." />
<meta name="twitter:description" content="..." />
<meta name="twitter:image" content="https://example.com/og-image.png" />
```
OG image ideally 1200×630 PNG. SVG works for logo fallback but raster is safer.

## React (SPA) implementation

Use `react-helmet-async`. A reusable `<SEO>` component takes `title`,
`description`, `url`, `image`, `keywords`, `schema` props and emits all of the
above + JSON-LD. Pass a unique title/description per page. Default the
description to a sensible brand sentence.

```tsx
<SEO
  title="How to Make a Table in LaTeX | LetX"
  description="Create tables in LaTeX with the tabular environment..."
  url="https://letx.app/learn/how-to-make-a-table-in-latex"
  schema={faqSchema}
/>
```

## Audit checklist for titles/meta

- [ ] Every public page has a unique `<title>` ≤ 60 chars
- [ ] Consistent suffix pattern (full descriptor on marketing, `| Brand` on content)
- [ ] No page with bare `— Brand` and no descriptor
- [ ] Unique meta description 140–160 chars, answer-first
- [ ] Canonical on every page, absolute URL
- [ ] OG + Twitter tags present, image set
- [ ] No fabricated claims in any of them
