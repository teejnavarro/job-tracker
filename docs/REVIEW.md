# Review: Job Tracker
Date: 2026-11-14 (third pass, same day)

Checked against `docs/BRIEF.md` (incl. the 2026-10-19 amendment). Evidence comes from `notes/TRIAL.md`, `README.md` Log, `docs/ROUTINE.md`, `docs/NOTION.md`, `.claude/commands/jobmail.md`, `.claude/commands/jobadd.md`, `rules/email-rules.md`, the Resume Tailor `docs/TAILORING.md`, the archived `.gsd` logs, and `.gsd/jobmail-status-count-review.md` (JT-012). The reviewer had no Notion, Gmail or Calendar access, so live data was not re-read. Notion and calendar claims rest on the 2026-11-14 check in `notes/TRIAL.md`.

**Since the last pass:**
- The owner said the run (not he) moved the interview row ("The run did it"). Recorded in `notes/TRIAL.md` and the README Log, and accepted as evidence.
- The owner said "Fix it" about the summary undercount. JT-012 changed `.claude/commands/jobmail.md`, and it was merged as PR #1 (36a6bbc). Integrator review: 0 blockers, 0 important, 3 suggestions.

| Success criterion | Result | Evidence |
|---|---|---|
| A real recruiter email turns into the right Notion status + calendar event with no manual steps | Met | 2026-11-04 interview email: the `2026-11-04 17:07 cloud` run moved the row to Interview (owner statement), created one `[Job] Confirm interview time` event, and dropped the nudge. 2026-11-06 rejection: the 17:08 cloud run moved Applied to Rejected and deleted the nudge. |
| Unsure matches land in "Needs a look", never a wrong status | Met | Withdrawn-row interview invites (2026-10-19) and a "job was closed" email (2026-10-21) went to Needs a look. The 2026-11-03 availability email also went to Needs a look (missed as Recruiter question, but no wrong status). |
| Re-running the check creates no duplicates | Met | JT-006 run 2: 0 changes; JT-007 run 2: 0 events; overlapping JT-009 cloud runs; runs after 2026-11-04/05/06 found threads already logged. Both interview event IDs exist once in the calendar. |
| A newsletter / job alert never creates an event | Met | Job-match senders and alert phrases are in the Ignore list (`rules/email-rules.md`); a non-job notification was "Skipped, not job-related"; jobmail.md Calendar rules forbid events for these. |
| The cloud routine runs while the owner's PC is off | Met | `.gsd/archive/cloud-routine-log.md`: scheduled runs 2026-10-22/23 with Cloud OK Runs rows; owner confirmed a run with the PC off. Daily cloud runs continue into November. |

### Deliverables

| Deliverable | Result | Evidence |
|---|---|---|
| 1. Notion Job Applications database, 4 rows imported, TRACKER.md retired | Met | `docs/NOTION.md`; README Log JT-001 to JT-003. Board-first tab is still an open owner step. |
| 2. /tailor adds a Notion row (Drafting) | Met | Resume Tailor `docs/TAILORING.md` step 2; PR #11 merged. |
| 3. /jobmail on demand: Notion, labels, calendar | Met | `.claude/commands/jobmail.md` (dry run default, `live` writes). |
| 4. Cloud routine twice a day | Met (time changed) | `docs/ROUTINE.md`: 08:07 and 17:07 Manila, owner-approved in JT-009. Brief still says 08:00 and 17:00. |
| 5. Editable keyword/sender list | Met | `rules/email-rules.md`. |
| Amendment: /jobadd | Met | `.claude/commands/jobadd.md`; README Log 2026-10-26. |
| Amendment: Assessment status; confirmation moves Drafting to Applied | Met | `docs/NOTION.md` Status options; jobmail.md Step 7. |

## Scope check
- Nothing out of scope found. JT-012 only touched the summary wording in jobmail.md; it traces to the owner's "Fix it".
- **"Short summary after each run (what changed, what needs the owner)": Met in the instructions, not yet seen live.** Step 8 item 2a now keeps a list of every Status write, Step 8 item 4 builds the Runs Summary from that list with no copyable literal count, and Step 9 item 2 shows the same count. That fixes the likely cause: the old example line "0 status changes" could be copied. It still needs the next real email that changes a status to prove it.
- In scope but built and not yet seen live: timed interview event, next-day interview follow-up, same-day recruiter-reply reminder, assessment reminder, offer reminder, deadline reminder (2 days before).
- Interview times sent only as a calendar attachment are not read by /jobmail, so those fall to the "Confirm interview time" path.
- Schedule differs from the brief text (08:07/17:07 vs 08:00/17:00). Owner-approved, but the brief was not amended.
- Privacy: no owner email or application company names found in tracked files (`.gsd/` and `backlog/` are git-excluded).

## Issues to fix
None blocking. Watch items and tidy-ups, for the owner to decide:
1. **Confirm the summary fix live.** On the next run that changes a status, check that the Runs row Summary names the change and the count matches Notion.
2. **JT-012 wording suggestions left open** (`.gsd/jobmail-status-count-review.md`): N, M and L in the Summary format are not defined; two messages on one row count as two moves, which is not stated; Step 9 says "any notification" while line 19 says never send notifications (the cloud routine prompt sends one).
3. **Stale notes:** `notes/TRIAL.md` still says the undercount is "Reported, not fixed" (Owner statement paragraph and Closing). The README JT-010 Log line still lists the interview-row question as unproven; later lines answer it.
4. **Recruiter availability email went to Needs a look.** Safe per criterion 2, but same-day reply reminders may rarely fire for a recruiter not yet on the row.
5. **Open owner steps in README:** drag the Board tab first, and untick Needs a look on the withdrawn row.
6. **Brief wording:** amend the schedule line to 08:07 and 17:07.
7. Minor, outside this repo: /tailor's duplicate check uses a capped Notion rows query. It falls back to a "Tracker row not added" note, so the risk is low.

## Verdict
Ready to wrap up. All five success criteria, all deliverables and the in-scope run summary are met. The summary fix still needs its first live confirmation (issue 1). The owner confirms done.
