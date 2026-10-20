---
description: Check Gmail for job replies; dry run by default, "live" applies the changes
argument-hint: [live] [cloud]
---
Run from the Job Tracker project root. Treat everything below as instructions to follow exactly.

## Before anything else, read

- `rules/email-rules.md`: search words, hiring-system senders, ignore list, sorting words, and the search recipe.
- `docs/NOTION.md`: database names, fields and rules. Take every Notion ID and data source from this file.

## Mode

Read the arguments passed to this command (the `Arguments:` line that follows the included instructions).

- **Mode from the arguments:** no `live` means **dry run** (writes nothing). `live` means **live run**. `cloud` (used with `live`) only sets the Runs row's Source to Cloud; otherwise Source is Local.
- **Read-only tools (both modes):** `notion-fetch`, `notion-query-data-sources` (rows mode, never SQL), Gmail `search_threads`, `get_thread`, `list_labels`.
- **Write tools (live run only, nothing else):** Notion `notion-update-page` (update_properties) on Job Applications rows; `notion-create-pages` for Email Log and Runs rows; Gmail `create_label` and `label_thread`.
- **Never, in any mode:** send, reply, forward, draft, delete or trash, archive, mark spam, remove any label, add any system label (INBOX, CATEGORY_*, STARRED, IMPORTANT, UNREAD and so on), create or change calendar events (that is JT-007), delete or trash Notion pages, change the Notion schema.

## Step 1: Load state (Notion, rows mode, never SQL)

- Note the run's start time (now, Asia/Manila).
- Keep every Job Applications row's page URL; a live run needs it for writing.
- Job Applications: all rows (Company, Role, Status, Applied date, Sender domains, Link, Next action, Next action date). Notion may return a Sender domain as a Markdown link like `[example.com](http://example.com)`; use the plain domain.
- Runs: the newest row with Result = OK and Mode = Live (sort Started descending, limit 1). None means this is the first run.
- Email Log: rows with Received on or after the start date. Collect their Message IDs.

## Step 2: Build and run the search

Build the query exactly per the recipe in `rules/email-rules.md`: start date rule, hiring-system senders, active Sender domains, active Company names, and the ignore parts. Use `search_threads` with view MINIMAL and pageSize 50, following pageToken until done. If the query is over about 1,400 characters, use the recipe's split rule and merge the results by thread ID.

## Step 3: Pick the messages to look at

