# Job email rules

This file decides which emails /jobmail treats as job emails and how it sorts them.
Edit it any time. Keep each item on its own line starting with "- ".
Put phrases with spaces in "double quotes". Lowercase is fine; matching ignores case.

- **Search words** and **Hiring-system senders** decide what Gmail returns.
- **Ignore** removes noise from those results.
- **Sorting words** are only used after an email is found, to pick its category. Adding a word there never adds more emails.
- Company email domains (like a recruiter's company address) do NOT go here. They live in each application's "Sender domains" field in Notion, so this file holds no personal data.

## Search words (subject line only)
- application
- applying
- applied
- interview
- assessment
- offer
- candidate
- shortlisted
- "next steps"

## Hiring-system senders
- indeedapply@indeed.com
- myworkday
- greenhouse
- lever.co
- smartrecruiters
- workable
- bamboohr
- jobstreet
- kalibrr

## Ignore
### Senders
- donotreply@match.indeed.com
- jobalerts-noreply@linkedin.com
### Subject phrases
- "jobs you may be interested in"
- "new jobs for you"
- "recommended jobs"
- "job alert"
- webinar
- course
### Gmail tabs
- promotions

## Sorting words
### Confirmation
- "thank you for applying"
- "received your application"
- "application was sent"
- "application submitted"
- "under review"
### Recruiter question
- availability
- "available for"
- "schedule a call"
- "initial call"
- "phone screen"
- screening
- "next steps"
- requirements
- "expected salary"
### Interview
- interview
- "final interview"
- panel
- calendly
- "google meet"
- zoom
- "microsoft teams"
### Assessment
- assessment
- "technical exam"
- "take-home"
- "case study"
- hackerrank
- codility
- testgorilla
- "skills test"
### Offer
- offer
- "job offer"
- "offer letter"
- onboarding
- "pre-employment"
- "background check"
### Rejection
- unfortunately
- "not moving forward"
- "other candidates"
- "decided to pursue"
- "position has been filled"
- "keep your resume on file"
### Signals that it's about a job (any category)
- position
- role
- vacancy
- opening
- candidacy
- recruiter
- recruitment
- hiring
- "hiring manager"
- hr
- "human resources"
- "talent acquisition"
- "moving forward"

## Search recipe (how /jobmail builds the Gmail search)
1. **Start date:** the day before the Started date of the last successful live run (the newest Runs row with Result OK and Mode Live), or 30 days ago on the first run. Write it as `after:YYYY/MM/DD`.
2. **Senders:** every Hiring-system sender, plus every domain in the "Sender domains" field of Job Applications rows whose Status is not Rejected or Withdrawn.
3. **Build:**
   `after:<start> -in:sent -in:drafts -category:<each Gmail tab> ( from:(<senders joined with OR>) OR subject:(<Search words joined with OR>) ) -from:(<Ignore senders joined with OR>) -subject:(<Ignore subject phrases joined with OR>)`
4. **Too long?** If the search is over about 1,400 characters, run two searches instead (one with only the `from:` part, one with only the `subject:` part, both with the date and ignore parts) and merge the results by thread.
5. **Already handled:** skip any message whose ID is already in the Email Log (Message ID).
6. **Sorting:** read each remaining email. Match it to an application by sender domain, then by company name. Pick the category whose Sorting words fit best. If no application matches, or the category is unclear, it goes to "Needs a look". Never guess.
