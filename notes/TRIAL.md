# Live inbox trial (JT-010)

## Purpose
Record how the full flow (Gmail, Notion, Calendar, twice-daily cloud routine) has behaved on the owner's real inbox. JT-010 closed on 2026-11-14: see "Open check result" below.

## Results
Status as of 2026-11-14.

| Criterion | Status | Evidence |
|---|---|---|
| Dry run on the real inbox reviewed with the owner; misclassifications fixed in the keyword list | Met | Dry runs on 2026-10-20 and the live cloud test runs on 2026-10-21 were reviewed with the owner. Fixes below (JT-004 QA, JT-005, JT-009). |
| Write run: a real recruiter email becomes the right Notion status and calendar event with no manual steps | Met | 2026-11-06: a rejection email for an applied row was handled by the 17:08 cloud run (status to Rejected, 7-day nudge removed). 2026-11-04 and 2026-11-05: two interview emails from a recruiter each got a `[Job] Confirm interview time` event. Detail and limits under "Open check result". |
| An unsure email lands in Needs a look | Met | 2026-10-19: interview invites for a withdrawn application. 2026-10-21: an Indeed "job was closed" email for an applied row (no sorting word fits). Both went to Needs a look. |
| A second run straight after creates no duplicates | Met | JT-006 live run 2 made 0 changes. JT-007 live run 2 made 0 events. JT-009: two overlapping cloud runs, the second saw the email already logged. Daily cloud runs since have made no duplicates. |
| A job alert or newsletter creates no event | Met | Daily Indeed job-match emails are excluded by the ignore list. A GitHub notification was classified "Skipped, not job-related". No event was created for either. |
| Results written to the project notes, then `/review-brief` | Met | This file is the notes. `/review-brief` run on 2026-11-14 (`docs/REVIEW.md`). |

## Fixes made during the trial
- 45593cd: match the hiring-system sender `myworkdayjobs` and sort "shortlisted" emails (JT-004 QA).
- 1b3db22 and 75228b4: match hiring-system emails by role, recognise "has been submitted" confirmations, match whole words only, and require a non-empty Role (JT-005).
- 7efb296: exclude only the Promotions tab in Gmail searches (JT-009).
- 4266447: read the run time from the clock, never guess it (JT-009).

## Decisions
- 2026-10-23: Indeed "job was closed" emails stay in Needs a look. They are not treated as a rejection.
- 2026-10-23: no test drill (no temporary row, no test email). Wait for a real recruiter-type email instead.

## Open check result (2026-11-14)
Checked from the Notion Email Log, Runs and Job Applications (`jobmail:` views only), the calendar, and the two interview emails.

Rejection email, received 2026-11-06 12:36 Manila, for a row that was Applied:
- Run row: `2026-11-06 17:08 cloud`, Live, OK. Passed.
- Status moved Applied to Rejected, as the Step 7 table says. The run summary names the change. Passed.
- Events: a rejection creates none, and none was created. Passed.
- The row's 7-day nudge was deleted and the row shows `nudge:dropped`. Passed.
- Later runs made no new changes: `2026-11-07 17:08 cloud` found the thread already logged. Passed.

Interview emails from a recruiter, received 2026-11-04 11:05 and 2026-11-05 11:27 Manila, for one applied row:
- Run rows: `2026-11-04 17:07 cloud` and `2026-11-05 17:07 cloud`, both Live, OK.
- Each email got one all-day `[Job] Confirm interview time` event (the "time unclear" path). Both event IDs are in the Email Log and exist once in the calendar.
- The row's nudge was deleted on 2026-11-04 and the row shows `nudge:dropped`.
- The runs after each (`2026-11-05 08:07`, `2026-11-06 08:09`) made no new changes.

Owner statement (2026-11-14): the run moved the interview row to Interview; the owner did not move it by hand. So the 2026-11-04 interview email changed the status, created an event and dropped the nudge in one run. The run summary still said "0 status changes", so the summary line undercounts status changes (also seen on the 2026-10-26 rejection). Fixed in JT-012 (PR #1, 2026-11-14); the fix has not been seen on a live status change yet.

Not seen live:
- A timed interview event with a next-day follow-up has not been seen. The first email proposed "today at 5PM" and was handled by the 17:07 run, after that time. The second carried its time only in a calendar attachment, which `/jobmail` does not read.
- Assessment, offer and recruiter-question reminders have not been seen live.
- The recruiter's first email (2026-11-03, asking for availability) went to Needs a look, not Recruiter question.

## Closing
The open check passed on the rejection email. JT-010 marked done on 2026-11-14. The summary undercount was fixed in JT-012. The items under "Not seen live" are reported to the owner, not fixed.
