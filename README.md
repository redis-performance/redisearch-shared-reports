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
| 2026-05-15 | [PR #9362 HFE fast-path — 4-commit evolution vs master](2026-05-15-pr9362-hfe-evolution.html) | Consolidated longitudinal view of all 4 PR-head passes (a89a0c5 → 14f9779 → 79b85e5 → 88ef9b2) vs the same master baseline. Same workload + same topology + master pinned, so the only variable across columns is the Joan commit. |
| 2026-05-15 | [PR #9362 HFE fast-path — v4 (Joan @ 88ef9b2, n=8 vs n=4)](2026-05-15-pr9362-hfe-write-read-concurrent-v4.html) | Fourth pass; PR head moved 79b85e5→88ef9b2. Throughput parity; HPERSIST p99 −25.5%, FT.SEARCH p99 −11.8% — v2 FT.SEARCH regression remains absent. n=4 (vs 5 dispatched) due to S3 filename collision. |
| 2026-05-15 | [PR #9362 HFE fast-path — v3 (Joan @ 79b85e5, n=8 vs n=5)](2026-05-15-pr9362-hfe-write-read-concurrent-v3.html) | Superseded by v4. Third pass; PR head moved 14f9779→79b85e5. FT.SEARCH tail regression seen in v2 no longer reproduces. |
| 2026-05-15 | [PR #9362 HFE fast-path — v2 (Joan @ 14f9779, n=8 vs n=5)](2026-05-15-pr9362-hfe-write-read-concurrent-v2.html) | Superseded by v4. Showed credible FT.SEARCH tail regression on Joan @ 14f9779. |
| 2026-05-15 | [PR #9362 HFE fast-path — v1 (n=3, Joan @ a89a0c5)](2026-05-15-pr9362-hfe-write-read-concurrent.html) | Superseded by v4. First-pass n=3 with the initial PR head. |
| 2026-05-13 | [v8.6.0→v8.7.91 focused regression](v8.6.0-to-v8.7.91-regression.html) | gc + filter-pipeline regressions tag-to-tag, 5 runs/version, with Polar Signals links |
