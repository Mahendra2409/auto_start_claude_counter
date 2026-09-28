# auto_start_claude_counter

GitHub Actions workflow that keeps a Claude 5-hour usage window always
running, by sending "hi" (Haiku) right after each window resets.

## How it works
- Each ping runs `claude -p` with `--output-format stream-json`, which reports
  the exact reset time of the current 5-hour window. `next_run.txt` is set to
  that time + 30 s and committed.
- A checker runs about every 5 min. When the reset is under 20 min away, the
  job waits for it and pings right after, so a new window starts immediately.
- If you message Claude yourself first, the next ping just reads that window's
  reset time and lines up with it.
- Failed pings retry 3 times, then again in 10 min. A rejected token shows a
  "Claude token rejected" error and retries every 6 h.
- The `next_run.txt` commits keep the repo active, so GitHub doesn't disable
  the schedule after 60 days.

## Setup
1. In a normal terminal (PowerShell/cmd), run `claude setup-token`, approve in
   the browser, then copy the `sk-ant-oat01-...` token printed in the terminal.
2. `gh secret set CLAUDE_CODE_OAUTH_TOKEN` and paste it.
3. `gh workflow run claude-warmup` to ping now and sync with the current window.

## Check on it
`gh run list --workflow warmup.yml --limit 10`, or read the commit log: each
"next ping: ..." commit says when the next ping is due and why.

## Limits
- GitHub doesn't guarantee cron timing; under load, checks can be 10-20+ min
  apart. The 20 min in-job wait covers most of that.
- The token from `claude setup-token` expires (about a year). Repeat Setup
  steps 1-2 when you see the "Claude token rejected" error.
