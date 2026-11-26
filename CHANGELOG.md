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
| P05 | v1→v2 | [FILL] | Grounded in supplied policy + ACL summary; clause citations | Grounding / delimiters | [FILL] |
| P05 | v2→v3 | [FILL] | Analyst role; 6 reasoning steps; UNCLEAR path; approval flags; human_decision_required | Role framing; decomposition; human-in-the-loop | [FILL] |
| P06 | v1→v2 | [FILL] | Role; 120 words; AU English; approved remedy only | Role framing; constraints | [FILL] |
| P06 | v2→v3 | [FILL] | House-tone example; empty-remedy rule; safety line | Few-shot; conditional rules | [FILL] |
| P07 | v1→v2 | [FILL] | 7-item compliance checklist, pass/fail + evidence, any fail = FAIL | Refinement (critic); structured output | [FILL: P06 v1 = FAIL vs P06 v3 = PASS] |
| P08 | v1→v2 | [FILL] | Decision-first template; 80 words; "Unknown" rule | Role framing; constraints | [FILL] |
| P09 | v1→v2 | [FILL] | Group → count → compare → propose; ≥3-ticket threshold | Decomposition | [FILL] |
| P10 | v1→v2 | [FILL] | Numbers only from log with basis shown; "Not available from data"; structure + owners | Grounding; anti-hallucination constraint | [FILL: invented numbers in v1 vs none in v2] |
