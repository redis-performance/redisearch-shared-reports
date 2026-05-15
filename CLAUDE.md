# shared-reports — authoring conventions

Instructions for agents (and humans) producing reports in this repo. The user-facing landing page is `README.md`; everything below is implementation detail.

## File format

- One self-contained HTML file per report (preferred) or a `.md` file (rendered by Jekyll).
- Filename pattern: `YYYY-MM-DD-<short-slug>.<ext>` so the index sorts naturally and the URL is self-explanatory.
- Inline CSS — no external stylesheets, no JS. Reports must render offline and survive being mirrored to other places.

## Slack-friendly preview (Open Graph + Twitter Card)

Every HTML report MUST carry these meta tags right after `<title>` so the URL renders a TL;DR card when pasted into Slack:

```html
<meta name="description" content="<TL;DR with ⚪/🟢/🔴 emojis>">
<meta property="og:type" content="article">
<meta property="og:site_name" content="RediSearch shared reports">
<meta property="og:title" content="<short title — fits on one card line>">
<meta property="og:description" content="<TL;DR with ⚪/🟢/🔴 emojis>">
<meta property="og:url" content="https://redis-performance.github.io/redisearch-shared-reports/<filename>.html">
<meta name="twitter:card" content="summary">
<meta name="twitter:title" content="<short title>">
<meta name="twitter:description" content="<TL;DR — can be shorter than og:description>">
```

Rules for the description:
- **Emoji colors must match the in-doc table** so the card tells the same story at a glance:
  - 🟢 improvement (≥3% in metric's right direction)
  - 🔴 regression (≥3% in wrong direction)
  - ⚪ within noise (<3% on means, or driven by single-run outliers)
- Keep `og:description` under ~300 chars — Slack truncates anything longer.
- `og:url` must be the absolute GH Pages URL — relative paths don't unfurl.
- Skip `og:image` unless you have a stable hosted image; Slack falls back gracefully to a text-only card.

### Slack cache gotcha

Slack aggressively caches unfurls per URL. If you re-publish meta-tag changes for an already-shared link, force a re-scrape with one of:
- Append `?v=N` (or any unused query string) to the link.
- Use `/unfurl <URL>` in any channel.

## Cell format conventions (when reporting deltas)

For multi-version / multi-target comparison tables:
- Baseline column: `<b>mean</b><br>CV X%` — no Δ shown (it's the reference).
- Comparison column: `<b>mean</b><br>CV X%<br><span class="<class>">{emoji} {sign}{abs_delta}%</span>` where `<class>` is `improve` / `regress` / `neutral`.
- Drop `Totals` (it duplicates the per-command rows) and `p50` (it's noise) unless they tell their own story.
- Profile rows: a single "Polar Signals profiles" row per (test, command) — or per topology if the spec runs all commands in one memtier invocation. Don't duplicate the same window across rows.

## Linkable git hashes

Every short SHA that appears in the report MUST link to the corresponding commit on GitHub. Readers should be able to click any hash in a column header, a "raw datapoints" bullet, an "individual reports" cross-reference, or a "PR head moved A → B → C" chain and land on the commit page.

URL pattern (RediSearch — adjust the org/repo for other targets):

```
https://github.com/RediSearch/RediSearch/commit/<full-40-char-sha>
```

Anchor template (mirror the table-cell style — display the short form, link the full SHA):

```html
<a href="https://github.com/RediSearch/RediSearch/commit/<full>" target="_blank" rel="noopener noreferrer">{short}</a>
```

Places to cover:
- Column headers in `<small>` sub-labels (e.g. `<small>{linked-short} · n=5</small>`).
- "Setup" / "Targets" paragraph — any `<code>{hash}</code>` and any A → B → C chain.
- "Raw datapoints" bullets — `Joan v2 ({linked-short})`.
- "Individual reports" / cross-references — `[v2 ({linked-short}, n=5)](filename.html)`.

Two safer ways to apply this consistently across a report:
1. **Generate-time**: emit linked anchors directly from the report-builder script so the hashes are linked at first publish.
2. **Post-process**: run a one-shot regex pass over the HTML. The HFE evolution report used a Python pass keyed on a `{short: full}` dict and a handful of `re.sub` patterns over `<small>SHORT · n=N</small>`, `<code>SHORT</code>`, `<code>SHORT → SHORT → …</code>`, and `(SHORT,` / `(SHORT)` patterns. Either approach is fine; pick whichever fits the existing builder.

If short hashes appear ambiguous (e.g. a 7-char prefix matches multiple unrelated commits), use a longer prefix in the display text — at 8 chars collisions are extremely unlikely on a single repo.

## Index discipline

Add an entry to `README.md`'s index table on every new report. Format:

```
| YYYY-MM-DD | [<Short title with key context>](filename.html) | <one-line summary: workload + datapoints + targets> |
```

Keep the table sorted newest-first.

## Commit hygiene

- One commit per report addition (HTML + README.md index update bundled).
- Subject line: `<Report title>` so it's findable later.
- Body: 2-3 lines on workload + targets + headline finding.

## Reference reports

- `2026-05-15-pr9362-hfe-write-read-concurrent.html` — 2-target comparison with single profile row, OG meta, colored description.
- `2026-05-15-pr9362-hfe-evolution.html` — 5-column longitudinal comparison (1 baseline + 4 PR-head commits), every short SHA linked to its GitHub commit.
- `v8.6.0-to-v8.7.91-regression.html` — 4-version comparison with per-(test,command) profile rows.
