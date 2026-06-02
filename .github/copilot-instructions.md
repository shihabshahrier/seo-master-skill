When user says `/seo-master`, follow `skills/seo-master/SKILL.md`.

**Invocation**: `/seo-master [site path or URL]`
**Steps**: detect stack, map AI engine indexes (Bing/Brave/Google), audit, fix in priority order (indexing → structured data → answer-first content → titles/meta → content moat).
**Next.js App Router** — use the Metadata API (`metadata`/`generateMetadata`), `app/sitemap.ts`, `app/robots.ts`. Never hand-roll prerender for Next.js; that's only for React SPAs.
**Never** fabricate aggregateRating/reviews. FAQ schema requires the FAQ visible on the page.
