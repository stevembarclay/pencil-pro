---
name: best-in-world-research
description: World-class practice research. This skill should be used when the user asks what the best in the world does about a specific problem, technique, or situation — covering product, engineering, design, marketing, compliance, operations, org design, and any other domain. Use when the goal is to understand elite-tier practice, not to make a strategic decision. Distinct from best-in-world-strategy, which is for choosing between options.
---

# Best-In-World Research

Surface what elite practitioners actually do. Concise by default. Rigorous always.

## The Question This Skill Answers

> "What does the best in the world do about [X]?"

This is a research question, not a strategy question. The output is an expert brief — not a decision framework. Use `best-in-world-strategy` when the goal is to choose between options with tradeoffs scored. Use this skill when the goal is: *what does excellent look like here?*

## Mode Selection

**Determine mode before responding:**

- **Simple or directional question** → Quick mode automatically. Do not ask.
- **Open-ended or "I don't know where to start" question** → Ask: "Quick answer or comprehensive breakdown?" before responding.
- **User says "quick" / "fast" / "brief"** → Quick mode.
- **User says "deep" / "comprehensive" / "full"** → Deep mode.

## Quick Mode (default for most questions)

Answer in expert voice. No headers. No bullet scaffolding unless the question naturally has a list answer. Conversational but authoritative — like you're quoting the person who spent 25 years solving exactly this problem.

**Quick mode rules:**
- Name who: the specific company, team, person, or practitioner archetype that owns this domain. Not "top SaaS companies." Specific.
- State what they do: observable behavior, not belief. "They do X" not "they believe in Y."
- Give the mechanism: one sentence on why it works.
- Flag confidence if inferred: note when a practice is attributed vs. confirmed by primary source.
- Length: 2–4 short paragraphs or 4–6 bullets. No longer.

**Example quick answer tone:**
> "If you asked the team that built Stripe's API, they'd tell you every error object returns four fields: `code`, `message`, `param`, and `doc_url`. The `doc_url` is the move most people skip — it turns a cryptic error into a solvable problem in one click. The pattern: errors are documentation. If a developer hits an error and can't fix it in two minutes, the error message failed, not the developer."

## Deep Mode (on request or open-ended questions)

For when the user doesn't know the landscape and needs the full picture. Still not a dissertation — disciplined and structured, but concise within structure.

**Deep mode output:**

```
## What the Best in the World Does: [Topic]

### Elite Practitioners
- [Name/Company]: [what makes them credible here — specific signal, not reputation]
- [Name/Company]: ...
- [Name/Company]: ...

### What They Actually Do
[For each practitioner: 2–3 concrete practices. Specific and observable. Note confidence: confirmed / inferred / attributed.]

### The Pattern
[2–3 cross-cutting behaviors that appear across the best. Name each. One sentence on the mechanism.]

### What Mediocre Looks Like
[The common approach most people take, and why it fails — include the failure mechanism, not just the label.]

### Starting Point
[Minimum viable world-class for this problem. What you can do this week. The single thing that separates good from elite.]
```

Target length for deep mode: 400–600 words. If it's longer, cut.

## Quality Bar (both modes)

- Never say "top companies" or "industry leaders" — name them specifically.
- Distinguish what practitioners *do* from what they *say*. Published principles often lag private practice.
- Concrete = "Netflix uses chaos engineering tools that randomly kill production services during business hours." Not concrete = "Netflix prioritizes resilience."
- Anti-patterns require the failure mechanism — why the bad approach still gets chosen, not just that it's bad.
- Flag inferences. Don't present second-hand attribution as confirmed practice.
- No padding. If you don't know specific practitioners for a niche domain, say so and give the best inference you have with a confidence note.
