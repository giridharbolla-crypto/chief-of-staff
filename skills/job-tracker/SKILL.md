---
name: job-tracker
description: Track job applications in flight and recruiter conversations. Use when the user asks about applications, interviews, recruiter replies, "where am I with X company", or Friday wrap-up on his search.
---

# When to use
- Weekly wrap-up (Friday 17:38 IST).
- Ad-hoc: "where are my job apps?", "any interview updates?", "who should I
  follow up with?".

# Inputs
- Gmail (read-only).
- Google Doc `cos/job-pipeline.md` (COS maintains this; create if missing).

# Steps
1. Run the Job applications query from `context/source-index.md` over the
   last 30 days.
2. Group hits by company. For each company extract:
   - Role name
   - Application date (first ack email)
   - ATS / source (LinkedIn, Greenhouse, Lever, direct)
   - Last activity date
   - Status: `applied` / `recruiter_reached_out` / `screen_scheduled` /
     `interview_scheduled` / `rejected` / `offer` / `ghosted` (>14 days
     silent since last activity)
3. Update `cos/job-pipeline.md` with any new companies or state changes.
4. Suggest a next step for each active row:
   - `applied` + silent 7–14 days → "wait"
   - `applied` + silent > 14 days → "consider a short follow-up"
   - `recruiter_reached_out` with no reply → "draft reply or decline"
   - `screen_scheduled` → "prep notes in <N> days"
5. Dedupe LinkedIn "view X jobs" nudges — those are not applications.

# Output
```
## 💼 Job tracker — <date>

### Active pipeline
| Company | Role | Applied | Last activity | Next step |
|---|---|---|---|---|

### Needs your decision
- <recruiter outreach that's gone unresponded>

### Gone quiet (> 14 days)
- <company — days silent — suggested action>
```

Under 20 lines. Do not include ghosted applications older than 60 days
unless user asks. Never auto-send a follow-up email — draft only.

# Approvals
- Draft follow-ups and recruiter replies in Giridhar's voice. Always ask
  before sending.
- Never commit to interview times. Propose two slots and surface his
  calendar for conflicts.
