# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

A single-page curated "awesome list of awesome AI lists." The entire deliverable is `README.md` — there is no build, no tests, no application code. Changes are almost always edits to that one file (adding/removing/moving table rows, renaming categories, fixing badges).

## Curation rules (enforced by CodeRabbit)

The rules in `.coderabbit.yaml` will be applied to every PR. Follow them when editing `README.md` so reviews don't bounce:

- **Table schema (strict):** every category is a 4-column Markdown table with header `| Resource | Description | Stars | Last Commit |`.
- **Badges (exact format):**
  - Stars: `![stars](https://img.shields.io/github/stars/owner/repo?style=flat)`
  - Last commit: `![last commit](https://img.shields.io/github/last-commit/owner/repo?style=flat)`
- **Categories alphabetical** in both the Table of Contents and the body. Keep them in sync.
- **Within a category**, insert new rows in descending star-count order when stars are visible.
- **Bilingual tone:** existing entries mix English and Chinese descriptions. Match the surrounding style — concise, professional, no marketing fluff.
- A row that names a repo must link to `https://github.com/owner/repo` (the weekly audit keys off this exact pattern — see below).
- Before updating an existing row, check the repository's current GitHub page for renames, redirects, and material changes in scope. Do not rewrite an accurate description merely for style.
- When a repository moves or is renamed, update the resource link and both shields.io badge paths together to the canonical `owner/repo`.

## Category boundaries

- **AI Coding & Agents:** AI coding tools, agent instructions/configuration, project rules, and design context intended for coding agents.
- **AI Directories:** broad collections spanning multiple AI domains, tools, projects, or learning resources.
- **LLMs & Platforms:** resources centered on specific models, model families, or AI product ecosystems.
- **Prompt Engineering:** material that teaches how to design and improve prompts, including techniques, patterns, research, and tooling.
- **Prompt Libraries:** ready-to-use prompts, examples, templates, and prompt managers. In short: Prompt Engineering teaches the method; Prompt Libraries provides reusable prompts.

## Weekly audit workflow

`.github/workflows/weekly-audit.yml` runs Sundays 16:00 UTC (also manually dispatchable) and opens a PR labeled `automated-audit` + `ai-curation` on branch `weekly-audit-pr`:

- Extracts every `(https://github.com/owner/repo)` link from `README.md`, queries the GitHub API for each.
- **404/451 → row deleted** via `sed` matching `github.com/${repo}[^a-zA-Z0-9_-]`. If you add a link in a format that doesn't match this regex (e.g. with a trailing path segment, or wrapped differently), the audit can't clean it up later.
- **Archived repos** are kept but flagged in the PR body.
- Updates the `_Last audited: <timestamp>_` footer in place.

After a comprehensive manual audit, update the `_Last audited` footer using its existing format.

When reviewing/editing an `automated-audit` PR, CodeRabbit is configured to do a broader health check (low-activity entries, miscategorizations, broken badges) — expect comments beyond the diff.

## Conventions worth knowing

- Feature branches per addition (e.g. `add-awesome-opensource-ai`); the audit owns `weekly-audit-pr`.
- Commit messages follow Conventional Commits (for example, `docs: reorganize categories and refresh resources`). Use `docs:` for README and repository-guidance changes unless another existing type is more accurate.
- License/PR/Twitter shields and `[x-url]`-style reference definitions live at the bottom of `README.md` — preserve them when editing the footer.
- `.idea/` is gitignored; no other tooling state should be committed.
