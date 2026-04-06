# claude-rooster

Wakes Claude Code up via GitHub Actions scheduled events to optimise session timings.

## Setup

In your terminal, use Claude Code to generate a long-lived OAuth credential token attached to your Claude Pro/Max subscription account (not Claude Console/API).
```bash
npm install -g @anthropic-ai/claude-code # if not yet installed
claude setup-token
```

In your repository go to **Settings → Secrets and variables → Actions → Secrets**
and create a secret named:

| Secret | Value |
|--------|-------|
| `CLAUDE_CODE_OAUTH_TOKEN` | Your OAuth token |

IMPORTANT: ensure the copied token does NOT include a newline (when copying from the terminal with line wrapping, the newline is copied also). The Action will throw "API Error: Headers.append: "***\n ***" is an invalid header value."

Set the cron timings and default prompt to wake Claude up via the `claude-rooster.yml` file.

| Variable | Default | Description |
|----------|---------|-------------|
| `DEFAULT_PROMPT` | *(see file)* | Prompt sent to Claude on each scheduled run |
