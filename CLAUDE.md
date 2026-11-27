# Votega/ga-legislators: instructions for AI coding agents

This repository is **published output**. Its data files are generated in
[Votega/votega.org](https://github.com/Votega/votega.org) and overwritten on every
publish, so an edit made here is undone by the next run. This file is generated too
(`scripts/lib/sibling_publish.py`, `CLAUDE_MD_TEMPLATE`).

## What not to edit here

Every file in this repository except the ones listed below: the data (JSON, CSV,
GeoJSON), the schemas, `latest.json`, the per-folder `README.md` / `.md` summaries
inside data folders, `LICENSE`, `NOTICE.md`, `data/*/manifest.json` and this file.

## What is maintained by hand here

- `README.md` at the root: what the files are and the caveats a schema cannot express.
- `CHANGES.md` at the root, where present: only changes a consumer must act on (a new
  or renamed field, a changed `$id`, corrected values), newest first. Routine data
  refreshes are not listed.
- `.github/`, where present.

When a published field or file changes, update `README.md` and `CHANGES.md` here in the
same piece of work as the publisher change in votega.org.

## Where a fix goes

Wrong data, a missing field, a schema or CSV change: fix it in votega.org. This
repository is written by:

- `scripts/publish_ga_legislators.py`

Producers, data dictionaries and conventions there:

- `docs/conventions-publishing.md`: the rules every sibling repo follows. New CSV
  columns are appended, never inserted. A schema gains `properties` but never a new
  `required`. Each schema's `$id` is its own path, and it carries `schemaVersion`
  (bumped for a change a consumer must act on) and `schemaStability`.
- `docs/data-schemas.md` and `docs/ga-soap-data-dictionary.md`: what the source fields
  mean and how the published ones are built.

## Upstream

The facts come from the Georgia General Assembly (legis.ga.gov).
