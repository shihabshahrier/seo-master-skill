# SEO Master Skill

A portable, agent-readable **SEO + GEO + AEO** playbook for 2026 — traditional
search ranking, how AI answer engines **and coding agents** (ChatGPT, Claude,
Gemini, Perplexity, Copilot, Claude Code, Codex, OpenClaw, Hermes)
discover/rank/cite pages, **framework-aware implementation (Next.js App Router +
React SPA)**, structured data, technical SEO, titles/meta, content strategy,
IndexNow, Google + Bing Search Console, competitor analysis, the 2026 I/O +
core-update changes, and a prioritized implementation checklist.

Distilled from real work optimizing a SaaS against AI-native competitors —
applied knowledge, not theory. Works in any agent that implements the Agent
Skills open standard.

```
seo-master-skill/
  skills/seo-master/
    SKILL.md                 ← entry point (frontmatter)
    references/*.md          ← 10 lazy-loaded domain files
  install.sh                 ← install to all agents
  .claude-plugin/            ← Claude Code plugin + marketplace
  .cursor/ .windsurf/ .clinerules/ .codex/ .github/
  AGENTS.md GEMINI.md CLAUDE.md
```

## Install

```bash
bash install.sh
```

Installs to Claude Code, Codex, opencode, Gemini CLI, and OpenClaw. Then invoke:

```
/seo-master [site path or URL]
```

### Manual install (per agent)

| Agent | Path |
|-------|------|
| Claude Code | `~/.claude/skills/seo-master/` |
| Codex / Amp / Goose / Kiro | `~/.agents/skills/seo-master/` |
| opencode | `~/.config/opencode/skills/seo-master/` (also reads `~/.claude/skills/`) |
| Gemini CLI | `~/.gemini/antigravity/skills/seo-master/` |
| OpenClaw | `~/.openclaw/workspace/skills/seo-master/` |

```bash
mkdir -p ~/.claude/skills/seo-master
cp -r skills/seo-master/. ~/.claude/skills/seo-master/
```

## Usage

| Arg | Default | Meaning |
|-----|---------|---------|
| site path or URL | current project | what to audit/optimize |

The agent reads `SKILL.md` first, then pulls `references/*.md` as needed.

## Contents

- **skills/seo-master/SKILL.md** — entry point: mental model, priority order, hard rules.
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

## How it works

1. **Detect the stack** (Next.js App/Pages Router, React SPA, Astro, Remix…) — implementation differs by render model.
2. **Map AI engine/agent → index** (Bing/Brave/Google) so you optimize the right place.
3. **Audit**, then fix in priority order: indexing reach → structured data → answer-first content → titles/meta → content moat → off-site.

## Agent support

| Agent | Mechanism |
|-------|-----------|
| Claude Code | `skills/` + `.claude-plugin/` |
| Codex | `agents/openai.yaml` + `.codex/` |
| Cursor | `.cursor/rules/seo-master.mdc` |
| Windsurf | `.windsurf/rules/seo-master.md` |
| Cline | `.clinerules/seo-master.md` |
| Copilot | `.github/copilot-instructions.md` |
| Gemini CLI | `GEMINI.md` + `gemini-extension.json` |
| opencode / OpenClaw / Amp / Goose | `AGENTS.md` + `skills/` |

## Top 3 things people get wrong

1. Optimizing only for Google — invisible to ChatGPT (Bing) and Claude / coding agents (Brave).
2. Fabricating ratings/reviews in schema — Google policy violation → manual action.
3. Hand-rolling prerender on a Next.js site — it already has the Metadata API + SSG/ISR.

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md). Verified facts only; edit the source in
`skills/seo-master/`, not the synced agent files. MIT licensed.
