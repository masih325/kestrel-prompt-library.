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
- Tool / model / date: [FILL]
- Output (verbatim):
```
[PASTE]
```
- Observed problems: [FILL: highlight any number that is not computable from the log]
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
- Output (verbatim):
```
[PASTE]
```
- Verification: [FILL: check every number by hand. Expected: 16 tickets; DAMAGED_IN_TRANSIT 5, LATE_OR_MISSING_DELIVERY 5, CHANGE_OF_MIND_RETURN 2, BILLING_PAYMENT 2, PRODUCT_SAFETY 1, FAULTY_PRODUCT 1; unresolved 4 (KH-T1004, T1006, T1014, T1015); escalated 3 (KH-T1006, T1007, T1014); >12 hrs first response = 2 (KH-T1007 14.0, KH-T1014 16.0); median first response = 6.75 hrs]
- Remaining limitations: LLMs can make arithmetic errors even when told to calculate. In production, the metrics should be calculated by the ticketing system or a spreadsheet, with the model writing only the narrative. The numbers must be checked before the brief is circulated.
