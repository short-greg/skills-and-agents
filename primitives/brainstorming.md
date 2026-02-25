---
name: brainstorming
description: >
  Generates diverse options and ideas before committing to an approach.
  You MUST satisfy the Goal, Key Results and follow the Requirements of this primitive.
  Triggers on: "brainstorm", "what are our options", "alternatives", "how else could we",
  "generate ideas", "explore possibilities", "creative solutions".
argument-hint: "[topic or problem to brainstorm]"
disable-model-invocation: false
user-invocable: true
allowed-tools: Read, Grep, WebSearch
---

# Brainstorming

**Goal:** Generate a diverse set of viable options to expand the solution space before narrowing down.

**Intent:** Prevent premature commitment to the first idea. Good brainstorming surfaces alternatives that may be better, simpler, or more appropriate.

**Scope:** Generating and exploring ideas, options, and alternatives. Answers "what are the possibilities?" not "which one is best?"

---

## Key Results - KR

1. Diverse ideas generated — multiple distinct options spanning different approaches, including non-obvious ones
2. Ideas are actionable — each described clearly enough to evaluate later

## Requirements and Constraints - REQ

1. Defer judgment — no critiquing during generation
2. Quantity over quality — generate many ideas before filtering
3. Include wild ideas — brilliant ideas often seem ridiculous initially

---

## Protocols

Use these protocols to satisfy key results. Read each protocol before using it.

- **reasoning.md** - Must use to evaluate which categories to explore and assess diversity
- **checklists.md** - Must use to ensure coverage across idea categories
- **tracking.md** - Use when tracking idea categories explored vs remaining
- **recovery.md** - Use when resuming after interruption

---

## Preconditions

**Required:** Topic or problem — what to brainstorm about

**Elicit if not provided:**
- Context and constraints — problem space, what makes an idea viable

**Optional:** Starting ideas to build from, evaluation criteria for flagging promising ideas

## Postconditions

**Success:** Multiple distinct options generated, spanning different approaches, each described enough to evaluate

**Failure:** Only one idea generated, topic too vague, constraints too tight for viable options

---

## Actions

Select based on context. Each action shows which KR it serves.

### First Principles Decomposition (→ KR1)
Assumptions must be questioned. Break problem to fundamentals, generate ideas by modifying or removing each assumption.

### Invert the Problem (→ KR1)
Opposite approaches must be considered. Ask what the reverse solution would look like.

### Analogy Search (→ KR1)
Cross-domain solutions must be explored. Ask how this problem is solved in other fields.

### SCAMPER (→ KR1)
Systematic variations must be generated. Apply: Substitute, Combine, Adapt, Modify, Put to other uses, Eliminate, Reverse.

### Push Extremes (→ KR1)
Boundary cases must be explored. Generate simplest, most powerful, and most unusual solutions.

### Build and Combine (→ KR1, KR2)
Ideas must be expanded. Take promising ideas and generate variations or merge aspects into hybrids.

### Describe Each Idea (→ KR2)
Ideas must be actionable. For each idea, state what it is and why it might work.

### Group and Label (→ KR2)
Ideas must be organized. Cluster related ideas, identify key tradeoffs that differentiate them.

---

## Additional Notes and Terms

None

---

## References

- [IDEO Brainstorming Rules](https://www.ideou.com/blogs/inspiration/7-simple-rules-of-brainstorming)
- [IDEO Divergent Thinking](https://www.ideou.com/blogs/inspiration/brendan-boyle-on-divergent-thinking-and-the-innovation-funnel)
- [IxDF Ideation Methods](https://www.interaction-design.org/literature/article/learn-how-to-use-the-best-ideation-methods-brainstorming-braindumping-brainwriting-and-brainwalking)
