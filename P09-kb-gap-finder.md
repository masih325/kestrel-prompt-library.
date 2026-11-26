# P09: Knowledge-Base Gap Finder

**Workflow step:** Continuous improvement (weekly). Finds repeat questions that a help-centre article could prevent.
**Problem solved:** The same "where's my parcel" questions recur every week. Nobody has time to analyse them, so avoidable contacts keep consuming agent hours.
**Techniques:** Decomposition (group → count → compare → propose), a threshold rule, structured output, grounding in supplied data.
**Test input:** `test-data/ticket-log.csv` and the existing article list below.

Existing help-centre articles (fictional): "Returns and refunds policy", "How to update your account details", "Delivery time estimates".

---

## v1 (naive)
```
What FAQ articles should we write based on these tickets?
{TICKET_LOG}
```
**Anticipated issues:** a generic FAQ list ("Shipping FAQ", "Returns FAQ") not tied to evidence; suggests articles that already exist; no counts, so impact can't be prioritised.

### v1 run record
- Tool / model / date: [FILL]
- Output (verbatim):
```
[PASTE]
```
- Observed problems: [FILL]
- Change made in v2: Step-by-step grouping by root cause, counts with ticket IDs, a check against existing articles, and a minimum of 3 tickets before an article is proposed.

---

## v2 (final)
```
You are a knowledge-base analyst for Kestrel Home Co. customer care. Use ONLY the ticket log in <log> and the article list in <existing_articles>.

Steps:
1. Read each resolution_note and group tickets by the underlying customer question (root cause), not by category.
2. Count tickets per group and list their ticket_ids.
3. Check whether an existing article already answers that question.
4. Propose a NEW article only if the group has 3 or more tickets AND no existing article covers it.

Return a markdown table with columns:
| Root-cause question | Ticket count | Ticket IDs | Covered by existing article? | Proposed article title | 3-bullet outline |

Then one sentence on which gap to fix first and why (highest count).

<existing_articles>Returns and refunds policy; How to update your account details; Delivery time estimates</existing_articles>
<log>{PASTE ticket-log.csv}</log>
```

### v2 run record
- Output (verbatim):
```
[PASTE]
```
- Result vs answer key: [FILL: expected top gap = "how do I track my parcel" (KH-T1004, T1008, T1011 = 3 tickets); "glass/ceramic items breaking in transit" (5 tickets: KH-T1002, T1005, T1010, T1013, T1016) is a packaging issue, not a KB gap. Did the model make that distinction?]
- Remaining limitations: Small weekly samples can produce false patterns. Proposed articles are reviewed by the Team Lead before publishing. Glass breakage is an operations (packaging) problem; P09 should surface it, but an article won't fix it.
