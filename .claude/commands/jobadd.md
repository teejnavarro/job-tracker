---
description: Add a job you applied to without /tailor to the Notion tracker
argument-hint: <job link, or the pasted job description>
---
Run from the Job Tracker project root. Treat everything below as instructions to follow exactly.

## Before anything else, read

- `docs/NOTION.md`: database, fields, the `jobmail: all rows` view URL and write formats. Take every Notion ID from this file.

## Input

Read the `Arguments:` line that follows the included instructions. It is a job link, or pasted job text. If nothing is given, ask for a link or the text, then stop.

## Never

- Apply, click Apply / Easy Apply / Message, post, send anything, or change anything on a job site. Read page text only.
- Write anything except one Notion row in Job Applications. No calendar, Gmail, Email Log or Runs writes.
- Read Notion in any mode except view mode (never rows or SQL mode).

## Step 1: Read the posting

- **Indeed link:** take the `jk` value and call the Indeed connector's `get_job_details` with it. If that fails, open the link in the browser built into the Claude desktop app. If Indeed shows a security check, ask the owner to clear it in the browser pane, then read the page text.
- **LinkedIn link:** open it in the built-in browser and read the page text only.
- **Any other link, or if reading fails:** ask the owner to paste the job description, then stop until they do.
- **Pasted text with no link:** Link stays empty. Source is Other unless the text names LinkedIn or Indeed.

## Step 2: Pull out the fields

- Company
- Role (the job title as posted)
- Source (LinkedIn, Indeed or Other, from the link's host)
- Link (the link as given, without tracking query parameters except Indeed's `jk`)
- Deadline (only if the posting states an application deadline; never guess)

If Company or Role can't be found, ask the owner.

## Step 3: Check for a duplicate

Read every row of the `jobmail: all rows` view (view mode, page_size 100; if `has_more` is true, pass `next_cursor` as `start_cursor`). Compare job IDs: LinkedIn `/jobs/view/<id>`, Indeed `jk=<id>`; for other links compare without query string or fragment.

If a row matches, say "Already in Notion", show that row (Company, Role, Status, Link) and stop. Write nothing.

## Step 4: Ask once

Show the row to be added (Company, Role, Source, Link, Deadline) and ask: "Add it as **Drafting** or **Applied**? (Applied uses today's date unless you give another.)" Then end the turn and wait for the answer.

## Step 5: Add the row

After the answer, call `notion-create-pages` into Job Applications (parent `data_source_id` from `docs/NOTION.md`, `allow_async: false`) with:

- Company, Role, Source, Link, Status
- Applied date, only if Applied: the given date, or today in Asia/Manila (get it from `date -u`, converted; never guess). Dates use `date:<Prop>:start` with `is_datetime` 0.
- Deadline, if any
- Needs a look `__NO__`

Leave Local folder, Sender domains, Next action and Reminder event IDs empty.

Then say the row was added. If Applied, add: "The next /jobmail live run adds the 7-day follow-up." If a Deadline was set, add: "...and a deadline reminder."

## Privacy

Never paste the posting into Notion or files; write only the fields above.
