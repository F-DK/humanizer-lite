# AGENTS.md

Guidance for AI coding agents (Claude Code, Codex, Warp, etc.) working in this repository.

## What this repo is

A portable agent skill implemented entirely as Markdown. The runtime artifacts are `SKILL.md` and the `references/` folder it links: the agent reads the frontmatter and prompt up front, and loads a reference only when the text calls for it. There is no build step, and the repo should avoid wording that limits support to one or two harnesses.

## Key files

- `SKILL.md` — the skill itself. Portable YAML frontmatter (`name`, `description`, `license`, `metadata.version`) followed by the rules the agent cannot derive: no fabrication, sterility as a tell, the dash ban, voice matching, and output modes. Keep it short; it is loaded on every invocation.
- `references/patterns.md` — the canonical, numbered pattern list (1-35) with watchlists and before/after examples. **This is the source of truth for pattern content.**
- `references/false-positives.md` — what looks like AI but is not, and the human signals over-editing destroys.
- `README.md` — for humans: installation, usage, a summary table of the patterns, and a version history.
- `.claude-plugin/plugin.json` — optional Claude Code plugin manifest.
- `.claude-plugin/marketplace.json` — optional single-repo marketplace entry so `/plugin marketplace add F-DK/humanizer-lite` works.
- `scripts/validate-package.py` — dependency-free package and synchronization checks used locally and in CI.

## The maintenance contract

`SKILL.md`, `references/`, and `README.md` must stay in sync. When you change behavior or content:

- **Patterns:** the skill currently defines **35 numbered patterns**, in `references/patterns.md`. If you add, remove, or renumber any, update the README pattern table, its pattern-count heading, and every cross-reference in the same change. Keep numbering stable unless you are deliberately renumbering.
- **Version:** `SKILL.md` frontmatter stores the version under `metadata.version`, `README.md` has a "Version History" section, and `.claude-plugin/plugin.json` has a `version` field. Bump them together so package metadata matches the skill. Keep the skill version under `metadata`; a top-level `version` key is not portable across Agent Skills hosts. (`marketplace.json` intentionally omits a version so `plugin.json` stays the package source of truth.)
- **Compatibility:** keep install and usage language harness-neutral. The skill should work in any agent harness that can load Markdown skill instructions; Claude Code, OpenCode, Codex, and other harnesses are examples, not limits.
- **Validation:** run `python3 scripts/validate-package.py`, `npx skills add . --list`, and `claude plugin validate .` before publishing.
- **Non-obvious fixes:** if you change the prompt to handle a tricky failure mode (a repeated mis-edit, an unexpected tone shift), add a short note to the README version history explaining what was fixed and why.

## Editing SKILL.md

- Preserve valid YAML frontmatter (formatting and indentation).
- The prompt below the frontmatter is the product. Edit it like a careful instruction document, not code.
- Keep it lean. Anything a competent model already knows belongs in `references/`, or nowhere. `SKILL.md` is for what it cannot derive.
