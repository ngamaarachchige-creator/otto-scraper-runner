# otto-scraper-runner

Runs [otto-deal-hunter](https://github.com/ngamaarachchige-creator/otto-deal-hunter)'s scheduled scraper on GitHub Actions. This repo holds no scraper logic itself — otto-deal-hunter is private, and private repos share one account-wide free-tier pool of Actions minutes, which a heavy hourly scraper burns through fast. This repo is public specifically so those Actions minutes are free and unlimited, without needing otto-deal-hunter itself to be public.

## How it works

The single workflow (`.github/workflows/scrape.yml`) checks out otto-deal-hunter's real source using the `OTTO_REPO_TOKEN` secret (a fine-grained PAT scoped to just this repo and otto-deal-hunter: `Contents: Read-only` on otto-deal-hunter so the checkout works, `Actions: Read and write` on this repo so otto-deal-hunter's Cloudflare Worker cron can dispatch it), then runs `scripts/scheduled_scrape.py` exactly as it would inside otto-deal-hunter itself.

## Required secrets

- `OTTO_REPO_TOKEN` — the fine-grained PAT described above.
- `D1_PROXY_URL` / `D1_PROXY_AUTH_TOKEN` — same values as otto-deal-hunter's own secrets, so the scraper writes to the same D1 database.
