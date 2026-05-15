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
| 2026-05-15 | [PR #9362 HFE fast-path — n=8 vs n=5 follow-up](2026-05-15-pr9362-hfe-write-read-concurrent-v2.html) | Same workload as the n=3 report below, but updated PR head (14f9779) and 8 master + 5 Joan datapoints. FT.SEARCH tail regression on Joan now reproduces and is credible; the v1 HPERSIST tail "improvement" was n=3 noise. |
| 2026-05-15 | [PR #9362 HFE fast-path — search-hfe-write-read-concurrent on cluster-04-threads-6 (n=3)](2026-05-15-pr9362-hfe-write-read-concurrent.html) | First pass at n=3; superseded by the v2 report above (PR head moved a89a0c5→14f9779). |
| 2026-05-13 | [v8.6.0→v8.7.91 focused regression](v8.6.0-to-v8.7.91-regression.html) | gc + filter-pipeline regressions tag-to-tag, 5 runs/version, with Polar Signals links |
