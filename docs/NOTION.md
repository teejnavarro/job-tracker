# Notion layout

The job tracker lives in Notion (workspace "Teej Navarro's Space"). Later stories read and write it through the Notion connector, so use these names exactly.

## Page

- Title: Job Tracker (private, not shared, workspace level)
- Page ID: `3fe43162-036c-81b4-8f1e-f42ad5396bb6`
- URL: https://app.notion.com/p/3fe43162036c81b48f1ef42ad5396bb6

It holds three inline databases: Job Applications, Email Log, Runs.

## Job Applications

- Database ID: `0b62afe9-bd09-443a-b393-edea5521c341`
- Data source: `collection://d6966c55-ce11-4dbc-81b0-b5fdd296cf06`

| Property | Type | Options / notes |
|---|---|---|
| Company | title | |
| Role | text | |
| Source | select | LinkedIn, Indeed, Other |
| Link | url | Dedupe key for /tailor and /jobadd |
| Status | status | to_do: Drafting. in_progress: Applied, Assessment, Interview, Offer. complete: Rejected, Withdrawn |
| Applied date | date | |
| Deadline | date | |
| Last contact | date | |
| Next action | text | |
| Next action date | date | |
| Sender domains | text | Comma-separated, e.g. `microsourcing.com, myworkdayjobs.com` |
| Local folder | text | Folder name under resume-tailor `applications/` |
| Needs a look | checkbox | |
| Reminder event IDs | text | Calendar event IDs for the 7-day nudge and deadline reminder |
| Emails | relation | Created by the Email Log relation (synced). Do not write to it directly; set Application on the Email Log row |

Default status for new rows is Drafting (the only to_do option).

### Views

- Board: board grouped by Status, shows Role, Next action, Next action date, Needs a look. Intended default view.
- All: table sorted by Applied date, newest first. Shows every property except Reminder event IDs and Emails.

The tools could not reorder views, so Board must be dragged to the first tab by hand in Notion.

## Email Log

One row per job email handled. Subject and IDs only.

- Database ID: `55175910-ef2d-4c36-a1b6-7bb756681e77`
- Data source: `collection://05160f03-566d-4f07-b6d4-c3f6b0b91a2a`

| Property | Type | Options / notes |
|---|---|---|
| Subject | title | Email subject only |
| Message ID | text | Gmail message ID. Dedupe key |
| Thread ID | text | |
| From | text | Sender address |
| Received | date | Date and time |
| Class | select | Confirmation, Recruiter question, Interview, Assessment, Offer, Rejection, Needs a look |
| Application | relation | To Job Applications (synced property "Emails"). Empty for Needs a look |
| Event ID | text | Calendar event created from this email, if any |

## Runs

One row per inbox check. Gives the next run its start time and shows cloud runs happened.

- Database ID: `46f43686-83ad-45ff-9d8a-fc695c836dab`
- Data source: `collection://e0d0fd0e-ad3c-44b3-815a-0b6f1feff944`

| Property | Type | Options / notes |
|---|---|---|
| Run | title | e.g. `2026-10-20 08:00 cloud` |
| Started | date | Date and time, Asia/Manila |
| Source | select | Local, Cloud |
| Mode | select | Dry run, Live |
| Result | select | OK, Failed |
| Searched from | date | Start of the Gmail window used |
| Summary | text | Short plain summary |

## Rules

- Last successful run = the newest Runs row with Result OK and Mode Live. Dry runs never move the window.
- Dedupe keys: Link for Job Applications, Message ID for Email Log.
- Query with rows mode and structured filters, not SQL (SQL query mode is capped on this plan).
- Never store email bodies. Subject, sender, IDs and dates only.
- Date-time values need `date:<property>:is_datetime` set to 1 when writing rows.
