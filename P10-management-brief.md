# P10: Weekly Management Insights Brief

**Workflow step:** Reporting (weekly). Turns the ticket log into a one-page brief for the Head of Customer Experience.
**Problem solved:** The weekly report takes a team lead about half a day to compile by hand, and managers get it late. **[ASSUMPTION: confirm with client]**
**Techniques:** Role framing (operations analyst writing for an executive), grounding with calculations from data only, an explicit anti-hallucination constraint, a fixed section structure.
**Test input:** `test-data/ticket-log.csv`

---

## v1 (naive)
```
Write a weekly report on our customer service performance.
{TICKET_LOG}
```
**Anticipated issues:** **invents metrics not in the data** (e.g. a CSAT score or "resolution rate up 12% on last week" with no prior-week data), which is confabulation presented as fact; long and generic; no recommended actions or owners.

### v1 run record
- Tool / model / date: ChatGPT ("Analyzed" mode: it ran a calculation), 26 Nov 2026
- Output (verbatim excerpt from the screenshot; the full output was longer):
```
Weekly Customer Service Performance Report
Reporting Period: 16–22 November 2026
Company: Kestrel Home Co.
1. Executive Summary
During the reporting week, the customer service team handled 16 customer tickets across delivery, damaged products, returns, billing, product safety and faulty products.
Overall, 12 tickets were resolved, while 4 remained unresolved. Three tickets were escalated for further attention.
2. Key Performance Metrics
Total tickets received 16 | Tickets resolved 12 (75%) | Unresolved tickets 4 (25%) | Escalated tickets 3 (18.75%) | Average first response time 7.5 hours | Highest first response time 16 hours | Lowest first response time 1 hour
3. Ticket Category Analysis
Late or Missing Delivery 5 | Damaged in Transit 5 | Change of Mind Return 2 | Billing and Payment 2 | Product Safety 1 | Faulty Product 1 | Total 16
Delivery-related and damaged-product complaints accounted for 10 of the 16 tickets (62.5%), making them the main areas of customer concern.
4. Priority Analysis
P1: 1 ticket; P2: 4 tickets ...
```
- Observed (checked by hand against the log): **every visible figure is correct** (16 total, 12 resolved, 4 unresolved, 3 escalated, mean first response 120/16 = 7.5 hrs, category counts).
- **The anticipated failure (invented metrics) did NOT occur.** The tool ran code to calculate the figures, which reduces arithmetic and confabulation risk.
- Real weaknesses: long, report-style output rather than a two-minute executive brief; no calculation basis shown beside figures; uses a mean, where a median is more robust to one 16-hour outlier.
- Change made in v2: format for a time-poor executive (≤250 words, fixed sections), the basis shown for every number, "Not available from data" for anything not computable, and actions with owners.
- Change made in v2: Calculations only from the log, "Not available from data" for anything else, a fixed structure, a word limit, and actions with owners.

---

## v2 (final)
```
You are an operations analyst writing for Kestrel Home Co.'s Head of Customer Experience, who has two minutes to read this. Use ONLY the data in <log>.

Rules:
- Every number must be calculable from <log>. Show the basis in brackets, e.g. "6 of 16 tickets (38%)".
- If a metric cannot be calculated from <log> (e.g. CSAT, week-on-week change), write "Not available from data". Never estimate.
- Do not speculate about causes that the resolution notes don't show.
- Maximum 250 words.

Structure:
1. Headline (2 sentences)
2. Volume by category (table: category, count, % of total)
3. Service levels: median first response (hrs); number of tickets over 12 hrs first response; unresolved count
4. Escalations and risks (list ticket IDs)
5. Top 3 recurring issues (from resolution notes)
6. Two recommended actions, each with a suggested owner role

<log>{PASTE ticket-log.csv}</log>
```

### v2 run record

**Run 1 (accidental stress test): the `<log>` section was sent EMPTY**
```
{
"Headline": "Customer service performance metrics cannot be calculated because the <log> section is empty. No ticket volume, response time, resolution, escalation or recurring issue data is available.",
"Volume by category": {"status": "Not available from data", "table": []},
"Service levels": {"Median first response (hrs)": "Not available from data", "Tickets over 12 hrs first response": "Not available from data", "Unresolved count": "Not available from data"},
"Escalations and risks": "Not available from data. No ticket IDs provided.",
"Top 3 recurring issues": "Not available from data.",
"Recommended actions": [
{"action": "Provide the customer service ticket log to enable performance analysis.", "owner": "Customer Service Operations Manager"},
{"action": "Provide escalation and unresolved ticket records for risk assessment.", "owner": "Head of Customer Experience"}
]
}
```
- Finding: this run was in the **same chat** as the v1 run, so the data was visible earlier in the conversation. The v2 prompt still **refused to use anything outside `<log>`** and invented **no** figures. That's strong evidence the grounding and anti-hallucination constraints work.

**Run 2: with the full log.** Not run because of time; this is the next test in the pilot.
- Verification: [FILL: check every number by hand. Expected: 16 tickets; DAMAGED_IN_TRANSIT 5, LATE_OR_MISSING_DELIVERY 5, CHANGE_OF_MIND_RETURN 2, BILLING_PAYMENT 2, PRODUCT_SAFETY 1, FAULTY_PRODUCT 1; unresolved 4 (KH-T1004, T1006, T1014, T1015); escalated 3 (KH-T1006, T1007, T1014); >12 hrs first response = 2 (KH-T1007 14.0, KH-T1014 16.0); median first response = 6.75 hrs]
- Remaining limitations: LLMs can make arithmetic errors even when told to calculate. In production, the metrics should be calculated by the ticketing system or a spreadsheet, with the model writing only the narrative. The numbers must be checked before the brief is circulated.
