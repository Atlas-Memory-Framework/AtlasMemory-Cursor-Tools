# Skill Spec: Human Q/A Loop (Wrapper)

## Purpose
Force completion of a target skill by iterating with the user until explicit success criteria are met.

## When to use
- Wrap Define Problem, Adversarial Loop, Technical Planning, and Reviews.
- Any time the user tries to "approve" without demonstrating understanding.

## Inputs
- Target skill output (draft).
- Target success criteria.
- Known blockers / missing inputs.

## Outputs
- Updated target output.
- Explicit checklist of success criteria status.
- NextRequiredUserAction if still blocked.

## Success criteria
- All target criteria are satisfied or explicitly deferred with DR entry.
- Open questions are either answered or documented with impact radius + trigger.

## Loop behavior
1) Validate output vs criteria.
2) Highlight missing items with concrete questions.
3) Require specific answers (no "looks good").
4) Patch output based on responses.
5) Repeat until criteria are satisfied or user defers with a decision log.

## Stop rules
- Stop only when criteria satisfied or a DR-backed deferral exists.
- Do not advance to next gate if criteria incomplete.

## Plan artifact updates
- Append Decision Log entries for deferrals.
- Record misunderstandings or clarifications in Notes.
