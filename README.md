# auto_start_claude_counter

GitHub Actions workflow that sends "hi" to Claude (Haiku) every **5h05m**, so
the 5-hour usage window restarts automatically.

## How it works
- After each ping, the workflow writes the next due time (Unix seconds) to
  `next_run.txt` and commits it. Those commits also keep the repo active, so
  GitHub doesn't disable the schedule after 60 days.
- A checker runs every 30 min and pings only once that time has passed, so a
  ping can land up to ~30 min (plus GitHub delay) after its due time.
- Actions → claude-warmup → **Run workflow** pings immediately and restarts
  the 5h05m cycle from now.

## Setup
1. In a normal terminal (PowerShell/cmd, not the VS Code chat), run
   `claude setup-token`, approve in the browser, then return to the terminal:
   the token (`sk-ant-oat01-...`) is printed there.
2. `gh secret set CLAUDE_CODE_OAUTH_TOKEN` and paste the token.
3. `gh workflow run claude-warmup` to start the cycle.

## Tuning
- Interval: `INTERVAL_SECONDS` in `.github/workflows/warmup.yml`.
- Checker frequency: the `cron` line (keep `*/30` on a private repo to stay
  within the free minutes).
