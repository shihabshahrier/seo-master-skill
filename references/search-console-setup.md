# Search Console + Webmaster Tools + IndexNow Setup

Three separate systems. Google for Gemini/AI Overviews reach; Bing for
ChatGPT/Copilot reach; IndexNow for instant Bing/Brave/Yandex indexing. Do all
three — they cover different AI engines.

## 1. Google Search Console (Gemini, AI Overviews, Google)

URL: https://search.google.com/search-console

Steps:
1. Add property. Prefer **Domain property** (covers all subdomains + http/https) —
   requires a DNS TXT record. Or **URL-prefix** (verify via HTML tag/file/GA).
2. Verify ownership (DNS TXT is most robust).
3. **Submit sitemap**: Sitemaps → enter `sitemap.xml` → Submit.
4. Use **URL Inspection** to request indexing for key new pages immediately.
5. Monitor:
   - **Performance**: queries, impressions, CTR, position.
   - **Pages** (Coverage): indexed vs excluded; fix "Discovered – not indexed".
   - **Enhancements**: structured-data validity (FAQ, Breadcrumb, etc.).
   - **Core Web Vitals**: LCP/INP/CLS per page.
6. Google does **not** support IndexNow — rely on crawl + sitemap + URL
   Inspection here.

## 2. Bing Webmaster Tools (ChatGPT, Copilot, Bing)

URL: https://www.bing.com/webmasters

**Why it matters most for AI:** ChatGPT grounds on Bing (~87% citation overlap).
Most competitors ignore Bing — easy edge.

Steps:
1. Sign in; add your site. Fast path: **import from Google Search Console**
   (one click, copies verification + sitemaps).
2. Or verify via XML file, meta tag, or CNAME.
3. **Submit sitemap**: Sitemaps → Submit `https://example.com/sitemap.xml`.
4. Enable / watch the **AI Performance report** (public preview from Feb 2026):
   shows when your URLs are cited in Copilot / Bing AI answers — the only direct
   window into AI citations. Track which URLs get cited over time.
5. Use **URL Inspection** + **Submit URLs** for immediate indexing.
6. Bing ranking signals for AI answers: relevance, technical credibility, and
   **extractability** (clear headings, tables, FAQ, schema). Same content moves
   that help AEO help Bing AI.

## 3. IndexNow (instant Bing + Yandex + DuckDuckGo + Naver + Seznam)

Notifies participating engines the moment content changes → indexed in hours not
days. **Does NOT include Google.** Feeds ChatGPT (Bing) fast.

### Option A — Cloudflare Crawler Hints (easiest, zero code)
If the site is proxied through Cloudflare (orange-cloud DNS):
Dashboard → site → **Speed → Optimization → Crawler Hints → Enable**.
Cloudflare auto-submits IndexNow pings when cache shows content changed.

### Option B — Key file + submit script (works anywhere, CI-friendly)

1. Generate a key (32 hex chars):
   `tr -dc 'a-f0-9' < /dev/urandom | head -c 32`
2. Host it at the site root as `<key>.txt` containing exactly the key:
   `public/<key>.txt` → served at `https://example.com/<key>.txt`
   (proves domain ownership).
3. Submit script (`scripts/indexnow.mjs`) — reads sitemap, POSTs all URLs:

```js
import { readFileSync } from 'fs';
const HOST = 'example.com';
const KEY = '<key>';
const xml = readFileSync('public/sitemap.xml', 'utf-8');
const urlList = [...xml.matchAll(/<loc>(https:\/\/example\.com\/[^<]*)<\/loc>/g)].map(m => m[1]);
const res = await fetch('https://api.indexnow.org/IndexNow', {
  method: 'POST',
  headers: { 'Content-Type': 'application/json; charset=utf-8' },
  body: JSON.stringify({ host: HOST, key: KEY, keyLocation: `https://${HOST}/${KEY}.txt`, urlList }),
});
console.log(res.status); // 200/202 = accepted
```
4. Wire `"indexnow": "node scripts/indexnow.mjs"`; run after deploys with
   meaningful content changes.

**Etiquette:** don't resubmit unchanged URLs repeatedly; wait ≥5 min between
updates to the same URL. Submit on real content changes only.

## 4. Order of operations after a deploy

1. Confirm deploy succeeded + new routes render live (view-source shows meta +
   content from prerender).
2. `npm run indexnow` (or rely on Cloudflare Crawler Hints).
3. In Google Search Console: submit/refresh sitemap; URL-inspect key new pages.
4. In Bing Webmaster: submit/refresh sitemap; submit key URLs.
5. Days later: check Bing AI Performance report + GSC Performance for pickup.
6. Periodically ask ChatGPT/Claude/Perplexity the target queries ("best X",
   "X alternative", "how to Y in X") — track whether you're cited.

## Other engines (optional)
- **Yandex Webmaster** (if relevant geos) — also gets IndexNow.
- **DuckDuckGo** uses Bing's index — Bing setup covers it.
- **Brave** (Claude's index) has no public webmaster console; ensure crawlable
  + allowed in robots.txt and it will index organically.
