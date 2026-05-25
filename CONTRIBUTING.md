# Contributing

We treat this repo as "Open Source" within Redis: anyone who clears the bar below is welcome to contribute.

## Local setup

This is a static collection of self-contained HTML (and occasionally Markdown) reports served via GitHub Pages — there is no build step, package manager, or test runner.

```bash
git clone git@github.com:redis-performance/redisearch-shared-reports.git
cd redisearch-shared-reports

# Open any report directly in a browser to review it locally.
xdg-open index.html   # Linux
open index.html       # macOS
```

GitHub Pages auto-deploys on every push to `main`. Published URLs follow:
`https://redis-performance.github.io/redisearch-shared-reports/<filename>`

## Adding a report

1. Drop the self-contained report file in the repo root, named `YYYY-MM-DD-<short-slug>.<ext>` (`.html` preferred, `.md` rendered by Jekyll).
2. Add a newest-first row to the index table in `README.md`.
3. Commit both the report and the index update together.

See `CLAUDE.md` for the full authoring conventions — required Open Graph / Twitter Card meta tags, emoji-color rules, linked-SHA patterns, and cell-format conventions for delta tables.

## Branch naming

```
<type>/<short-description>
```

Types: `feat`, `fix`, `refactor`, `test`, `docs`, `chore`

Example: `docs/add-v8.8-regression-report`

## Coding standards

- Keep changes focused; one logical change per PR.
- Follow the conventions already present in `CLAUDE.md` and in existing reports.
- Reports must be self-contained: inline CSS only, no external stylesheets, no JavaScript — they have to render offline.
- Every short SHA in a report must link to its GitHub commit.
- Every report must carry the required Open Graph / Twitter Card meta tags with a description under ~300 chars.
- Never change the filename of an already-published report — it breaks shared links.

## Submitting changes

1. Create a branch from `main`.
2. Make your changes with clear, atomic commits. Bundle the report and its `README.md` index row in one commit.
3. Open a pull request against `main` with a descriptive title and summary.
4. Address review comments promptly; force-push to the same branch to update.
5. Do not push directly to `main`.

## Testing

There is no automated test suite. Before opening a PR, verify by hand:

1. The report opens and renders correctly in a browser.
2. Every short SHA is linked to the correct GitHub commit.
3. Open Graph meta tags are present with a description ≤ 300 chars, and the emoji colors match the in-doc table (🟢 improvement, 🔴 regression, ⚪ within noise).
4. A newest-first row was added or updated in the `README.md` index.
5. The filename follows the `YYYY-MM-DD-<short-slug>.<ext>` pattern (`ls *.html` to sanity-check).

## Review process

- At least one maintainer approval is required before merge.
- Maintainers may request changes or close PRs that don't meet the bar — this is normal and not personal.
