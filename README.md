# Çapraz Manşet 5.0

Static export of Çapraz Manşet 5.0: the built site, its data and the pipeline that produces them.
Built from [`aylinunel/caprazmanset-oct26`](https://github.com/aylinunel/caprazmanset-oct26) branch
`claude/capraz-5-pipeline` at `fcb1eab3f2d68b9d95fe247dc2501975ca4d5950`; that repository remains the
place where the code is developed.

| | |
|---|---|
| Window | 1 September – 25 October 2026 (Türkiye time) |
| Articles | 139,702 (130,484 with a lede), 28 outlets, no sampling |
| Events | 2,911 (2,771 automatic, 140 hand-checked and unchanged) |
| Source | `backfill-data` branch of `aylinunel/turkish-news-scraper` |

## Open the site

`public/` is the whole site: `index.html` plus `public/data/*.bin` (gzip JSON packs: `core`, one
headline pack and one lede pack per month). Serve the folder with any static server, for example

```bash
cd public && python3 -m http.server 8000   # then open http://localhost:8000
```

Opening `index.html` straight from disk does not work: the page fetches its data files.

## Rebuild or extend

```bash
python -m pip install -r pipeline/requirements.txt     # Python 3.11+, Node 22
cd pipeline
python -m capraz_pipeline run --from <collector export> --since 2026-09-01 --site ../public
```

Older dates or a scheduled live run use the same command with a different `--from`/`--since`; see
[pipeline/README.md](pipeline/README.md) and [ADR-0002](docs/adr/0002-analysis-pipeline.md).
`docs/build-report.json` holds the validation figures of this build (classifier cross-validation,
clustering agreement with 4.0, topic model coherence, sizes, run time).

## Layout

```
public/            built site (publish this)
pipeline/          analysis pipeline + offline tests
shared/analysis/   the headline scorer shared by the site and the pipeline
site/              site sources (4.0 renderer + 5.0 views + data loader)
data/reference/    4.0 labels, hand-checked events and editorial layer the algorithms extend
docs/              ADR-0002 and the build report
```

## Boundaries

Machine results on this site (scores, events, blind spots, speed, headline–lede gap, similarity map,
topic model) are labelled "otomatik" and documented on the Yöntem page. Outlet placements, ownership
notes, narrative coding and hand-checked events are supplied material carried unchanged, not
independently verified. Nothing here is published to the editorial app; imports there stay drafts.
