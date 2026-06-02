# Contributing

Thanks for improving seo-master.

## Ground rules
- **Verified facts only.** Every tactic, number, or claim needs an official-docs or
  primary-source backing. No unverified blog assertions.
- **Edit the source**, not the synced copies. Source of truth:
  `skills/seo-master/SKILL.md` and `skills/seo-master/references/*.md`.
  Don't edit `.cursor/`, `.windsurf/`, `.clinerules/`, `.codex/`, `AGENTS.md`,
  `GEMINI.md`, `.github/copilot-instructions.md` directly — update them to match
  when you change behavior.
- **Keep SKILL.md lean** (under ~5000 tokens). Push detail into a reference file.
- **Time-sensitive facts** go in `references/whats-new-2026.md` with the date and
  source, so they're easy to re-verify later.

## Workflow
1. Make the change in the source files.
2. If behavior changed, sync the agent rule files.
3. Open a PR describing the change with a before/after example and a source link.

## Reporting
Use the issue templates: bug report (wrong/outdated) or feature request (new
engine, framework, schema, or tactic).
