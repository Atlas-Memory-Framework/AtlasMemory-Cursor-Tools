---
name: challenge-loop
description: Run anti-sycophancy challenge iterations for ideation or technical design. Use during /plan when Feature or Technical clarity is being tested.
---

# /challenge-loop

## Modes
- mode=ideation: stress-test the feature idea before deep technical design.
- mode=technical: stress-test the proposed technical approach.

## Required outputs each iteration
- 2 concrete weaknesses
- 1 failure mode
- 1 disliked alternative (and why it might still win)
- Assumptions -> tests
- Ranked risks with status/owners
- Alternatives considered
- Milestone(s)

## User experience rule (no "go read the plan")
- If the user is expected to review, choose, or correct anything, include the relevant excerpt directly in the chat response.
- Do not require the user to open the plan artifact to see what changed; summarize deltas and paste the updated bullet(s) (e.g., the updated risks/assumptions/tests).

## Stop rule
Stop when:
- No unresolved decision-boundary blockers
- Top assumptions have tests
- Top risks have owner + status
- Evaluation criteria exists
- Iteration cap or no new high-impact deltas in the last iteration

## Gate
- ideation -> FeatureClarity
- technical -> TechnicalClarity
