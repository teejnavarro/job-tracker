# Review: Job Tracker
Date: 2026-11-14 (re-review, same day)

Checked against `docs/BRIEF.md` (incl. the 2026-10-19 amendment). Evidence comes from `notes/TRIAL.md`, `README.md` Log, `docs/ROUTINE.md`, `docs/NOTION.md`, `.claude/commands/jobmail.md`, `.claude/commands/jobadd.md`, `rules/email-rules.md`, the Resume Tailor `docs/TAILORING.md`, and the archived `.gsd` logs. The reviewer had no Notion, Gmail or Calendar access, so live data was not re-read. Notion and calendar claims rest on the 2026-11-14 check in `notes/TRIAL.md`.

**What changed since the first review:** the owner answered issue 1 ("The run did it"), recorded in `notes/TRIAL.md` under "Owner statement (2026-11-14)" and in the README Log. This is the owner's own account, not a Notion page-history check. It is accepted as evidence.

| Success criterion | Result | Evidence |
|---|---|---|
| A real recruiter email turns into the right Notion status + calendar event with no manual steps | Met | 2026-11-04 interview email: the `2026-11-04 17:07 cloud` run moved the row to Interview (owner statement), created one `[Job] Confirm interview time` event, and dropped the nudge. 2026-11-06 rejection: the 17:08 cloud run moved Applied to Rejected and deleted the nudge. `notes/TRIAL.md`, "Open check result" and "Owner statement". |
| Unsure matches land in "Needs a look", never a wrong status | Met | Withdrawn-row interview invites (2026-10-19) and a "job was closed" email (2026-10-21) went to Needs a look. The 2026-11-03 availability email also went to Needs a look (missed as Recruiter question, but no wrong status). |
| Re-running the check creates no duplicates | Met | JT-006 run 2: 0 changes; JT-007 run 2: 0 events; overlapping JT-009 cloud runs; runs after 2026-11-04/05/06 found threads already logged. Both interview event IDs exist once in the calendar. |
| A newsletter / job alert never creates an event | Met | Job-match senders and alert phrases are in the Ignore list (`rules/email-rules.md`); a non-job notification was "Skipped, not job-related"; jobmail.md Calendar rules forbid events for these. |
| The cloud routine runs while the owner's PC is off | Met | `.gsd/archive/cloud-routine-log.md`: scheduled runs 2026-10-22/23 with Cloud OK Runs rows; owner confirmed a run with the PC off. Daily cloud runs continue into November. |

### Deliverables

| Deliverable | Result | Evidence |
|---|---|---|
| 1. Notion Job Applications database, 4 rows imported, TRACKER.md retired | Met | `docs/NOTION.md`; README Log JT-001 to JT-003 (TRACKER.md is a local pointer). Board-first tab is still an open owner step. |
| 2. /tailor adds a Notion row (Drafting) | Met | Resume Tailor `docs/TAILORING.md` step 2; PR #11 merged. |
| 3. /jobmail on demand: Notion, labels, calendar | Met | `.claude/commands/jobmail.md` (dry run default, `live` writes). |
| 4. Cloud routine twice a day | Met (time changed) | `docs/ROUTINE.md`: 08:07 and 17:07 Manila, owner-approved in JT-009. Brief still says 08:00 and 17:00. |
| 5. Editable keyword/sender list | Met | `rules/email-rules.md`. |
| Amendment: /jobadd | Met | `.claude/commands/jobadd.md`; README Log 2026-10-26. |
| Amendment: Assessment status; confirmation moves Drafting to Applied | Met | `docs/NOTION.md` Status options; jobmail.md Step 7. |

## Scope check
- Nothing out of scope found. jobmail.md forbids sending, replying, drafting, deleting, spam, other calendars and invites. PDFs stay local.
- **In scope, partly met: "Short summary after each run (what changed, what needs the owner)".** The owner's answer confirms the run changed the interview row's status, but the run summary said "0 status changes". The same undercount shows on the 2026-10-26 rejection. So the summary misreports what changed. `notes/TRIAL.md` lists it as a known issue, not fixed.
- In scope but built and not yet seen live: timed interview event, next-day interview follow-up, same-day recruiter-reply reminder, assessment reminder, offer reminder, deadline reminder (2 days before). The rules exist in jobmail.md Step 7 and the reminders pass.
- Interview times sent only as a calendar attachment are not read by /jobmail, so those fall to the "Confirm interview time" path.
- Schedule differs from the brief text (08:07/17:07 vs 08:00/17:00). Owner-approved, but the brief was not amended.
- Privacy: no owner email or application company names found in tracked files (`.gsd/` and `backlog/` are git-excluded).

## Issues to fix
1. **Run summary undercounts status changes.** At least two live runs changed a status and reported "0 status changes". The brief asks for a summary of what changed, and the owner reads it (and the phone notification) instead of opening Notion. Fix it, or the owner accepts it as a known limit before wrap-up.
2. **Recruiter availability email went to Needs a look.** Safe per criterion 2, but same-day reply reminders may rarely fire for a recruiter whose domain and company are not yet on the row. Worth a look at why it did not match.
3. **Open owner steps in README:** drag the Board tab first (the brief expects a board by status), and untick Needs a look on the withdrawn row.
4. **Brief wording:** amend the schedule line to 08:07 and 17:07 so the contract matches what runs.
5. **Stale README line:** the 2026-11-14 JT-010 Log entry still lists "proof that the run (not the owner) moved the interview row" as not seen. The later Log line answers it; no edit is needed if the owner is fine with log entries standing as written.
6. Minor, outside this repo: /tailor's duplicate check uses a Notion rows query, which shares the capped allowance that ran out on 2026-10-20. /tailor falls back to a "Tracker row not added" note, so the risk is low.

## Verdict
Needs work, on one item only. All five success criteria and all deliverables are now met. The remaining gap is issue 1: the in-scope run summary misreports status changes. If the owner fixes it or accepts it as a known limit, the project is ready to wrap up. Issues 2 to 6 are optional tidy-ups or owner decisions.
