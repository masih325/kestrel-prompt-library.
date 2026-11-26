# Iteration changelog

Each row summarises one version change. The full prompts, verbatim outputs and observations are in each prompt file.

| Prompt | Version | What failed in the previous version (observed) | What changed | Technique added | Evidence of improvement |
|---|---|---|---|---|---|
| P01 | v1→v2 | [FILL from run] | Exact mask tokens + keep-list for order IDs; no rephrasing | Constraints | [FILL] |
| P01 | v2→v3 | [FILL] | JSON audit trail; ticket treated as data; injection flag | Structured output; injection defence | [FILL] |
| P02 | v1→v2 | [FILL] | Fixed taxonomy + JSON + queue mapping | Constraints; structured output | [FILL] |
| P02 | v2→v3 | [FILL] | Multi-issue list, evidence quotes, confidence, 2 worked examples | Few-shot; decomposition | [FILL] |
| P03 | v1→v2 | [FILL] | Six risk flags checked one by one, then fixed rubric; sentiment can't raise priority | Decomposition; structured output | [FILL] |
| P04 | v1→v2 | [FILL] | Schema; null not guess; ≤2 clarifying questions | Constraints; structured output | [FILL] |
| P05 | v1→v2 | v1 gave its own verdict, guessed the return window, and suggested an unapproved goodwill credit (ACL point was correct) | Grounded in supplied policy + ACL summary; clause citations | Grounding / delimiters | See v3 row |
| P05 | v2→v3 | (v2 not run separately; time) | Analyst role; 6 reasoning steps; UNCLEAR path; approval flags; human_decision_required | Role framing; decomposition; human-in-the-loop | v3 on T3: s1 cited, no verdict, no credit, missing facts flagged. v3 on T5: injection detected, $200 credit refused. New weakness: remedy_options empty (v4 fix proposed) |
| P06 | v1→v2 | [FILL] | Role; 120 words; AU English; approved remedy only | Role framing; constraints | [FILL] |
| P06 | v2→v3 | (v1/v2 outputs not captured; time) | House-tone example; empty-remedy rule; safety line | Few-shot; conditional rules | v3 on T4: safety line included, no remedy promised, ~70 words. Defect: two apologies (v4 fix proposed) |
| P07 | v1→v2 | (v1 not run; time) | 7-item compliance checklist, pass/fail + evidence, any fail = FAIL | Refinement (critic); structured output | v2 blocked the P06 v3 draft on C2: a false positive caused by a cross-prompt rule conflict; it missed the double apology. v3: shared house rules + apology check |
| P08 | v1→v2 | [FILL] | Decision-first template; 80 words; "Unknown" rule | Role framing; constraints | [FILL] |
| P09 | v1→v2 | [FILL] | Group → count → compare → propose; ≥3-ticket threshold | Decomposition | [FILL] |
| P10 | v1→v2 | v1 figures were correct (it ran code) but the output was long, showed no basis and used a mean | Numbers only from log with basis shown; "Not available from data"; structure + owners | Grounding; anti-hallucination constraint | v2 given an empty log returned "Not available from data" for every metric, inventing none |

**Testing status (honest):** P05, P06, P07 and P10 were tested in ChatGPT on 26 Nov 2026 (see each file). P01–P04, P08 and P09 are designed and versioned but **not yet tested**. Their v1→v2 changes are based on anticipated failures and are marked as such. Testing those four is the first task of the recommended pilot.
