# Framework-Aware SEO — Next.js, React SPA, and others

SEO implementation differs by **render model**. The single most important rule:
**the served HTML must already contain title, meta, canonical, structured data,
and visible content — without running client JS.** Crawlers and most AI fetchers
(Brave, Bing, GPTBot, ClaudeBot) read the initial HTML; many do not execute JS.
How you achieve that depends on the stack.

## Step 0 — Detect the stack FIRST (always do this)

Check `package.json` dependencies and project files:

| Signal | Stack | SEO path |
|--------|-------|----------|
| `next` dep, `app/` dir | **Next.js App Router** | Metadata API (this file) |
| `next` dep, `pages/` dir | **Next.js Pages Router** | `next/head` + `next-sitemap` |
| `react` + `vite`/`react-scripts` + `react-router` | **React SPA** | helmet + prerender (`technical-seo.md`) |
| `remix`/`@remix-run` | **Remix** | route `meta()` export (SSR by default ✓) |
| `astro` | **Astro** | static by default ✓; `<head>` per page + `@astrojs/sitemap` |
| `gatsby` | **Gatsby** | `gatsby-plugin-react-helmet` + SSG ✓ |
| `vue`/`nuxt` | **Nuxt** | `useHead()`/`definePageMeta`, SSR ✓ |

Then read the matching section. **Do not assume React SPA** — most modern sites
are Next.js or another SSR/SSG framework where prerendering is built in.

---

## Next.js App Router (the common 2026 case)

The **Metadata API** is the cornerstone. `metadata` is resolved on the server
and injected into the initial HTML. `metadata`/`generateMetadata` work **only in
Server Components** (no `"use client"` at the top of the file that exports them).

### Static metadata (stable pages)

```tsx
// app/pricing/page.tsx
import type { Metadata } from "next";

export const metadata: Metadata = {
  title: "Pricing — Brand, Online Collaborative X Editor",
  description: "Answer-first sentence with the value prop. 140–160 chars.",
  alternates: { canonical: "https://example.com/pricing" },
  openGraph: {
    type: "website",
    url: "https://example.com/pricing",
    siteName: "Brand",
    title: "Pricing — Brand",
    description: "...",
    images: [{ url: "/og/pricing.png", width: 1200, height: 630 }],
  },
  twitter: { card: "summary_large_image", title: "Pricing — Brand", images: ["/og/pricing.png"] },
};
```

### Root layout — metadataBase + title template (brand suffix, done once)

```tsx
// app/layout.tsx
export const metadata: Metadata = {
  metadataBase: new URL("https://example.com"),
  title: {
    default: "Brand — Online Collaborative X Editor",
    template: "%s | Brand", // child pages set title:"How to Y" → "How to Y | Brand"
  },
  description: "Default brand description.",
  robots: { index: true, follow: true },
};
```

`template` gives every page the consistent `| Brand` suffix automatically — see
`titles-and-meta.md` for the suffix rule (keep page titles ≤60 chars including
the suffix).

### Dynamic metadata (data/param-driven pages)

