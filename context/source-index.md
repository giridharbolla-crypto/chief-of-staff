# Source index — Gmail queries COS uses

These are the Gmail search queries the skills rely on. All are rooted in
`giridhar.bolla@gmail.com`.

## Financial transactions
Query: `(from:hsbc OR from:hdfc OR from:hdfcbank) newer_than:7d -subject:OTP`
Exclude also: "Investment Weekly", "India Brief", credit-card promo subject lines.
Senders of interest:
- `alerts@hdfcbank.bank.in` (HDFC transaction alerts)
- `hsbc@hsbc.bank.in` (HSBC transaction alerts)
- `hsbc@mail.hsbc.bank.in` (HSBC mobile banking logins — suppress unless an
  unknown device appears)
- `cmpadh423_*@custcomm.hsbc.bank.in` (HSBC servicing notices — rate changes
  matter, promos do not)
- `hdfcmf@camsonline.com` (HDFC MF statements)

## Securities activity
Query: `(from:cdsl OR from:nsdl OR from:rathi OR from:indmoney OR from:indstocks) newer_than:7d`
Senders of interest:
- `transactions@transactions.indmoney.com` — actual buys/sells, dividends
- `statements@transactions.indmoney.com` — contract notes, margin statements
- `insights@updates.indmoney.com` / `updates@updates.indmoney.com` — IND Daily
  newsletter (summarise, don't list)
- `donotreply.evoting@cdslindia.co.in` — CDSL e-voting (always flag)
- `donotreplypms@rathi.com` — Anand Rathi PMS reports

## Job applications
Query:
`subject:(application OR applied OR "thank you for applying" OR interview OR assessment OR "take-home" OR "next steps") newer_than:30d`
Plus senders:
- `jobs-noreply@linkedin.com` for application sends
- `no-reply@eu.greenhouse-mail.io`, `*@greenhouse.io`, `*@lever.co` for ATS
  replies
- Direct recruiter domains as they appear

Dedupe LinkedIn "View X jobs and your next steps" — those are prompts, not
applications.
