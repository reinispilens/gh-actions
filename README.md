# gh-actions

Shared GitHub Actions for `reinispilens` repos — single source of truth so per-repo
workflows stay thin and don't drift. Contains **no secrets**.

## `telegram-notify`

Composite action that sends a formatted Telegram message for repo events.

```yaml
- uses: reinispilens/gh-actions/telegram-notify@main
  with:
    token: ${{ secrets.TELEGRAM_BOT_TOKEN }}
    chat_id: ${{ secrets.TELEGRAM_CHAT_ID }}
    kind: merge            # merge | failure | pr
    repo: ${{ github.repository }}
    title: "what happened (commit subject / PR title / failed workflow)"
    url: "https://github.com/owner/repo/commit/sha"
    actor: "who"           # optional
```

Each consuming repo keeps a small `.github/workflows/telegram-notify.yml` that wires the
events (push to main, pull_request, workflow_run failure) and calls this action. The
curl/format logic lives here, once.

Requires the consuming repo to have Actions secrets `TELEGRAM_BOT_TOKEN` and
`TELEGRAM_CHAT_ID`.
