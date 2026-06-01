# Competitor Research + Brand Positioning + Off-Site

SEO is downstream of positioning. If you fight the market leader head-on on
their strength, you lose. Find the open lane, then optimize for it.

## How to research competitors (the method)

1. **Find them via the queries you want.** Search "best <category>", "<leader>
   alternative", "<category> 2026". Note who appears — especially the listicles.
2. **Fetch each competitor's homepage + features + pricing.** Pull exact
   taglines, positioning words, pricing, feature claims, whether they have AI.
3. **Read the "alternatives" listicles.** These are what AI engines cite when
   asked "best X alternative". Note: (a) who's listed, (b) how each is
   positioned, (c) the selection criteria, (d) **whether you're listed** (often
   not — that's a gap to fix).
4. **Find each competitor's weaknesses** from reviews/criticism (speed,
   pricing, privacy, lock-in, missing features). These are your wedges.
5. **Identify the leader's SEO moat** — usually a content/learn hub with huge
   keyword coverage. You can't match it fast, but you can win specific long-tail.

## Finding the open lane (positioning)

- Map each competitor to the axis they own. (e.g. leader owns "online X editor";
  AI-native players own "AI X editor".)
- **Don't fight on a contested/owned axis.** If a giant (e.g. OpenAI-owned
  product) owns "free + AI", do not pitch "free + AI".
- Pick 2–3 axes nobody is fighting on that are true for you. Real example:
  **speed** (verifiable number), **privacy/independence** (not owned by an AI
  vendor; no training on user data), **regional/niche templates** (long-tail
  the giants ignore).
- Write the positioning as one sentence and thread it through hero, comparison
  pages, FAQs, and llms.txt consistently.

## Comparison pages (vs-Competitor) — get cited for "X alternative"

Build a `/vs/<competitor>` page per major rival. When users ask AI "best X
alternative" / "<competitor> alternative", these pages are what gets cited.

Structure (data-driven, one component, per-competitor config):
- **Answer-first hero**: "<Brand> is a [wedge] alternative to <Competitor>.
  [Concrete differentiators in first 40–60 words]."
- **Comparison table**: feature rows, highlight your wins (speed, price, privacy).
- **Wedge callout**: your single strongest differentiator vs *this* competitor.
- **Visible FAQ + FAQPage schema**: "What is the best alternative to <Competitor>?"
  with an answer naming your product — direct citation bait.
- CTA into the product.

Keep claims true and comparative-fair. Note competitors' real current pricing
(don't use stale numbers).

## Off-site: get listed where AI cites (highest off-site ROI)

AI engines cite "best X" / "X alternatives" listicles heavily. If you're absent
from them, you're invisible to that query class. Get listed:

- **AlternativeTo** (alternativeto.net) — add the product, list it as an
  alternative to the leader + the AI-native players. Tag features.
- **Product Hunt** — launch with the wedge tagline (≤60 chars) + a maker comment
  explaining the differentiator.
- **Reddit** (relevant subs) — genuinely helpful answers when people ask for
  alternatives; respect each sub's self-promo rules. (Perplexity heavily cites
  Reddit.)
- **G2 / Capterra / SaaSHub / Slant** — submit the product; answer "best X"
  questions.
- **Awesome-* GitHub lists** — PR a one-line entry.
- **Wikipedia "Comparison of X" tables** — only with independent reliable
  sources; unsourced rows get reverted. Get press/reviews first.
- **.edu / niche directories** — strong backlinks + E-E-A-T (e.g. university
  library guides for academic tools).

Keep one positioning doc (a `SEO_SUBMISSIONS.md`) with paste-ready copy for each
platform so submissions stay consistent. This is a **manual, human step**
(account creation, community rules) — code can't do it.

## Tracking
- Bing Webmaster AI Performance report → ChatGPT/Copilot citations.
- Google Search Console Performance → Gemini/Overviews + classic.
- Manually query ChatGPT/Claude/Perplexity the target prompts monthly; log
  whether/where you appear and which sources they cite (mirror those sources).
