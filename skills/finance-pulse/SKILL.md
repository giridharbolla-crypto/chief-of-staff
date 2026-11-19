---
name: finance-pulse
description: Summarise HSBC and HDFC account activity over a period. Use when the user asks about bank transactions, credit card charges, refunds, home loan or LAP changes, or any question starting with "what happened on my card / account / loan".
---

# When to use
- Weekly brief (Monday 07:38 IST).
- Ad-hoc: "what's on my cards?", "any transactions I missed?", "any refunds?".

# Inputs
- Gmail (read-only).
- INDmoney `networth_snapshot` → `liabilities.credit_cards` for card dues
  and due dates across all linked cards (read-only).
- Default window: last 7 days unless the user specifies otherwise.

# Steps
1. Run the Financial transactions query from `context/source-index.md` over
   the window.
2. For each hit, classify:
   - `charge` — transaction alert (HSBC or HDFC)
   - `refund` — explicit reversal
   - `rate_change` — home loan / LAP rate revision
   - `statement` — HDFC MF / credit card statement
   - `login` — mobile banking login (suppress unless from a new device)
   - `promo` — credit card offers (always drop)
   - `otp` — always drop
3. Ignore anything classified `otp`, `promo`, or routine `login` on known
   device (REALME RMX3990 as of Nov 2026).
4. Flag as ⚠️ urgent: any single charge > ₹25,000; any charge with no
   matching merchant Giridhar would recognise; any payment due in < 48h;
   any home loan rate change (always).
5. For each charge, pair it with any refund from the same merchant if found.
6. From INDmoney card dues: list every card with a non-zero due and its due
   date. Flag ⚠️ any due within 7 days; ⚠️ urgent if within 48h. Show cards
   as name + last 4 only.

# Output
```
## 💳 Finance pulse — <window>

### ⚠️ Attention
<zero to three lines; omit section if nothing>

### Transactions
| Date | Card | Merchant | Amount | Status |
|---|---|---|---|---|
...

### Card dues
- <card ••last4 — ₹amount — due date>

### Account notices
- <rate change, statement availability, etc.>
```

Keep the whole thing under 20 lines. If nothing material happened, say
"Quiet week on HSBC and HDFC — no charges above ₹1,000, no notices." and
stop.

# Approvals
- Never draft or send email to HSBC / HDFC.
- Never share, display, or guess full card numbers, OTPs, CVVs, or UPI PINs.
  If one is ever visible, truncate to last 4 digits and warn the user.
