# claude-rooster

Wakes Claude Code up via GitHub Actions scheduled events to optimise session timings.

## How it works

A GitHub Actions workflow runs on an hourly cron trigger. At each hourly tick it
checks the current UTC time against a configurable list of target times stored in a
**repository variable**. When the current time matches one of those slots, it calls
the [Anthropic Messages API](https://docs.anthropic.com/en/api/messages) and prints
Claude's response to the workflow log.

```
GitHub Actions (hourly cron)
        │
        ▼
  Check current UTC time
  against SCHEDULE_TIMES variable
        │
  time matches? ──no──► skip (job exits cleanly)
        │
       yes
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

### 2. Configure the schedule times (optional)

The workflow checks the repository variable `SCHEDULE_TIMES` for a comma-separated
list of `HH:MM` times in **UTC**. If the variable is not set it falls back to
`09:00,13:00,17:00`.

In your repository go to **Settings → Secrets and variables → Actions → Variables**
and create a variable named:

| Variable | Example value | Description |
|----------|---------------|-------------|
| `SCHEDULE_TIMES` | `08:00,12:00,18:00` | Comma-separated UTC times to call Claude |

### 3. Trigger manually (optional)

You can also invoke the workflow at any time from
**Actions → Claude Rooster → Run workflow**. An optional *Prompt* input lets you
override the default message sent to Claude for that run.

## Workflow file

The workflow is located at [`.github/workflows/claude-rooster.yml`](.github/workflows/claude-rooster.yml).

Key environment variables defined at the top of the workflow:

| Variable | Default | Description |
|----------|---------|-------------|
| `DEFAULT_SCHEDULE_TIMES` | `09:00,13:00,17:00` | Fallback times used when `SCHEDULE_TIMES` repo variable is not set |
| `DEFAULT_PROMPT` | *(see file)* | Prompt sent to Claude on each scheduled run |
