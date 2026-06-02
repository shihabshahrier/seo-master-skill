# AI Engines, Their Search Indexes, and How Each Cites (2026)

**This is the most important file in the skill.** Each AI answer engine grounds
its web search on a *different* index. If you only optimize for Google, you are
invisible to ChatGPT (Bing) and Claude (Brave). "Get indexed" is several tasks.

## The grounding map

| AI engine | Grounds on / search index | Implication |
|-----------|---------------------------|-------------|
| **ChatGPT** (search) + **Microsoft Copilot** | **Bing** — ~87% overlap between Bing top-10 and ChatGPT citations | Rank in Bing + wire IndexNow → ChatGPT can cite you |
| **Claude** (web search) | **Brave Search** (independent crawler) + direct open-web fetch | Be crawlable by Brave; allow Claude bots in robots.txt |
| **Gemini** + **Google AI Overviews / AI Mode** | **Google** native index | Classic Google SEO + E-E-A-T |
| **Perplexity** | Its **own** crawl/index; favors Reddit + content < 90 days old | Freshness + community presence (Reddit) matter |
| **Brave Search** | Own independent crawler/index | One of the few true independent indexes (with Google, Yandex) |

Takeaway: **Bing and Brave indexing matter as much as Google now** because they
feed ChatGPT and Claude. Most sites neglect Bing — that's the opportunity.

(I/O 2026: Google AI Mode now defaults to **Gemini 3.5 Flash**, 1B+ monthly
users, queries doubling each quarter — see `whats-new-2026.md`.)

## Coding agents ground mostly on Brave (the developer-audience lane)

If your audience is developers, the **coding agents** are a distinct, growing
referral surface — and most of them search through the **Brave Search API**, not
Google.

| Agent | Web-search backend | Note |
|-------|--------------------|------|
| **Claude Code** | Brave Search (Anthropic web search / Brave skill) + open-web fetch | terminal coding agent |
| **Codex** (OpenAI CLI) | Brave Search via the Agent Skills standard | also reaches Bing-grounded GPT models |
| **OpenClaw** | Brave Search skill | 345k★ open agent |
| **Hermes Agent** (Nous) | Brave Search skill | MIT, self-improving agent |
| **Cursor / Windsurf / Gemini CLI / Copilot** | Brave Search skill (Gemini CLI also Google) | IDE/CLI agents |

The common thread: Brave ships an official **brave-search-skills** package that
plugs into Claude Code, Codex, Cursor, Gemini CLI, Copilot, Windsurf, OpenClaw,
and others via the Agent Skills open standard. So **being well-indexed and
cleanly crawlable by Brave makes you discoverable across nearly the entire
coding-agent ecosystem at once.**

Implication: for dev tools / technical products, **Brave crawlability is
high-leverage** — clean server-rendered HTML, allowed UA in robots.txt, dense
structured docs. One index (Brave) ≈ the whole agent fleet.

## How each platform cites (2026 observed patterns)

- **ChatGPT**: heavily cites Wikipedia (~48% of factual citations), news, and
  educational/high-authority sources. **Page speed matters**: pages with First
  Contentful Paint < 0.4s average ~6.7 citations; > 1.13s drop to ~2.1. Fast
  pages are ~3× more likely to be cited.
- **Perplexity**: ~47% of top sources are Reddit; strong preference for content
  published in the last 90 days. Freshness is a real ranking signal.
- **Claude**: prefers content that is **highly structured, clearly sourced, and
  densely informative**. Clean heading hierarchy + schema win.
- **Google AI Overviews**: 96% of citations come from sources with strong
  E-E-A-T; pages with 15+ recognized named entities show ~4.8× higher selection;
  FAQPage schema → ~3.2× more likely to appear; content scoring high on
  "semantic completeness" (fully answers the query in a self-contained passage)
  is ~4.2× more likely to be cited. Multimodal (text+image+video+schema) → much
  higher selection.

## Per-engine tactics

### Win ChatGPT / Copilot (via Bing)
- Submit sitemap to **Bing Webmaster Tools**; enable its **AI Performance report**
  (shows when you're cited in Copilot/Bing AI answers).
- Wire **IndexNow** (instant Bing indexing on content change).
- Optimize page speed (FCP) — directly tied to citation rate.
- Clear headings, tables, FAQ sections — "extractable" facts.

### Win Claude (via Brave)
- Ensure Brave can crawl: don't block its UA; keep clean server-rendered HTML.
- In robots.txt, explicitly allow `Claude-User`, `Claude-SearchBot`, and
  (for citations) `ClaudeBot` with `ai-input=yes`.
- Dense, well-structured, sourced content. Claude rewards structure.

### Win Gemini / Google AI Overviews
- Classic Google SEO + real E-E-A-T (author bylines, credentials, sources).
- 15+ named entities per key page (proper nouns: products, orgs, standards).
- FAQPage schema + answer-first 134–167-word passages.

### Win Perplexity
- Freshness: keep `dateModified` current; publish/update regularly.
- Be present on Reddit (genuine, rule-respecting participation in relevant subs).
- Its own index rewards recent, citable, factual content.

## The robots.txt AI-bot allowlist (Content Signals)

Modern best practice — declare per-bot permission with the Content-Signal
standard (contentsignals.org). Allow search + AI live-citation, optionally
disallow training. Example skeleton (full version in `technical-seo.md`):

```
User-agent: *
Content-Signal: search=yes,ai-input=yes,ai-train=no
Allow: /

User-agent: OAI-SearchBot      # ChatGPT search index
Content-Signal: search=yes,ai-input=yes
Allow: /

User-agent: Claude-SearchBot   # Claude
Content-Signal: search=yes,ai-input=yes
Allow: /

User-agent: PerplexityBot
Content-Signal: search=yes,ai-input=yes
Allow: /

User-agent: GPTBot             # OpenAI training — allow citation, refuse train
Content-Signal: ai-input=yes,ai-train=no
Allow: /
```

`ai-input=yes` lets them cite you live; `ai-train=no` signals "don't train on my
content." Bots that ignore signals (CCBot, Bytespider) can be hard-disallowed.

## llms.txt — the AI-readable site summary

Host `/llms.txt` at the site root: a Markdown summary of what the site is, key
URLs, and differentiators, written for LLMs. **Optional, low priority** — no
major engine confirms it as a grounding/ranking input, and Google says it "isn't
needed for AI Search" (useful mainly for docs). Add it if cheap; never prioritize
it over content, schema, or indexing reach. See `whats-new-2026.md` for the full
contested stance and `technical-seo.md` for the template.
