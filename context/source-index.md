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

## INDmoney connector (live portfolio data, read-only)
Use the connector, not emails, for current holdings and values:
- `networth_snapshot` — totals by asset type, asset-class split, market-cap
  split of direct stocks, liabilities incl. credit card dues and due dates
- `networth_holdings` (asset_type IND_STOCK / MF / US_STOCK / PMS ...) —
  per-holding value, P&L, one-day change
- `networth_allocation_breakdown` — allocation within one asset type
- `mf_sips`, `indian_stocks_sips`, `us_stocks_sips` — upcoming SIP debits
Caveats: Indian stock invested value is only accurate for INDmoney-held
stocks, so P&L on externally held stocks is unreliable — don't quote it.
Sector breakdown is not available. Never call `save_analysis`.
Emails remain the source for trades, contract notes, dividends, PMS reports
and CDSL/NSDL corporate actions.
