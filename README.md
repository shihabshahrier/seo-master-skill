# SEO Master Skill

A portable, agent-readable SEO + GEO + AEO playbook for 2026 — covering
traditional search ranking, how AI answer engines and coding agents (ChatGPT,
Claude, Gemini, Perplexity, Copilot, Claude Code, Codex, OpenClaw, Hermes)
discover/rank/cite pages, framework-aware implementation (Next.js App Router +
React SPA), structured data, technical SEO, titles/meta, content strategy,
IndexNow, Google + Bing Search Console, competitor analysis, the 2026 I/O + core-
update changes, and a prioritized implementation checklist.

Distilled from real work optimizing a SaaS against AI-native competitors — applied
knowledge, not theory.

## Use as a Claude Code / agent skill

`SKILL.md` is the entry point (has the skill frontmatter). Point your agent at
this directory, or drop it in your skills folder. The agent reads `SKILL.md`
first, then pulls `references/*.md` as needed.

## Contents

- **SKILL.md** — entry point: mental model, priority order, hard rules.
- **references/ai-engines-and-indexes.md** — which index each AI + coding agent (Claude Code/Codex/OpenClaw → Brave; ChatGPT → Bing; Gemini → Google) uses; per-engine tactics. *Read first.*
- **references/framework-seo.md** — detect the stack; Next.js App Router Metadata API + Pages Router + React SPA prerender.
- **references/whats-new-2026.md** — Google I/O 2026, May core update, "AEO/GEO still SEO", llms.txt contested.
- **references/structured-data.md** — every JSON-LD schema, ready to paste.
- **references/titles-and-meta.md** — title formula, brand-suffix decision, meta/OG.
- **references/content-and-aeo.md** — answer-first writing, fact density, content hubs, error-fix play, courses.
- **references/technical-seo.md** — prerendering, sitemap, robots+Content-Signals, llms.txt, code-splitting, Core Web Vitals.
- **references/search-console-setup.md** — Google + Bing + IndexNow, step by step.
- **references/competitor-and-positioning.md** — research method, finding the open lane, off-site listicles.
- **references/implementation-checklist.md** — prioritized, copy-paste audit + execution.

## Top 3 things people get wrong

1. Optimizing only for Google — invisible to ChatGPT (Bing index) and Claude (Brave index).
2. Fabricating ratings/reviews in schema — Google policy violation → manual action.
3. Schema-only FAQ with no visible FAQ on the page — also a policy risk.
