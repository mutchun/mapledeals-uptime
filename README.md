# mapledeals-uptime

External uptime probe for https://mapledeals.app (MD-775 launch ops watch, section B).

- One workflow: `.github/workflows/uptime.yml`, on a 5-minute schedule.
- It checks the home page, `/api/app-config` and `/api/dealList` from a GitHub-hosted runner (outside the droplet), with marker strings so a 200 error page still fails.
- Telegram alert to the ops channel after 2 consecutive failed checks, re-alert about hourly while down, and a recovery line when green again.
- State lives in this workflow's own run history (no cache, no gist). Telegram credentials are repo secrets `TG_OPS_BOT_TOKEN` / `TG_OPS_CHAT_ID`.
- This repo is public only so the schedule runs on free Actions minutes. It contains no application code and no secrets.

The canonical copy is mirrored in the private app repo at `scripts/uptime-probe/uptime.yml`. Runbook: `docs/ops/LAUNCH_OPS_RUNBOOK.md` in that repo (`#uptime-down`, `#probe-failure`).

GitHub cron is best effort and can run late; treat this as a fallback pager next to the droplet's own probes (ops_monitor.sh) and a hosted keyword monitor.

Schedule: every 5 min (minutes 2,7,...,57). If the Actions tab shows no `schedule` runs, push any commit to main to re-register it.
