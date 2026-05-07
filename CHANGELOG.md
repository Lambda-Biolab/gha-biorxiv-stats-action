<!-- markdownlint-disable MD024 no-duplicate-heading -->

# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

**Types of changes**: `Added`, `Changed`, `Deprecated`, `Removed`, `Fixed`, `Security`

## [Unreleased]

### Renamed

- Repo `gha-biorxiv-stats-action` → `gha-rxiv-stats-action` to reflect
  multi-server scope. GitHub auto-redirects old URLs; in-repo
  references (README badges, Usage examples, action.yaml `name`)
  updated (#19).

### Added

- Client-side `CATEGORIES` filter: comma-separated set, case-insensitive,
  applied in `parse_biorxiv_json` (the bioRxiv `/details/` API has no
  server-side category filter, so the prior `?category=` URL query was a
  no-op) (#1)
- `prune_existing_csvs()` rewrites historical CSVs on each run so rows
  outside the current category set are dropped retroactively (#1)
- `docs/categories.md` documenting per-server taxonomies (bioRxiv 25
  canonical, medRxiv 29 sampled) (#1)
- Matrix workflow `write-rxiv-stats.yml` (renamed from
  `write-biorxiv-stats.yml`) running biorxiv + medrxiv legs serially
  (`max-parallel: 1`) under one cron (#4)
- `data/medrxiv/.gitkeep` scaffolding the per-server output dir (#4)
- Project-scoped Claude Code plugins via `.claude/settings.json` (#2)
- Issue #7: ingestion recipes for consumer repos + `gh models` LLM
  pipeline (relevance filter + abstract extraction)
- `data/biorxiv/.gitkeep` mirroring `data/medrxiv/.gitkeep` (#12, later
  removed in #18 in favor of a single `data/.gitkeep`)
- `Lint MD and Links` workflow now starts cleanly — caller workflow
  grants the `issues: write` permission the reusable workflow's
  `notify` job requires (#14)
- README inline `strategy.matrix` snippet wrapped in `<details>` for
  compact reading (#15, #16)

### Changed

- Output layout: `./data/<server>/<year>/<week>.csv` (was
  `./data/<year>/<week>.csv`); default `OUT_DIR` is now
  `./data/biorxiv` (#1)
- Default `CATEGORIES` set to 8 Lambda-Biolab focus areas:
  `bioinformatics, bioengineering, microbiology, biochemistry,
  biophysics, pharmacology and toxicology, genomics, synthetic
  biology` (#1)
- medRxiv matrix leg uses 4 core categories: `infectious diseases,
  dentistry and oral medicine, pharmacology and therapeutics, genetic
  and genomic medicine` (#4)
- Schedule changed from daily to weekly (Mondays @ 01:00 UTC) (#11)
- All workflow actions pinned to full-length commit SHAs to satisfy
  repo policy: `actions/checkout` → `de0fac2` (v6.0.2),
  `astral-sh/setup-uv` → `37802ad` (v7.6.0), `github/codeql-action/*`
  → `e46ed2c` (v4.35.3), `callowayproject/bump-my-version` →
  `e6ecdc3` (1.3.0) (#1, #6)
- Expanded `.gitignore` (Python, IDE, secrets, Claude per-user,
  OS) (#5)
- README, CHANGELOG, LICENSE refreshed to current state; LICENSE now
  byte-matches the canonical Apache-2.0 text with copyright line
  `Copyright 2026 Lambda-Biolab` (#13)
- README header trimmed (no static social-preview image; GitHub's
  auto-generated open-graph card carries org branding) (#15, #17)
- API section lists both bioRxiv and medRxiv URLs explicitly (#17)
- chemRxiv/psyArXiv mentions removed across README, categories doc,
  and CHANGELOG entry (data preserved inline in upstream issues
  qte77/gha-rxiv-stats-action#69 and #70) (#17, #18)

### Fixed

- bioRxiv `?category=` URL query was a silent no-op against the
  `/details/` endpoint (now applied client-side) (#1)

---

## [0.1.0] - 2026-03-29

---

### Added

- HTTPS validation for API server URLs (#10)
- Test and lint CI workflow (#12)

### Changed

- Rename `app/` to `src/` for standard project layout (#10)
- Migrate to `uv` package manager, DRY workflow (#12)
- Migrate license to Apache-2.0 (#13)
- Bump `actions/checkout` from 4 to 6 (#2)
- Bump `actions/setup-python` from 5 to 6 (#3)
- Bump `callowayproject/bump-my-version` from 1.2.7 to 1.3.0 (#4)

### Fixed

- Use temp file for tree entries to avoid argument list overflow (#5)
- Cast biorxiv API total/count to int for pagination (#6)
- Reduce default DAYS to 1, increase timeout, clear category (#7)
- Defensive `.get()` in `needs_pagination` for missing keys (#8)
- Update workflow `APP_DIR` from `app/` to `src/` (#11)

## [0.0.0] - 2026-03-26

### Added

- Composite `action.yaml` with inputs (OUT_DIR, DAYS, CATEGORIES, SERVER, PY_VER, TOKEN) and branding
- biorxiv API integration (`https://api.biorxiv.org/details/{server}/{date1}/{date2}/{cursor}/json`)
- 6 pytest tests for API fetch, parsing, and CSV output

---
