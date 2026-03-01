---
name: brainstorming
description: >
  Generating possible solutions, reasons, or methods from current knowledge. Generates diverse options and ideas before committing to an approach.
  You MUST satisfy the Goal, Key Results and follow the Requirements of this primitive.
  Triggers on: "brainstorm", "what are our options", "alternatives", "how else could we",
  "generate ideas", "explore possibilities", "creative solutions".
  keywords: generating, ideating, diverging, exploring, creating
argument-hint: "[topic or problem to brainstorm]"
disable-model-invocation: false
user-invocable: true
allowed-tools: Read, Grep, WebSearch
---

# Brainstorming

**Goal:** Generate a diverse set of viable options to expand the solution space before narrowing down.

**Intent:** Prevent premature commitment to the first idea. Good brainstorming surfaces alternatives that may be better, simpler, or more appropriate than initial solutions.

**Scope:** Generating and exploring ideas, options, and alternatives from current knowledge. Answers "what are the possibilities?" not "which one is best?" Brainstorming is divergent (expand options), not convergent (select best).

---

## Key Results - KR

1. Diverse ideas generated — multiple distinct options spanning different approaches, including non-obvious ones
2. Ideas are actionable — each described clearly enough to evaluate later

## Requirements and Constraints - REQ

1. Defer judgment — no critiquing during generation
2. Quantity over quality — generate many ideas before filtering
3. Include wild ideas — brilliant ideas often seem ridiculous initially
4. Use current knowledge — brainstorming doesn't research, it generates from what's already known

---

## Protocols

- **thinking.md** — Must use for Decompose to First Principles, Invert the Problem, Search for Analogies, Apply SCAMPER, Push to Extremes, Build and Combine Ideas, Group and Label
- **pragmatics.md** — Use for Describe Each Idea, Group and Label (framing ideas, option presentation for user)
- **tracking_and_recovery.md** — Must use for Describe Each Idea and checklist

---

## Steps

MUST read and follow steps in `base.md`

---

## Terms

**Brainstorming Target:** What you're generating ideas for (problem to solve, approach to take, explanation for phenomenon, etc.)

**Brainstorming Method:** Technique used to generate ideas (first principles, analogies, SCAMPER, inversion, extremes, etc.)

---

## Preconditions

**Required:** Topic or problem — what to brainstorm about

**Elicit if not provided:**
- Context and constraints — problem space, what makes an idea viable
- Goal of brainstorming — what kind of ideas are needed

**Optional:** Starting ideas to build from, evaluation criteria for flagging promising ideas

## Postconditions

**Success:** Multiple distinct options generated, spanning different approaches, each described enough to evaluate later

**Failure:** Only one idea generated, topic too vague, constraints too tight for viable options

---

## Possible Actions

Select actions based on context. Each action shows which KR it serves.

### Decompose to First Principles (→ KR1)

Execute first principles decomposition using `thinking.md` to generate foundational ideas when seeking novel approaches or when assumptions need questioning. Break problem to fundamental truths, identify underlying assumptions, generate ideas by modifying or removing each.

### Invert the Problem (→ KR1)

Execute problem inversion using `thinking.md` to generate contrarian ideas when seeking opposite approaches or when forward thinking feels stuck. Ask what the reverse solution would look like, what would make the problem worse, how to achieve the opposite goal.

### Search for Analogies (→ KR1)

Execute analogy search using `thinking.md` (Analogical reasoning) and WebSearch to generate cross-domain ideas when seeking inspiration from other fields. Identify similar solved problems, map structural similarities, note key differences, adapt patterns to current problem.

### Apply SCAMPER (→ KR1)

Execute SCAMPER method using `thinking.md` (Analytical) to generate systematic variations when building on existing solution or idea. Decompose idea into components, apply each transformation systematically: **S**ubstitute, **C**ombine, **A**dapt, **M**odify, **P**ut to other uses, **E**liminate, **R**everse.

### Push to Extremes (→ KR1)

Execute extreme exploration using `thinking.md` (Creative/Lateral, Novelty Assessment) to generate boundary-case ideas when seeking unusual solutions or when moderate ideas lack impact. Assess likelihood of each idea, deliberately generate low-likelihood alternatives (<0.3), explore simplest, most powerful, fastest, cheapest, and most unusual solutions.

### Build and Combine Ideas (→ KR1)

Execute idea synthesis using `thinking.md` to generate evolved ideas when expanding on promising directions or when seeking hybrids. Take existing ideas, generate variations (refine, extend, specialize). Merge aspects of different ideas into new combinations.

### Describe Each Idea (→ KR2)

Execute idea description using `tracking_and_recovery.md` and `pragmatics.md` (Option Presentation) to ensure actionability when documenting all generated ideas. State clearly for each: what it is, how it would work, why it might be viable. Frame ideas appropriately for audience. Ensure every idea is concrete enough for later evaluation.

### Group and Label (→ KR2)

Execute idea categorization using `thinking.md` (Analytical) and `pragmatics.md` (Framing) to organize output when preparing ideas for evaluation. Cluster related ideas into categories, identify key tradeoffs that differentiate approaches, label each cluster, frame categories for clear presentation to user.

---

## Additional Notes and Terms

**Brainstorming vs Investigating:** Brainstorming generates from current knowledge. Investigating gathers new knowledge through research. If brainstorming reveals knowledge gaps, investigate first.

**Brainstorming vs Evaluating:** Brainstorming is judgment-free divergent thinking (expand options). Evaluating is analytical convergent thinking (select best). Defer evaluation until after brainstorming completes.

---

## References

- [IDEO Brainstorming Rules](https://www.ideou.com/blogs/inspiration/7-simple-rules-of-brainstorming)
- [IDEO Divergent Thinking](https://www.ideou.com/blogs/inspiration/brendan-boyle-on-divergent-thinking-and-the-innovation-funnel)
- [IxDF Ideation Methods](https://www.interaction-design.org/literature/article/learn-how-to-use-the-best-ideation-methods-brainstorming-braindumping-brainwriting-and-brainwalking)
