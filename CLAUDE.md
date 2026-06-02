# CLAUDE.md — seo-master

## What this repo is
seo-master is a reference skill for AI coding agents — a complete SEO + GEO + AEO
playbook for 2026 (multi-engine indexing, framework-aware implementation,
structured data, agent-backend map). Applied knowledge, not theory.

## Source of truth — edit only these
| File | Controls |
|------|----------|
| `skills/seo-master/SKILL.md` | All skill behavior, mental model, priority order, hard rules |
| `skills/seo-master/references/*.md` | Domain knowledge (engines, framework, schema, content, 2026 updates) |

## Agent-specific files (synced from source — don't edit directly)
`.cursor/` · `.windsurf/` · `.clinerules/` · `.codex/` · `.github/copilot-instructions.md`
`AGENTS.md` · `GEMINI.md` · `agents/openai.yaml` · `gemini-extension.json`

## Key constraints
- Verified facts only — every claim true and sourced; never copy a competitor's stat.
- Detect the framework before implementing (Next.js Metadata API ≠ React prerender).
- Never fabricate `aggregateRating`/reviews; FAQ schema requires a visible FAQ.
- Time-sensitive facts live in `references/whats-new-2026.md` — re-verify them over time.

## Making changes
Edit `skills/seo-master/SKILL.md` or the references. Keep agent rule files in sync
when behavior changes. Open a PR with a before/after example.