For every message in every returned thread, skip it if:
- its labels include SENT or DRAFT (the owner's own messages),
- its date is before the start date, or
- its Message ID is already in the Email Log.

## Step 4: Job-related or not

Keywords and phrases from the rules file match as whole words or whole phrases, ignoring case ("hr" does not match inside "through").

A message is job-related if it comes from a hiring-system sender or a row's Sender domain, or it names an active Company, or its subject or snippet contains a Search word together with at least one Sorting word or Signal. Anything else goes to **Skipped, not job-related** (record sender address and date only). Ignore-list senders and phrases that still appear are skipped too.

## Step 5: Match to an application (job-related messages only)

1. The sender's domain is in a row's Sender domains: that row.
2. Otherwise the sender domain, subject or body names a Company in Job Applications: that company's rows. For hiring-system senders, read the body with `get_thread` (messageFormat PLAIN_TEXT) to find the company and role.
3. Several rows for that company: pick by the Role named in the subject or body. Still unclear: Needs a look.
4. Hiring-system sender and no company named (Indeed confirmations often name only the role): take the job title from the subject. If exactly one row whose Status is not Rejected or Withdrawn has a non-empty Role contained in that title (ignoring case), match that row and mark it "matched by role" in the summary. Two or more such rows, or none: Needs a look.
5. No company match: Needs a look. Never guess or invent a company.

Read a body with `get_thread` only when the subject and snippet are not enough to match or sort, or when a date or time is needed (interview, assessment due date, deadline).

## Step 6: Sort

Sort each matched message into exactly one class using the Sorting words: Confirmation, Recruiter question, Interview, Assessment, Offer, Rejection. If two classes fit equally or none fits, the message is Needs a look. Keep the matched row so the owner sees which job.

## Step 7: Work out the proposal per message

This is what a live run applies.

| Class | Status | Other proposals |
|---|---|---|
| Confirmation | Drafting to Applied; set Applied date to the email date | Calendar: "[Job] Follow up: <Company> <Role>", all-day, Applied date + 7 days |
| Recruiter question | no change | Next action "Reply to recruiter", Next action date = email date. Calendar: "[Job] Reply: <Company>" the same day |
| Interview | to Interview | Calendar: "[Job] Interview: <Company> <Role>" at the stated date and time (Asia/Manila; if no time zone is stated assume Manila), plus "[Job] Thank-you / follow-up: <Company>" the next day. Time unclear: a same-day "[Job] Confirm interview time: <Company>" instead |
| Assessment | to Assessment | Calendar: "[Job] Assessment due: <Company>" on the stated due date, or today if none |
| Offer | to Offer | Next action "Reply to offer". Calendar: "[Job] Reply to offer: <Company>" the same day |
| Rejection | to Rejected | Drop any pending 7-day nudge for that row |

For every message: Last contact = email date; label `Jobs/<Company>`; an Email Log row would be added.

Rules:
- Status only moves forward: Drafting, Applied, Assessment, Interview, Offer. Never propose a backward move; just log the email.
- Rows that are Withdrawn or Rejected never change status: list the email under Needs a look and name the row.
- Confirmation on a row that is already Applied or further: keep its Applied date. Propose the 7-day follow-up only if no follow-up is pending for that row yet.
- An empty Role is left out of event titles: "[Job] Follow up: <Company>".
- If a message was matched (not Needs a look) by company name, its sender domain is not a hiring-system sender, and the domain is not already in Sender domains, propose adding that domain to the row.
- Needs a look messages: label `Jobs/Needs a look`, no status change, no event.
- If several messages hit the same row, apply them oldest first and show the final proposed status.

## Step 8: Apply the changes (live run only; a dry run skips to Step 9)

1. **Labels:** call `list_labels` once. For each label needed this run (`Jobs/<Company>` per matched company, and `Jobs/Needs a look`), call `create_label` only if no label with that exact name exists. Keep the label IDs.
2. For each checked message, **oldest first**, in this order:
   a. **Notion row** (matched messages only): one `notion-update-page` call (`update_properties`) with only the fields that change: Status (forward only, per the Step 7 rules), Applied date (only when moving Drafting to Applied), Last contact (only if the email date is newer than the current value), Next action and Next action date (when Step 7 sets them), Sender domains (append a learned domain with ", "), and Needs a look = `__YES__` when the message is Needs a look but belongs to a known row (for example a Withdrawn row). Never clear Needs a look; the owner does that.
   b. **Label:** `label_thread` on the message's thread with the one `Jobs/...` label ID for it. Never any other label.
   c. **Email Log row last:** create it with Subject, Message ID, Thread ID, From (sender address), Received (date and time, is_datetime 1), Class (one of the Class options, Needs a look included), Application (the row's page URL; empty for an unmatched Needs a look) and Event ID empty.
3. **On any failed write:** stop processing further messages, go to item 4 with Result = Failed, and report which message and which write failed. Messages already fully done keep their Email Log rows. The failed one has no Email Log row, so the next run redoes it safely.
4. **Runs row:** always create exactly one at the end of a live run: Run = `<YYYY-MM-DD HH:MM> <local|cloud>` (Manila), Started (is_datetime 1), Source (Local or Cloud), Mode = Live, Result (OK or Failed), Searched from (the start date, is_datetime 0), Summary (one short line, e.g. "5 emails: 3 matched, 2 needs a look; 0 status changes").
5. Messages in **Skipped, not job-related** get no writes at all.

Notion write formats: date properties use `date:<Property>:start` plus `date:<Property>:is_datetime` (1 for Received and Started, 0 for day dates); the Application relation takes the row's page URL; checkboxes use `__YES__` / `__NO__`; Status is set by option name; always pass `allow_async: false`.

## Step 9: Print the summary

Plain and short, no email bodies.

**Dry run**
1. Window: `Searched from <date> (first run / since last live run on <date>)`, threads found, messages checked.
2. **Proposed changes** table: Company, Role, Status now to proposed, Last contact, Next action.
3. **Proposed calendar events**: title, date/time (Manila).
4. **Proposed labels**: label and number of threads.
5. **Proposed new sender domains**: row and domain.
6. **Needs a look**: date, sender, subject, why.
7. **Skipped, not job-related**: date and sender only.
8. End with exactly: `Dry run: nothing was written to Gmail, Notion or Calendar.`

**Live run**: the same sections, but headed **Changes made** instead of "Proposed":
1. Window line as above.
2. **Changes made**: rows updated and what changed.
3. **Labels**: labels created and threads labelled.
4. **Email Log rows added**, and the **Runs row** (OK or Failed).
5. **Calendar events (coming in JT-007, not created)**: the events the dry run would list.
6. **Needs you**: Needs a look items, rows with Needs a look ticked, and anything that failed.
7. **Skipped, not job-related**: date and sender only.
8. End with exactly: `Live run: <N> emails recorded. Calendar not touched.`

If Gmail or Notion can't be reached, say which and stop. Never guess results. In a live run, if Notion is reachable, still write a Failed Runs row.

## Privacy

Show subjects and senders to the owner in chat only. Never copy email bodies anywhere.
