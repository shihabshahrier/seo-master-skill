# Structured Data (JSON-LD) — Ready-to-Use Schemas

Structured data is the highest-leverage AEO/GEO lever after content. **FAQPage
in particular: ~3.2× more likely to appear in Google AI Overviews, +28%
citation across AI platforms.** Emit JSON-LD in a `<script type="application/ld+json">`.
In React, use `react-helmet-async` to inject per-page; static HTML can inline it.

## Iron rules

1. **Visible-content rule.** Schema must reflect what's actually on the page.
   FAQPage → the Q&As must be visible. Review/rating → real reviews must be shown.
   Schema-only = Google policy violation → manual action.
2. **No fabricated ratings.** Never invent `aggregateRating`/`review`. This is the
   #1 mistake. If you have no real reviews, omit rating schema entirely. Use
   `featureList`, plain text claims, or `interactionStatistic` instead.
3. **One canonical entity, shared `@id`.** If the homepage and a page both
   describe the app, give both the same `@id` (e.g. `https://site/#app`) so
   engines dedupe instead of seeing conflicting duplicates.
4. **Use `@graph`** to bundle multiple schema objects on one page cleanly.

## Site-wide graph (put in the root index.html `<head>`)

```json
{
  "@context": "https://schema.org",
  "@graph": [
    {
      "@type": "Organization",
      "@id": "https://example.com/#organization",
      "name": "BrandName",
      "url": "https://example.com",
      "logo": "https://example.com/logo.svg",
      "sameAs": ["https://github.com/org", "https://twitter.com/handle"]
    },
    {
      "@type": "WebSite",
      "@id": "https://example.com/#website",
      "url": "https://example.com",
      "name": "BrandName",
      "publisher": { "@id": "https://example.com/#organization" },
      "potentialAction": {
        "@type": "SearchAction",
        "target": "https://example.com/search?q={search_term_string}",
        "query-input": "required name=search_term_string"
      }
    },
    {
      "@type": "SoftwareApplication",
      "@id": "https://example.com/#app",
      "name": "BrandName",
      "applicationCategory": "BusinessApplication",
      "operatingSystem": "Web",
      "url": "https://example.com",
      "description": "One-sentence factual description.",
      "offers": { "@type": "Offer", "price": "0", "priceCurrency": "USD" },
      "featureList": [
        "Feature one",
        "Feature two"
      ],
      "publisher": { "@id": "https://example.com/#organization" }
    }
  ]
}
```

Note: `aggregateRating` deliberately omitted. Add ONLY with real, visible reviews.

## FAQPage (use on almost every page)

Pair visible accordion + this schema. Answers should be self-contained
(134–167 words ideal) so an AI can quote one directly.

```json
{
  "@context": "https://schema.org",
  "@type": "FAQPage",
  "mainEntity": [
    {
      "@type": "Question",
      "name": "What is X?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "X is ... (direct, complete answer in the first sentence)."
      }
    }
  ]
}
```

React pattern (data-driven):
```tsx
const faqSchema = {
  "@context": "https://schema.org",
  "@type": "FAQPage",
  mainEntity: faqs.map(f => ({
    "@type": "Question",
    name: f.q,
    acceptedAnswer: { "@type": "Answer", text: f.a },
  })),
};
// <Helmet><script type="application/ld+json">{JSON.stringify(faqSchema)}</script></Helmet>
```

## HowTo (tutorials, fixes, step-by-step)

```json
{
  "@context": "https://schema.org",
  "@type": "HowTo",
  "name": "How to do X in Y",
  "totalTime": "PT10M",
  "step": [
    { "@type": "HowToStep", "position": 1, "name": "Step name", "text": "Do this." },
    { "@type": "HowToStep", "position": 2, "name": "Next", "text": "Then this." }
  ]
}
```

## Course (multi-lesson learning content)

```json
{
  "@context": "https://schema.org",
  "@type": "Course",
  "name": "The Complete X Course",
  "description": "...",
  "url": "https://example.com/course",
  "provider": { "@type": "Organization", "name": "Brand", "url": "https://example.com" },
  "isAccessibleForFree": true,
  "inLanguage": "en",
  "hasPart": [
    { "@type": "CreativeWork", "name": "Lesson 1: ...", "url": "https://example.com/course/lesson-1" }
  ]
}
```

Per-lesson page: `LearningResource` with `isPartOf` → the Course, plus a FAQPage
in an `@graph`.

```json
{
  "@type": "LearningResource",
  "name": "Lesson title",
  "educationalLevel": "Beginner",
  "learningResourceType": "Lesson",
  "isAccessibleForFree": true,
  "isPartOf": { "@type": "Course", "name": "The Complete X Course", "url": "https://example.com/course" }
}
```

## Article / blog post

```json
{
  "@context": "https://schema.org",
  "@type": "Article",
  "headline": "Title",
  "description": "Excerpt.",
  "author": { "@type": "Person", "name": "Author Name" },
  "publisher": {
    "@type": "Organization",
    "name": "Brand",
    "logo": { "@type": "ImageObject", "url": "https://example.com/logo.svg" }
  },
  "datePublished": "2026-06-01",
  "dateModified": "2026-06-01",
  "mainEntityOfPage": "https://example.com/blog/slug",
  "keywords": "kw1, kw2"
}
```
`author` can be `Organization` if no named person. Keep `dateModified` fresh —
freshness is a Perplexity/Bing signal.

## BreadcrumbList (every deep page)

```json
{
  "@type": "BreadcrumbList",
  "itemListElement": [
    { "@type": "ListItem", "position": 1, "name": "Templates", "item": "https://example.com/templates" },
    { "@type": "ListItem", "position": 2, "name": "Category", "item": "https://example.com/templates/cat" },
    { "@type": "ListItem", "position": 3, "name": "This page", "item": "https://example.com/templates/cat/slug" }
  ]
}
```

## ItemList (index/listing pages — hub, gallery, blog index)

```json
{
  "@type": "ItemList",
  "name": "All Guides",
  "itemListElement": [
    { "@type": "ListItem", "position": 1, "url": "https://example.com/guide-a", "name": "Guide A" }
  ]
}
```

## Product / CreativeWork (a downloadable asset, template, tool)

Prefer `CreativeWork` / `SoftwareSourceCode` for free assets; reserve `Product`
for things with real offers + (real) reviews.

```json
{
  "@type": "CreativeWork",
  "name": "Asset name",
  "learningResourceType": "template",
  "isAccessibleForFree": true,
  "inLanguage": "en",
  "publisher": { "@id": "https://example.com/#organization" },
  "url": "https://example.com/asset"
}
```

For code templates: `SoftwareSourceCode` with `programmingLanguage`, `license`,
`author`, an `offers` (price 0), and a `breadcrumb`.

## Validate

- Google Rich Results Test: search.google.com/test/rich-results
- Schema.org validator: validator.schema.org
- After deploy, check Search Console → Enhancements for schema errors.
