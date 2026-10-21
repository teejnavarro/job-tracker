# Cloud routine (JT-009)

## What it is
A Claude Code cloud scheduled task named "Job Tracker inbox check". It runs the `/jobmail` inbox check on Anthropic's cloud, so it works while the owner's PC is off.

## Schedule
Daily at 08:07 and 17:07, Asia/Manila (entered in local time). The 7-minute offset avoids the late starts that on-the-hour runs get.

## Repository
`teejnavarro/job-tracker`, branch `main` (cloned fresh from GitHub on every run, so push `main` before relying on a change).

## Connectors
Gmail, Notion and Google Calendar only. Remove every other connector from the task.

## Model
Account default.

## Prompt
Use this text exactly:

```
Run the Job Tracker inbox check. Follow .claude/commands/jobmail.md in this repository exactly, with the arguments: live cloud
Read rules/email-rules.md and docs/NOTION.md as that file says.
Do not change, commit or push any files in this repository, and do not open pull requests.
If a Gmail, Notion or Google Calendar tool is missing, stop, say which, and still write a Failed Runs row if Notion works.
End with the command's live-run summary.
```

## How to check a run
- Open the run on claude.ai/code/routines. A green status does not mean success; read the transcript.
- In the Notion Runs database, look for a row with Source Cloud and Result OK.
- Check nothing was duplicated: no repeated Email Log rows, no repeated Jobs labels, no repeated `[Job]` calendar events.

## Pause or change
Use the on/off switch on the task page, or `/schedule update`. Keep this file in step with any change to the task.
