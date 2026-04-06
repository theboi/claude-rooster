# claude-rooster

Wakes Claude Code up via GitHub Actions scheduled events to optimise session timings.

## How it works

A GitHub Actions workflow runs at four fixed times each day (UTC). At each trigger it
calls the [Anthropic Messages API](https://docs.anthropic.com/en/api/messages) and
prints Claude's response to the workflow log.

```
GitHub Actions (06:00 / 11:00 / 16:00 / 21:00 UTC)
        │
        ▼
  POST /v1/messages  ──►  Anthropic API  ──►  Claude response logged
```

## Setup

### 1. Add the API key secret

In your repository go to **Settings → Secrets and variables → Actions → Secrets**
and create a secret named:

| Secret | Value |
|--------|-------|
| `ANTHROPIC_API_KEY` | Your Anthropic API key |

### 2. Trigger manually (optional)

You can also invoke the workflow at any time from
**Actions → Claude Rooster → Run workflow**. An optional *Prompt* input lets you
override the default message sent to Claude for that run.

## Workflow file

The workflow is located at [`.github/workflows/claude-rooster.yml`](.github/workflows/claude-rooster.yml).

Key environment variables defined at the top of the workflow:

| Variable | Default | Description |
|----------|---------|-------------|
| `DEFAULT_PROMPT` | *(see file)* | Prompt sent to Claude on each scheduled run |
