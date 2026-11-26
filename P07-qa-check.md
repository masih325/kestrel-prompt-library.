# P07: QA Compliance Check (critic pass)

**Workflow step:** Quality. A second AI pass checks every draft reply against a compliance checklist before a human reviews and sends it.
**Problem solved:** Team leads currently QA only a small sample of replies by hand. Compliance errors (wrong ACL statements, unapproved promises) reach customers.
**Techniques:** Refinement (a separate critic prompt reviews another prompt's output), checklist decomposition, structured pass/fail output with evidence.
**Test inputs:** the **v1 output of P06** (expected to fail several checks) and the **v3 output of P06** (expected to pass). Comparing the two is direct evidence of improvement across versions.

---

## v1 (naive)
```
Check this reply is good.
{DRAFT_REPLY}
```
**Anticipated issues:** vague praise ("Looks great!"); no defined standard; misses compliance problems; output can't be used to block a send.

### v1 run record
- Tool / model / date: [FILL]
- Input: P06 v1 output for T1
- Output (verbatim):
```
[PASTE]
```
- Observed problems: [FILL]
- Change made in v2: A 7-item compliance checklist, each item marked pass/fail with evidence, and any failure makes the overall result FAIL.

---

## v2 (final)
```
You are a quality and compliance reviewer for Kestrel Home Co.'s customer care team. Review the draft reply in <draft> against the approved remedy and the original ticket.

Check each item. Mark pass true/false and quote the evidence from the draft.
C1 Remedy offered matches <approved_remedy> exactly (or none offered if empty).
C2 No promises beyond the approved remedy (dates, credits, refunds).
C3 No personal information beyond what the customer provided; no request for card numbers.
C4 No statement that limits Australian Consumer Law rights (e.g. "faulty items can only be returned within 30 days").
C5 If the ticket mentions a safety issue, the draft tells the customer to stop using the item.
C6 120 words or fewer.
C7 Australian English spelling and a professional tone.

If ANY check fails, overall = "FAIL". Suggest the smallest fix for each failed item.

Return ONLY JSON:
{
  "checks": [{"id": "C1", "pass": true, "evidence": ""}],
  "overall": "PASS|FAIL",
  "fixes": []
}

<approved_remedy>{APPROVED_REMEDY}</approved_remedy>
<ticket>{MASKED_TICKET}</ticket>
<draft>{DRAFT_REPLY}</draft>
```

### v2 run record
- Input A: P06 **v1** output (T1 or T4). Input B: P06 **v3** output (same ticket)
- Output A (verbatim):
```
[PASTE]
```
- Output B (verbatim):
```
[PASTE]
```
- Result: [FILL: A = FAIL on which checks? B = PASS?]
- Remaining limitations: A model checking a model can share its blind spots. P07 reduces errors but does not replace the human reviewer. Team leads should also QA a random sample of AI-passed replies each week to measure the miss rate.
