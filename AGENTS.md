# Agent guidelines

Instructions for AI coding agents (Claude Code, Copilot, Cursor, etc.) working in this repo.

## Project overview

`redisearch-shared-reports` is a GitHub Pages site that hosts sharable HTML performance and regression reports produced by the RediSearch performance team. Each report is a single, fully self-contained HTML file (inline CSS, no external dependencies) that can be viewed offline or shared as a direct link. The landing page (`README.md`) acts as an index; GitHub Pages auto-deploys on every push to `main`. Reports are generated externally (by benchmark pipelines or manual analysis) and committed here so the team can share stable, unfurlable URLs in Slack, Jira, and PRs.

## Local setup

No build step, no package manager, no test runner — this is a static file collection.

```bash
git clone git@github.com:redis-performance/redisearch-shared-reports.git
cd redisearch-shared-reports
# Open any .html file directly in a browser to review it locally.
```

## Branch naming

Same as human contributors: `<type>/<short-description>` (e.g. `fix/off-by-one-in-pipeline`).

## Coding standards

- Match the style already in the file you are editing.
- Prefer clear, minimal changes over large refactors unless explicitly asked.
- Do not add comments that describe *what* the code does — only add comments when the *why* is non-obvious.
- Do not introduce new dependencies without checking with the maintainer.
- All HTML reports must be self-contained: inline CSS only, no external stylesheets, no JavaScript.
- Every HTML report must carry the required Open Graph / Twitter Card meta tags — see `CLAUDE.md` for the exact tag set, emoji-color rules, and character-limit guidance.
- Every short SHA that appears in a report must be linked to its GitHub commit — see `CLAUDE.md` for the URL pattern and anchor template.
- Filenames follow `YYYY-MM-DD-<short-slug>.<ext>`; do not deviate from this pattern.

## Running tests

There is no automated test suite. Before declaring a task complete:

1. Verify the HTML file opens and renders correctly in a browser.
2. Confirm all short SHAs are linked to the correct GitHub commit.
3. Confirm Open Graph meta tags are present with a description ≤ 300 chars.
4. Confirm a `README.md` index row (newest-first) was added or updated.

```bash
# Quick sanity check — list all HTML files to confirm filename pattern
ls *.html
```

## How to submit changes

1. Create a branch: `git checkout -b <type>/<description>`.
2. Commit with a clear message focused on *why*, not *what*. Bundle the HTML report and the `README.md` index update in one commit.
3. Open a pull request against `main`.
4. Do **not** push directly to `main`.

## What to avoid

- Do not reformat files unrelated to your change.
- Do not remove error handling or tests.
- Do not commit secrets, credentials, or large binary files.
- Do not amend published commits.
- Do not add external stylesheets, JavaScript, or other remote dependencies to reports — they must render offline.
- Do not change the filename of an already-published report; doing so breaks existing shared links.
- Do not add an `og:image` tag unless you have a stable hosted image URL — omit it and Slack will render a text-only card.
