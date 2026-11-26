# P08: Escalation Handover Summary

**Workflow step:** Escalation. Briefs the Team Lead or Product Safety Lead when a ticket is escalated (P03 = P1/P2, or P05 approval_required = true).
**Problem solved:** Escalations arrive as forwarded email threads. Leads have to read the whole history to work out what decision is needed, which delays safety and approval decisions.
**Techniques:** Role framing (the audience is a time-poor manager), a fixed template, a length limit, a grounding rule ("Unknown", not guessing).
**Test inputs:** T4 (safety) + P03 output; T6 + P05 output.

---

## v1 (naive)
```
Summarise this ticket for my manager.
{MASKED_TICKET}
```
**Anticipated issues:** narrative paragraph; buries the decision needed; length varies; may add assumptions ("the customer is likely exaggerating").

### v1 run record
- Tool / model / date: [FILL]
- Input: T4
- Output (verbatim):
```
[PASTE]
```
- Observed problems: [FILL]
- Change made in v2: Fixed headings with "Decision needed" near the top, an 80-word limit, and "Unknown" for missing facts.

---

## v2 (final)
```
You brief a time-poor Team Lead at Kestrel Home Co. Using ONLY the ticket and the analysis below, write an escalation summary. Do not speculate about the customer's motives. Write "Unknown" for anything not provided.

Format exactly (maximum 80 words in total):
DECISION NEEDED: <what the lead must approve or decide>
BY WHEN: <deadline from policy or customer, or Unknown>
SITUATION: <one sentence>
CUSTOMER IMPACT / RISK: <one sentence>
DONE SO FAR: <one sentence>
REFS: <order IDs, priority, policy clauses>

<ticket>{MASKED_TICKET}</ticket>
<analysis>{P03 and/or P05 JSON output}</analysis>
```

### v2 run record
- Inputs: T4 + P03 output; T6 + P05 output
- Output (verbatim):
```
[PASTE]
```
- Result vs answer key: [FILL: ≤80 words? Decision clear? T4 deadline = 2 business hours per s4?]
- Remaining limitations: The summary is only as good as the upstream outputs. Errors in P03 or P05 carry through, so leads can open the full ticket with one click.
