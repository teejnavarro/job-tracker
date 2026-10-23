# Live inbox trial (JT-010)

## Purpose
Record how the full flow (Gmail, Notion, Calendar, twice-daily cloud routine) has behaved on the owner's real inbox, and what is still to be seen live before JT-010 is closed.

## Results so far
Status as of 2026-10-23.

| Criterion | Status | Evidence |
|---|---|---|
| Dry run on the real inbox reviewed with the owner; misclassifications fixed in the keyword list | Met | Dry runs on 2026-10-20 and 2026-10-21 were reviewed with the owner. Fixes below (JT-004 QA, JT-005, JT-009). |
| Write run: a real recruiter email becomes the right Notion status and calendar event with no manual steps | Waiting | No recruiter, interview, assessment, offer or rejection email for an active row has arrived yet. The paths are built but not seen live. |
| An unsure email lands in Needs a look | Met | 2026-10-19: interview invites for a withdrawn application. 2026-10-21: an Indeed "job was closed" email for an applied row (no sorting word fits). Both went to Needs a look. |
| A second run straight after creates no duplicates | Met | JT-006 live run 2 made 0 changes. JT-007 live run 2 made 0 events. JT-009: two overlapping cloud runs, the second saw the email already logged. Daily cloud runs since have made no duplicates. |
| A job alert or newsletter creates no event | Met | Daily Indeed job-match emails are excluded by the ignore list. A GitHub notification was classified "Skipped, not job-related". No event was created for either. |
| Results written to the project notes, then `/review-brief` | Waiting | This file is the notes. `/review-brief` runs after the open check below passes. |

## Fixes made during the trial
- 45593cd: match the hiring-system sender `myworkdayjobs` and sort "shortlisted" emails (JT-004 QA).
- 1b3db22 and 75228b4: match hiring-system emails by role, recognise "has been submitted" confirmations, match whole words only, and require a non-empty Role (JT-005).
- 7efb296: exclude only the Promotions tab in Gmail searches (JT-009).
- 4266447: read the run time from the clock, never guess it (JT-009).

## Decisions
- 2026-10-23: Indeed "job was closed" emails stay in Needs a look. They are not treated as a rejection.
- 2026-10-23: no test drill (no temporary row, no test email). Wait for a real recruiter-type email instead.

## Still to see live
The email-driven paths: status move, interview event plus next-day follow-up, reply/assessment/offer reminders, and removal of the 7-day nudge. When a real recruiter, interview, assessment, offer or rejection email is handled by a run, check:
- The run that handled it has a Cloud or Local OK row in Runs.
- The row's Status moved as the Step 7 table says (forward only).
- The expected `[Job]` event(s) exist exactly once: the interview at the stated time plus a next-day follow-up, or the reply, assessment or offer reminder.
- The row's 7-day nudge was deleted and marked `nudge:dropped` if the email was a reply.
- A second run made no new changes.

## Closing
When the open check passes, mark JT-010 done and run /review-brief.
