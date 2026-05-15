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
- `v8.6.0-to-v8.7.91-regression.html` — 4-version comparison with per-(test,command) profile rows.
