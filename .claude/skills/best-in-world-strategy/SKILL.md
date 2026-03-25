---
name: best-in-world-strategy
description: Excellence-first strategic decision support. This skill should be used when users need to choose between options, pressure-test a plan, evaluate risk, or make a specific decision — across security, product, growth, operations, org design, and finance. Use when there is a decision with tradeoffs to score and a recommendation to make. For pure research ("what does the best in the world do about X?"), use best-in-world-research instead.
version: 1.1.0
last_updated: 2026-03-22
---

# Best-In-World Strategy

Set an excellence baseline first. Adapt intentionally second.

## Modes

Use `Speed` mode by default. Reserve `Full` mode for explicitly high-stakes or complex decisions.

- `Speed` (default): frame the decision → world-class baseline → recommendation → downside controls. Rigorous but concise. No options scoring, no red-team unless the stakes clearly warrant it.
- `Full` (when user asks for comprehensive analysis, or the decision is clearly high-stakes / irreversible): complete 9-step workflow with constraint ledger, options scoring, red-team, and learning loop.
- `Evidence-Backed` modifier (optional): add external citations to either mode when user explicitly asks.

Switch to `Risk-Acceptance Override` when user states willingness to accept risk and move quickly — drops options scoring, keeps assumptions + failure modes + downside controls.

## Workflow

1. Frame the decision.
- Write one decision sentence.
- Capture decision type: `architecture`, `security/risk`, `product`, `go-to-market`, `operations`, `org/talent`, `capital/finance`.
- Capture objective, owner, time horizon, and success metric.

2. Build a constraint ledger.
- Separate `hard constraints` from `soft constraints`.
- Record budget, timeline, legal, staffing, and political constraints.

3. Set the best-in-world baseline.
- Identify 2-4 expert archetypes relevant to the decision.
- For each archetype, output:
  - `principle`
  - `expected benefit`
  - `cost/complexity`
  - `confidence`
- Mark each item as `non-negotiable` or `adaptable`.

4. Apply first principles.
- Separate `facts`, `assumptions`, and `conventions`.
- State must-be-true conditions.
- Isolate the smallest causal drivers.

5. Run red-team pressure testing.
- Ask hard skeptical questions.
- Surface first-order and second-order failure modes.
- Define one disconfirming test.

6. Generate options.
- Option A: world-class ideal.
- Option B: pragmatic high-confidence.
- Option C: minimum viable risk-controlled.

7. Score options.
- Score each option 1-5 on:
  - `impact`
  - `risk`
  - `cost`
  - `speed`
  - `reversibility`
  - `learning value`
- Use default weights unless user provides custom weights:
  - `impact 30%`
  - `risk 20%`
  - `cost 15%`
  - `speed 15%`
  - `reversibility 10%`
  - `learning value 10%`

8. Recommend and sequence.
- Recommend one option and explain the tradeoffs.
- Sequence as `now`, `next`, `later`.
- Mark major decisions as `two-way door` or `one-way door`.
- Define downside controls:
  - max acceptable loss
  - kill criteria
  - revisit trigger/date
- Include 2-3 no-regret moves.

9. Define learning loop.
- Define what to measure.
- Define what would change the current strategy.

## Risk-Acceptance Override

When user accepts risk explicitly:

1. Keep rigor floor.
2. Switch to speed output.
3. Include this line exactly:
`Decision owner accepts risk and prefers speed over additional validation at this stage.`
4. Keep mandatory safeguards:
- assumptions list
- confidence tags (`high`, `medium`, `low`)
- top failure modes
- one disconfirming test
- max acceptable loss
- kill criteria
- revisit trigger/date

## Output Contract (Speed — default)

```markdown
## Decision
[One sentence]

## What the Best in the World Would Do
[2–3 bullets. Named practitioners or archetypes. Specific practices, not principles.]

## Recommendation
[One option. Why this one. What's intentionally deferred.]

## Key Assumptions
[2–3 items with confidence tags: high / medium / low]

## Downside Controls
- Max acceptable loss:
- Kill criteria:
- Revisit trigger/date:
```

## Output Contract (Full)

```markdown
## Decision

## Mode
- Selected mode:
- Risk-Acceptance Override:

## Decision Type

## Objective and Success Metric
- Objective:
- Success metric:
- Time horizon:
- Owner:

## Constraint Ledger
- Hard constraints:
- Soft constraints:

## What the Best in the World Would Say
- Expert archetype 1:
  - Principle:
  - Expected benefit:
  - Cost/complexity:
  - Confidence:
- Expert archetype 2:
  - Principle:
  - Expected benefit:
  - Cost/complexity:
  - Confidence:
- Non-negotiables:
- Adaptable elements:

## First-Principles Breakdown
- Facts:
- Assumptions:
- Conventions:
- Must-be-true conditions:
- Causal drivers:

## Red-Team Questions and Failure Modes
- Hard questions:
- First-order failures:
- Second-order failures:
- Disconfirming test:

## Strategic Options
- Option A (world-class ideal):
- Option B (pragmatic high-confidence):
- Option C (minimum viable risk-controlled):

## Option Scores (1-5)
- Weights used:
- Option A:
- Option B:
- Option C:

## Recommendation and Tradeoffs
- Recommended option:
- Why:
- What is intentionally deferred:

## Sequenced Plan
- Now:
- Next:
- Later:
- Two-way door decisions:
- One-way door decisions:

## Downside Controls
- Max acceptable loss:
- Kill criteria:
- Revisit trigger/date:

## No-Regret Moves
- Move 1:
- Move 2:
- Move 3:

## Learning Loop
- What to measure:
- What would change this strategy:
```

## Output Contract (Speed Mode)

```markdown
## Decision

## Why This Option

## Key Assumptions (with confidence tags)

## Top Risks and Failure Modes

## Next 3 Moves (now)

## Downside Controls
- Max acceptable loss:
- Kill criteria:
- Revisit trigger/date:

## Disconfirming Test
```

## Quality Bar

- Avoid generic advice.
- Avoid authority theater; output principles, not name-dropping.
- Make tradeoffs explicit.
- Mark confidence for major claims.
- Prefer falsifiable statements over slogans.

## Reference

Load `references/question-bank.md` when deeper adversarial questioning is needed.
