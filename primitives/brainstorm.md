---
name: brainstorm
description: >
  Use when generating options, ideas, or alternatives before committing to an approach.
  Triggers on: "brainstorm", "what are our options", "let's think of ideas", "alternatives",
  "how else could we", "what if we", "generate ideas", "explore possibilities",
  "what are the ways to", "creative solutions", "think outside the box".
argument-hint: "[topic or problem to brainstorm]"
disable-model-invocation: false
user-invocable: true
allowed-tools: Read, Grep, WebSearch
---

# Brainstorm

**Goal:** Generate a diverse set of viable options or ideas to expand the solution space before narrowing down.

**Intent:** Prevent premature commitment to the first idea that comes to mind. Good brainstorming surfaces alternatives that may be better, simpler, or more appropriate — and helps avoid anchoring bias.

**Scope:** Generating and exploring ideas, options, and alternatives. Includes: divergent thinking to expand possibilities, evaluating ideas at a high level for viability, combining or building on ideas, identifying non-obvious approaches, and organizing ideas for later evaluation. Brainstorming is about expanding the solution space — it answers "what are the possibilities?" not "which one is best?"

---

## Key Results

1. Multiple distinct ideas generated — not variations of one idea
2. Ideas span different approaches or paradigms
3. Each idea is described clearly enough to evaluate later
4. Non-obvious options are surfaced alongside obvious ones
5. Ideas are organized or grouped for easier review

---

## Protocols

Protocols are reusable patterns that ensure consistent behavior. They are in `protocols/`. You must comply with these. If you do not understand a protocol, read it.

- `tracking.md` — Track which idea categories have been explored vs remain
- `recovery.md` — Resume brainstorming from where it was interrupted
- `reasoning.md` — Reason about brainstorm approach before starting, verify diversity of ideas after
- `goals_and_objectives.md` — Generate ideas that could achieve the defined goals

---

## Preconditions

**Must be provided:**
- topic or problem: what to brainstorm about — ask if not clear from context

**Self-satisfiable:**
- context and constraints: understand the problem space, existing approaches, and what would make an idea viable
- prior ideas: check if brainstorming has already been done on this topic

**Non-essential:**
- starting ideas: if the user has initial thoughts, can build from there
- evaluation criteria: if known, can flag which ideas are more promising

---

## Postconditions

**Success:**
- Multiple distinct options or ideas are generated
- Ideas span different approaches — not just variations of one idea
- Each idea is described enough to understand what it is and why it might work
- Non-obvious or creative options are included, not just the obvious ones

**Failure:**
- Only one idea generated — not enough divergence
- Topic is too vague to brainstorm meaningfully — needs scoping
- Constraints are so tight that no viable options exist

---

## Possible Actions

Select and sequence based on context and your reasoning. Others may be used.

**Techniques for Diverse Generation**
- **confidence spectrum (commonality)**: generate ideas from common/obvious to rare/unusual — deliberately include low-probability ideas
- **confidence spectrum (effectiveness)**: rate each idea's expected effectiveness — high-confidence proven approaches vs. speculative ones
- **first principles decomposition**: break the problem down to fundamental assumptions, question each one, generate ideas by modifying or removing each
- **invert the problem**: what would the opposite approach look like?
- **analogy search**: how is this problem solved in other domains?
- **push extremes**: what's the simplest possible solution? The most powerful? The most unusual?
- **explore categories**: systematically cover different dimensions (performance vs. simplicity vs. cost vs. maintainability)

**Expansion**
- **build on ideas**: take a promising idea and generate variations
- **combine ideas**: merge aspects of different ideas into new hybrids
- **remove constraints**: what would we do if X wasn't a limitation?
- **search for prior art**: what solutions exist externally?

**Organization**
- **group ideas**: cluster related ideas together
- **label dimensions**: identify the key tradeoffs or axes that differentiate ideas
- **flag viability**: note which ideas seem more or less viable given known constraints

---

## Confirm

Before declaring done, verify against each key result:
- Are there multiple distinct ideas?
- Do they span different approaches?
- Are non-obvious options included?

Report outcome explicitly: state whether the skill succeeded or failed, and why.

---

## Use Cases

- Before designing a solution, to ensure alternatives are considered
- When stuck on a problem and need fresh perspectives
- Generating feature ideas or product directions
- Finding multiple ways to fix a bug or address a problem
- Exploring architectural options before committing

---

## Tools

WebSearch

## Hooks

None
