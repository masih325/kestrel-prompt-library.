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
- **Invalid run (recorded honestly):** in an earlier attempt the `<draft>` still held the placeholder text "PASTE DRAFT HERE". The checker returned FAIL (C5 and C7 failed because the "draft" had no safety line or tone). That shows it won't pass an empty draft, but it isn't a valid test, so it was re-run below.

- Tool / model / date: ChatGPT, fresh chat, 26 Nov 2026
- Input: the P06 **v3** draft for ticket T4 (approved_remedy empty)
- Output (verbatim):
```
{
"checks": [
{"id": "C1", "pass": true, "evidence": "No specific remedy is offered. The approved_remedy field is empty."},
{"id": "C2", "pass": false, "evidence": "We're reviewing your case and will get back to you within 1 business day with an update."},
{"id": "C3", "pass": true, "evidence": "The draft uses placeholders [NAME] and [AGENT_NAME] and does not request additional personal or payment information."},
{"id": "C4", "pass": true, "evidence": "The draft contains no statement limiting Australian Consumer Law rights."},
{"id": "C5", "pass": true, "evidence": "Please stop using the item and unplug it if it is safe to do so."},
{"id": "C6", "pass": true, "evidence": "The draft contains approximately 75 words, excluding placeholders."},
{"id": "C7", "pass": true, "evidence": "Uses Australian English spelling, including 'business day', and maintains a professional, empathetic tone."}
],
"overall": "FAIL",
"fixes": ["C2: Remove the specific 1 business day response commitment unless it has been authorised. Replace with: 'We're reviewing your case and will provide an update once we've completed our review.'"]
}
```
- **Result:** the checker works as a gate. It quoted evidence for each check and blocked the draft on C2.
- **Finding 1: a cross-prompt conflict (false positive).** The "within 1 business day" line is *required* by P06's empty-remedy rule, but P07 isn't told about it, so it treats the line as an unapproved promise. Testing the prompts **as a chain** surfaced an inconsistency that testing each prompt alone would have missed.
- **Finding 2: a missed defect.** The draft apologises twice, breaking P06's "one apology at most" rule. P07 has no check for this, so it passed C7.
- **v3 changes (proposed and documented):**
  - C2: "The holding line 'we will reply within 1 business day' is approved standard wording and is not a breach."
  - Add C8: "No more than one apology."
  - Pass P06's rules to P07 as a shared `<house_rules>` block, so writer and checker use one source of truth.
- Remaining limitations: A model checking a model can share its blind spots. P07 reduces errors but does not replace the human reviewer. Team leads should also QA a random sample of AI-passed replies each week to measure the miss rate.