```tsx
// app/learn/[slug]/page.tsx
export async function generateMetadata({ params }): Promise<Metadata> {
  const { slug } = await params;            // params is a Promise in Next 15
  const post = await getPost(slug);
  return {
    title: post.title,                       // → "...| Brand" via template
    description: post.excerpt,
    alternates: { canonical: `https://example.com/learn/${slug}` },
    openGraph: { title: post.title, description: post.excerpt, type: "article" },
  };
}
```

### Sitemap + robots (file conventions — no manual XML)

```ts
// app/sitemap.ts  → served at /sitemap.xml
import type { MetadataRoute } from "next";
export default async function sitemap(): Promise<MetadataRoute.Sitemap> {
  const posts = await getAllPosts();
  return [
    { url: "https://example.com", lastModified: new Date(), changeFrequency: "weekly", priority: 1 },
    ...posts.map(p => ({ url: `https://example.com/learn/${p.slug}`, lastModified: p.updatedAt, priority: 0.7 })),
  ];
}
```

```ts
// app/robots.ts  → served at /robots.txt
import type { MetadataRoute } from "next";
export default function robots(): MetadataRoute.Robots {
  return {
    rules: [
      { userAgent: "*", allow: "/", disallow: ["/api/", "/admin/", "/auth/"] },
    ],
    sitemap: "https://example.com/sitemap.xml",
  };
}
```

`robots.ts` cannot express per-bot `Content-Signal` lines — for the full AI-bot
allowlist (`OAI-SearchBot`, `Claude-SearchBot`, `PerplexityBot`, `GPTBot`…),
ship a **static `public/robots.txt`** instead (see `technical-seo.md`). Static
file in `public/` wins; use it when you need the AI directives.

### OG image, manifest, viewport

```tsx
// app/opengraph-image.tsx — dynamic OG image at the edge (or drop a static public/og.png)
import { ImageResponse } from "next/og";
export const size = { width: 1200, height: 630 };
export const contentType = "image/png";
export default function Image() { return new ImageResponse(<div style={{fontSize:64}}>Brand</div>, size); }
```

```ts
// app/manifest.ts and the viewport export (viewport moved OUT of metadata in Next 15)
export const viewport = { width: "device-width", initialScale: 1, themeColor: "#000" };
```

### JSON-LD structured data

No special API — render a `<script>` in a Server Component. Build the object in
JS, stringify it. (Schemas: see `structured-data.md`.)

```tsx
export default function Page() {
  const ld = { "@context": "https://schema.org", "@type": "FAQPage", mainEntity: faqs.map(/*…*/) };
  return (
    <>
      <script type="application/ld+json" dangerouslySetInnerHTML={{ __html: JSON.stringify(ld) }} />
      {/* visible FAQ must also render — schema-only = policy risk */}
    </>
  );
}
```

### Rendering mode — pick so HTML is prebuilt

| Mode | How | SEO |
|------|-----|-----|
| **SSG** (default) | static `page.tsx`, no dynamic APIs | ✅ best — HTML at build |
| **ISR** | `export const revalidate = 3600` | ✅ great for content that updates |
| **SSR** | `export const dynamic = "force-dynamic"` | ✅ HTML per request (slower TTFB) |
| **PPR** (Next 15) | `experimental.ppr` | ✅ static shell + streamed dynamic |
| **Client only** | `"use client"` whole route, fetch in `useEffect` | ❌ avoid — empty HTML, no metadata |

Rule: **content/marketing routes = static or ISR.** Reserve SSR/dynamic for
auth-gated app routes (which you `noindex` anyway). Verify with
`curl -s https://site/page | grep -i '<title>\|application/ld+json'` — the tags
must be present in raw HTML.

### Pages Router (`pages/` dir)

- Per-page tags via `next/head`; or `_document.tsx` for site-wide.
- `getStaticProps`/`getServerSideProps` to put data in HTML.
- Sitemap/robots: use the **`next-sitemap`** package (postbuild script).

---

## React SPA (Vite / CRA + React Router) — no SSR

A client-rendered SPA ships an empty `<div id="root">` — crawlers/AI see nothing.
Two-part fix, both required:

1. **`react-helmet-async`** for per-page title/meta/OG/JSON-LD (a reusable
   `<SEO>` component — see `titles-and-meta.md`).
2. **Prerender every public route to static HTML at build** (Vite + Puppeteer) so
   the served file already has helmet's tags + content. Full pattern in
   `technical-seo.md`.

If you can move the project to Next.js/Astro/Remix, that removes the prerender
machinery entirely — recommend it when the SPA is mostly content pages.

---

## Universal checks (any framework)

- [ ] `curl` the page → raw HTML contains unique `<title>`, meta description,
      canonical, OG, and JSON-LD **without JS**.
- [ ] One canonical per URL, absolute, consistent trailing slash.
- [ ] `sitemap.xml` + `robots.txt` reachable at root; sitemap referenced in robots.
- [ ] App/auth routes `noindex` (don't waste crawl budget or leak private pages).
- [ ] Core Web Vitals: measure **INP** (replaced FID), LCP, CLS. Next: use
      `next/image`, `next/font`, dynamic `import()` for heavy client components.
