# DH Conference Radar

A small Jekyll/GitHub Pages site for tracking major Digital Humanities conferences and the organizations behind them.

**Live site:** [https://dh-conference-radar.github.io/](https://dh-conference-radar.github.io/)

**Repository description:** A curated, source-linked calendar of major Digital Humanities conferences, with organization details, downloadable calendar files, and quarterly source checks.

The site is deliberately data-first:

- `_data/conferences.yml` contains event facts and confidence/status fields.
- `_data/organizations.yml` contains association details.
- `_data/source_watch.yml` lists official pages to check every three months.
- The visual presentation uses the remote [Serif Jekyll theme](https://jekyllthemes.io/theme/serif).
- `calendar/*.ics` contains one downloadable calendar file per confirmed dated conference.
- `scripts/generate_ics.py` regenerates the calendar downloads from `_data/conferences.yml`.
- `scripts/update_conferences.py` detects changed official source pages and writes a review report.

## Publish on GitHub Pages

This repository is designed for the organization/user Pages address:

```text
https://dh-conference-radar.github.io/
```

Keep GitHub Pages set to **GitHub Actions** in repository settings. The included `jekyll-gh-pages.yml` workflow builds and deploys the Jekyll site whenever `main` changes, and can also be run manually from the Actions tab.

## Local development

```bash
bundle install
bundle exec jekyll serve
```

Open `http://127.0.0.1:4000`.

Use Ruby 3.2 or newer. The included `.ruby-version` and GitHub Actions workflow both target Ruby 3.2.

## Quarterly updates

The `Quarterly source check` workflow runs on the first day of every third month and can also be started manually. When a watched source page changes, it updates `_data/source_watch.yml`, writes a report under `reports/`, and opens a pull request for review.

This review step is intentional. DH organizations publish conference information in different formats, and reviewed updates reduce the risk of a brittle scraper silently changing dates incorrectly.

## Adding a conference

Add an entry to `_data/conferences.yml` with:

- `status: confirmed` when an official source gives dates and place.
- `status: watch` when the organization is important but the next event is not yet announced.
- `source` pointing to the specific official page where the claim can be checked.

Set `sort_date` to the start date for confirmed events or a far-future value such as `9999-01-01` for watchlist entries.

After adding or changing confirmed dates, regenerate downloadable calendar files:

```bash
python3 scripts/generate_ics.py
```
