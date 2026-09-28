# auto_start_claude_counter

GitHub Actions workflow that sends "hi" to Claude (Haiku) at fixed times so the
5-hour usage window starts automatically.

## Setup
1. `claude setup-token` → copy the token it prints.
2. Push this folder to a **private** GitHub repo.
3. Repo → Settings → Secrets and variables → Actions → New secret
   `CLAUDE_CODE_OAUTH_TOKEN` = the token.
4. Actions tab → claude-warmup → Run workflow (to test).

## Change the schedule
Edit the `cron` lines in `.github/workflows/warmup.yml` (times are **UTC**;
IST = UTC + 5:30).

## Notes
- GitHub disables scheduled workflows after 60 days without repo activity —
  push a commit occasionally (or re-enable in the Actions tab).
- Scheduled runs can be 5–30 min late.
