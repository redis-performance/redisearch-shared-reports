# RediSearch shared reports

Sharable HTML/MD performance reports from the RediSearch perf team — served via GitHub Pages.

## How to add a report

```bash
cd shared-reports     # this directory, as a submodule of the agent workspace
cp /path/to/report.html ./<descriptive-name>.html   # or .md
git add . && git commit -m "<context>"
git push
```

GitHub Pages auto-deploys on push to `main`. URL pattern:
`https://redis-performance.github.io/redisearch-shared-reports/<filename>`

## Index

| Date | Report | Context |
|------|--------|---------|
| 2026-05-13 | [v8.6.0→v8.7.91 focused regression](v8.6.0-to-v8.7.91-regression.html) | gc + filter-pipeline regressions tag-to-tag, 5 runs/version, with Polar Signals links |
