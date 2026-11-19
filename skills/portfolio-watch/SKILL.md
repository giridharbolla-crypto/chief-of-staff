---
name: portfolio-watch
description: Summarise securities activity across INDmoney (Indian + US stocks), Anand Rathi PMS, CDSL, and NSDL. Use when the user asks about trades, dividends, IPOs, NFOs, contract notes, PMS statements, or corporate actions / e-voting.
---

# When to use
- Weekly brief (Monday 07:38 IST).
- Ad-hoc: "anything on my investments?", "any trades last week?", "any
  corporate actions?", "any IPOs open?".

# Inputs
- Gmail (read-only).
- INDmoney connector (read-only): `networth_snapshot`, `networth_holdings`,
  `mf_sips` / `indian_stocks_sips` / `us_stocks_sips`.
- `memory/portfolio-snapshots.md` — last week's asset-class totals.
- Default window: last 7 days unless user says otherwise. For PMS reports
  and corporate actions, look back 30 days.

# Steps
1. Run the Securities activity query from `context/source-index.md`.
2. Classify each hit:
   - `trade` — order confirmation (buy/sell), contract note
   - `dividend` — dividend received
   - `statement` — weekly / monthly funds + securities statement, PMS report
   - `ipo_nfo` — IPO or NFO open or closing
   - `corp_action` — CDSL / NSDL e-voting, bonus, split, merger
   - `newsletter` — IND Daily and similar (always drop)
3. Flag as ⚠️ urgent: any e-voting with deadline < 7 days; any IPO closing
   today or tomorrow; any trade with amount unusual for the user
   (define "unusual" as > 2× the 30-day median trade size).
4. For PMS reports, link the month and note it as unread.
5. Pull `networth_snapshot`. Compare totals per asset type with the last
   entry in `memory/portfolio-snapshots.md` and report week-on-week change.
   Then append this week's totals (asset-type level only, no holdings) to
   that file with the date.
6. Pull `networth_holdings` for IND_STOCK, MF, US_STOCK. Flag ⚠️ any single
   holding that moved more than 10% on the week or is > 15% of its asset type.
7. Note SIPs debiting in the next 7 days (one line total, not per SIP).
8. Observations only — never recommend buying or selling. Never call
   `save_analysis`.

# Output
```
## 📈 Portfolio watch — <window>

### ⚠️ Action needed
- <e-voting deadline, IPO closing, unusual trade>

### Portfolio (INDmoney)
- Net worth ₹<x> (<+/-y> vs last week) · Equity <a>% · Retirement <b>% · ...
- <biggest mover or concentration flag, one line each, max 3>

### Trades
<table: Date / Platform / Ticker / Side / Qty / Price / Amount>

### Dividends
<one-liner per dividend>

### Statements & reports
- <PMS report, weekly statements — one line each>

### Open IPOs / NFOs
<one-liner each with close date>
```

Under 25 lines total. If nothing material, say "Quiet week — no trades, no
corporate actions." and stop.

# Approvals
- Never place a trade.
- Never respond to CDSL e-voting on his behalf.
- Never open password-protected PDFs by guessing the password. Tell the user
  which report is available and that the password is his PAN / DOB combo per
  standard broker format (do not spell out the actual combo).
