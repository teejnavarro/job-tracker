# Job Tracker

**Status:** Active
**Started:** 2026-10-19

## Goal
After /tailor, track every application in Notion and watch Gmail for replies, so no recruiter email, interview or deadline gets missed.

## Scope
- In: Notion "Job Applications" database; /tailor adds a Notion row; /jobmail Gmail check (read + labels) that updates Notion and adds Google Calendar reminders; cloud routine twice a day (08:00 and 17:00 Manila); editable keyword/sender list
- Out: sending, replying, drafting or deleting email; applying to jobs; PDFs in Drive/Notion; Google Docs; cover letters

Full brief: `docs/BRIEF.md`.

## Next steps
- [x] `/pm` backlog JT-001..011 approved
- [x] JT-001 Notion "Job Tracker" page + 3 databases (see `docs/NOTION.md`)
- [ ] Owner: in Notion, drag the "Board" tab before "All" in Job Applications
- [x] JT-002 4 applications imported to Notion; TRACKER.md has a banner (retires in JT-003)
- [x] JT-003 /tailor adds the Notion row (Resume Tailor PR #11); TRACKER.md is a local pointer
- [x] JT-004 `rules/email-rules.md` (search words, hiring senders, ignore list, sorting words, search recipe)
- [ ] `/architect Work on JT-005`

## Links
- Brief: `docs/BRIEF.md`
- Related: `10-Active/2026-10_resume-tailor` (/tailor, current TRACKER.md)

## Log
- 2026-10-19 - Brief approved. Project created.
- 2026-10-19 - /pm backlog approved: 11 stories in 5 epics (JT-001..011). Brief amended: /jobadd added, /tailor option A confirmed, Assessment status.
- 2026-10-19 - JT-001 done: private Notion "Job Tracker" page with Job Applications, Email Log and Runs databases; QA passed (Runs ID typo fixed); merged to main locally (f4af7c8). Owner step: drag Board tab first.
- 2026-10-19 - JT-002 done: 4 applications imported to Notion (2 Applied, 2 Withdrawn), owner confirmed; TRACKER.md banner added, retirement moved to JT-003.
- 2026-10-19 - JT-003 done: Resume Tailor PR #11 merged (bce1d39). /tailor adds/updates the Notion row (never changes Status on an existing row); TRACKER.md retired to a local pointer.
- 2026-10-20 - JT-004 done: `rules/email-rules.md` added (search words split from sorting words; company domains stay in Notion); QA passed incl. a read-only 7-day Gmail check; merged to main locally (e2c9e4d).
