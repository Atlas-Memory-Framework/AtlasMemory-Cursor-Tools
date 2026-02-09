# Skill Spec: Adversarial Loop Wrapper

## Purpose
Apply the challenge loop with a completion gate and human Q/A enforcement.

## When to use
- After Problem Definition, before Technical Planning.

## Inputs
- Problem Definition output.
- Challenge Loop output (mode=ideation).
- Success criteria for FeatureClarity.

## Outputs
- Updated Challenge Artifacts (weaknesses, risks, assumptions -> tests, alternatives).
- Updated Feature / Design notes as needed.
- Decision boundaries (if new forks are found).

## Success criteria (gate: FeatureClarity)
- At least 2 concrete weaknesses.
- At least 1 end-to-end failure mode.
- Top assumptions have tests.
- Ranked risks have owners + status.
- Alternatives listed (including a disliked alternative).
- Evaluation criteria exists.

## Wrapper behavior
1) Run `challenge-loop mode=ideation`.
2) Validate against FeatureClarity criteria.
3) Use Human Q/A Loop to close gaps.
4) Update plan notes and decision log.

## Plan artifact updates
- Update Challenge Artifacts section.
- Record decision boundaries and DR entries.
