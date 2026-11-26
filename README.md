# Kestrel Home Co. — Customer Care Prompt Library (BUS4005 Assessment 1)

**Client (hypothetical):** Kestrel Home Co., a mid-sized Australian online homewares retailer with a six-person customer care team.
**Workflow automated:** Customer complaint and support-ticket handling, from intake through to the weekly management report.
**Deliverable:** A 10-prompt library in which each prompt handles one step of the same workflow, plus a documented iteration log showing how each prompt was tested and improved.

> All tickets, names, phone numbers, emails and order data in this repository are **fictional test data**. No real customer data was used. This is a deliberate privacy control, in line with the OAIC's guidance on not entering personal information into publicly available AI tools.

---

## Workflow map (how the 10 prompts connect)

```
Customer ticket
   │
   ▼
P01 PII redaction gate ──► P02 Triage & routing ──► P03 Priority & escalation scoring
                                                        │
                                                        ▼
                         P04 Order-detail extraction & missing-info request
                                                        │
                                                        ▼
                         P05 ACL remedy recommendation  ──► HUMAN APPROVES / AMENDS
                                                        │
                                                        ▼
                         P06 Customer reply draft ──► P07 QA compliance check ──► HUMAN SENDS
                                                        │
                    (if escalated) P08 Team-lead handover summary
                                                        │
                    Weekly: P09 Knowledge-base gap finder · P10 Management insights brief
```

The output of each step is the input to the next. A human approves every remedy decision (P05) and sends every reply (after P07). **No customer receives AI output without human review.**

## Prompt index

| ID | Prompt | Workflow step | Main techniques | Versions |
|----|--------|---------------|-----------------|----------|
| P01 | [PII redaction gate](prompts/P01-pii-redaction.md) | Intake | Constraints, structured output, injection defence | v1 → v3 |
| P02 | [Triage & routing](prompts/P02-triage-routing.md) | Intake | Fixed taxonomy, structured output, few-shot | v1 → v3 |
| P03 | [Priority & escalation scoring](prompts/P03-priority-escalation.md) | Intake | Decomposition (rubric), structured output | v1 → v2 |
| P04 | [Order-detail extraction](prompts/P04-order-extraction.md) | Investigation | Structured output, "null not guess" constraint | v1 → v2 |
| P05 | [ACL remedy recommendation](prompts/P05-acl-remedy.md) | Resolution | Role framing, grounding, decomposition, human-in-the-loop | v1 → v3 |
| P06 | [Customer reply draft](prompts/P06-reply-draft.md) | Resolution | Role framing, constraints, few-shot tone exemplar | v1 → v3 |
| P07 | [QA compliance check](prompts/P07-qa-check.md) | Quality | Refinement (critic pass), checklist, structured output | v1 → v2 |
| P08 | [Escalation handover summary](prompts/P08-escalation-summary.md) | Escalation | Role framing, fixed template, length limit | v1 → v2 |
| P09 | [Knowledge-base gap finder](prompts/P09-kb-gap-finder.md) | Continuous improvement | Decomposition, threshold rule, structured output | v1 → v2 |
| P10 | [Weekly management insights brief](prompts/P10-management-brief.md) | Reporting | Role framing, grounded calculation, anti-hallucination constraint | v1 → v2 |

## Iteration method

1. Write v1 as the "naive" prompt a busy staff member would type.
2. Record the **anticipated failure** *before* running (a hypothesis).
3. Run v1 on the fictional test tickets in `test-data/` and paste the **verbatim output**.
4. Record what **actually** went wrong. If v1 did not fail as expected, record that honestly.
5. Change one thing at a time where possible and explain *why* in the "Change made" field.
6. Re-run on the **same** test tickets so versions can be compared.

A summary of all changes is in [CHANGELOG.md](CHANGELOG.md).

## Test data

- [`test-data/tickets.md`](test-data/tickets.md): six fictional tickets (T1–T6), including a safety case, a prompt-injection attempt and a multi-issue ticket containing personal information.
- [`test-data/store-policy.md`](test-data/store-policy.md): the fictional Kestrel returns and remedies policy and a plain-English Australian Consumer Law (ACL) summary used to ground P05–P07.
- [`test-data/ticket-log.csv`](test-data/ticket-log.csv): a fictional one-week ticket log used by P09 and P10.

## Tool used for testing

- Tool / model: **[FILL: e.g. Microsoft Copilot / ChatGPT, model name]**
- Test dates: **[FILL]**

## AI use acknowledgement

AI tools were used to help draft and test these prompts, in line with the BUS4005 Assessment 1 AI-use conditions. See the AI acknowledgement form submitted with the assessment. **[FILL / confirm]**
