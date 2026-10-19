# PROJECT BRIEF — Job Tracker

**Status:** APPROVED by owner in Claude Code, 2026-10-19.
**Track:** Code (set at intake: commands, matching rules, cloud routine and Notion setup).

**Goal:** After /tailor, track every application in Notion and watch Gmail for replies, so no recruiter email, interview or deadline gets missed.

**Final deliverable:**
1. Notion "Job Applications" database (current 4 rows imported from resume-tailor `applications/TRACKER.md`; TRACKER.md retired)
2. /tailor updated to add a Notion row (status Drafting) instead of a TRACKER.md row
3. /jobmail command: on-demand Gmail check → update Notion, label, calendar events
4. Cloud routine running the same check twice a day (08:00 and 17:00 Asia/Manila)
5. A keyword/sender list file (editable by the owner) that drives the email matching

**Audience:** Owner only.

## In scope
- Read Gmail; add labels `Jobs/<Company>` and `Jobs/Needs a look`
- Classify replies: confirmation, recruiter question, interview, assessment, offer, rejection
- Update Notion status, last-contact date, next action + date
- Google Calendar events (Asia/Manila) on `teejnavarro@gmail.com`, titled "[Job] ...":
  - interviews at the stated time
  - same-day reply reminders (due within 24 hours) when a recruiter emails
  - next-day follow-up after an interview
  - 7-day nudge when an application gets no reply
  - deadline reminders 2 days before
- No duplicate events or rows when the check runs again
- Short summary after each run (what changed, what needs the owner)

## Out of scope
- Sending, replying, drafting or deleting any email; marking spam
- Applying to jobs; storing PDFs in Drive or Notion (PDFs stay local)
- Google Docs; cover letters; the Ppoppo and Holidays calendars

## Expectations
- Plain, short summaries; Notion database easy to scan (board by status)

## Constraints
- Free tools only; existing connectors (Gmail, Notion, Google Calendar)
- Personal data never committed to a public repo

## Success criteria
- A real recruiter email turns into the right Notion status + calendar event with no manual steps
- Unsure matches land in "Needs a look", never a wrong status
- Re-running the check creates no duplicates
- A newsletter / job alert never creates an event
- The cloud routine runs while the owner's PC is off

## Plan
1. Notion database + import
2. /tailor hook
3. Keyword list + /jobmail
4. Trial on the real inbox (read-only first, then with labels/events)
5. Cloud routine
6. /review-brief

## Open questions (answered by owner at approval)
- Follow-up timings: as proposed (same day / next day / 7 days / 2 days)
- Cloud routine instructions repo: a new **private** GitHub repo
- Notion database location: a new **private** Notion page

## Assumptions
- Project folder `10-Active/2026-10_job-tracker`; the /tailor change is a small PR to the archived resume-tailor repo (github.com/teejnavarro/resume-tailor)
- Events go on the owner's main Google Calendar (`teejnavarro@gmail.com`). That calendar is set to UTC, so every event sets Asia/Manila explicitly
- Gmail checks look back to the last successful run (first run: last 30 days)

## Starting keyword / sender list (from the approval chat)
- **Status:** application, applied, "thank you for applying", "received your application", candidate, candidacy, position, role, vacancy, opening, shortlisted, "next steps", "moving forward", "under review"
- **Interview:** interview, "phone screen", screening, "initial call", "schedule a call", availability, "available for", Calendly, "Google Meet", Zoom, "Microsoft Teams", "final interview", panel
- **Assessment:** assessment, "technical exam", "take-home", "case study", HackerRank, Codility, TestGorilla, "skills test"
- **Offer / HR:** offer, "job offer", "offer letter", HR, "human resources", "talent acquisition", recruiter, recruitment, hiring, "hiring manager", onboarding, "pre-employment", requirements, "background check"
- **Rejection:** "unfortunately", "not moving forward", "other candidates", "decided to pursue", "position has been filled", "keep your resume on file"
- **Hiring-system senders:** myworkdayjobs, greenhouse, lever, smartrecruiters, workable, bamboohr, jobstreet, kalibrr, indeed (application updates), linkedin (application updates only)
- **Ignore:** job-alert newsletters ("jobs you may be interested in", "new jobs for you", "recommended jobs"), webinar and course promos
- Unsure matches go to "Needs a look", never a guessed status
