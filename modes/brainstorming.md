---
name: brainstorming
description: >
  Generating possible solutions, reasons, or methods from current knowledge. Generates diverse options and ideas before committing to an approach.
  You MUST satisfy the Goal, Key Results and follow the Requirements of this mode.
  Triggers on: "brainstorm", "what are our options", "alternatives", "how else could we",
  "generate ideas", "explore possibilities", "creative solutions".
  keywords: generating, ideating, diverging, exploring, creating
---

# Brainstorming

**Goal:** Generate a diverse set of viable options to expand the solution space before narrowing down.

**Intent:** Prevent premature commitment to the first idea. Good brainstorming surfaces alternatives that may be better, simpler, or more appropriate than initial solutions.

**Scope:** Generating and exploring ideas, options, and alternatives from current knowledge. Answers "what are the possibilities?" not "which one is best?" Brainstorming is divergent (expand options), not convergent (select best).

---

## Table of Contents

- [Key Results](#key-results---kr) — Success criteria for this mode
- [Requirements](#requirements-and-constraints---req) — Rules and constraints to follow
- [Steps](#steps) — Reference to base mode execution
- [Terms](#terms) — Key vocabulary and definitions
- [Preconditions](#preconditions) — What's needed before starting
- [Postconditions](#postconditions) — What's delivered upon completion
- [Actions](#possible-actions) — Concrete steps to achieve results
- [Notes](#additional-notes-and-terms) — Additional context and details
- [References](#references) — External documentation and resources

## Key Results - KR

1. Diverse ideas generated — multiple distinct options spanning different approaches, including non-obvious ones
2. Ideas are actionable — each described clearly enough to evaluate later

## Requirements and Constraints - REQ

1. Defer judgment — no critiquing during generation
2. Quantity over quality — generate many ideas before filtering
3. Include wild ideas — brilliant ideas often seem ridiculous initially
4. Use current knowledge — brainstorming doesn't research, it generates from what's already known

---

## Steps

MUST read and follow steps in `base_mode.md`

---

## Terms

**Brainstorming Target:** What you're generating ideas for (problem to solve, approach to take, explanation for phenomenon, etc.)

**Brainstorming Method:** Technique used to generate ideas (first principles, analogies, SCAMPER, inversion, extremes, etc.)

---

## Preconditions

**Required:** Topic or problem — what to brainstorm about

**Optional:**
- Context and constraints — problem space, what makes an idea viable
- Goal of brainstorming — what kind of ideas are needed
- Starting ideas to build from, evaluation criteria for flagging promising ideas

## Postconditions

**Success:** Multiple distinct options generated, spanning different approaches, each described enough to evaluate later

**Failure:** Only one idea generated, topic too vague, constraints too tight for viable options. Output a request listing the information that was missing and what is needed to succeed.

---

## Possible Actions

**IMPORTANT:** Each action specifies protocols to use. When executing an action you MUST read those protocols if you haven't already, and MUST choose the appropriate techniques from those protocols to achieve the key results of this mode.

Select or propose actions based on context. Each action shows which KR it serves.

### Decompose to First Principles (→ KR1)

**Goal:** Generate foundational ideas from fundamental truths

**When:** Seeking novel approaches or when assumptions need questioning

**Protocol:** `protocols/thinking.md`

**Instructions:** Break problem to fundamental truths, identify underlying assumptions, generate ideas by modifying or removing each. Use first principles reasoning techniques.

**Inputs:**
- Problem or topic (required)
- Current assumptions (optional)

**Default Output:** Ideas based on fundamental truths

---

### Invert the Problem (→ KR1)

**Goal:** Generate contrarian ideas by reversing the problem

**When:** Seeking opposite approaches or when forward thinking feels stuck

**Protocol:** `protocols/thinking.md`

**Instructions:** Ask what the reverse solution would look like, what would make the problem worse, how to achieve the opposite goal. Use problem inversion techniques.

**Inputs:**
- Problem or goal (required)

**Default Output:** Inverted approaches and contrarian ideas

---

### Search for Analogies (→ KR1)

**Goal:** Generate cross-domain ideas from similar problems

**When:** Seeking inspiration from other fields

**Protocols:** `protocols/thinking.md`, WebSearch

**Instructions:** Identify similar solved problems, map structural similarities, note key differences, adapt patterns to current problem. Use analogical reasoning and web research.

**Inputs:**
- Problem or topic (required)
- Potential analog domains (optional)

**Default Output:** Ideas adapted from analogous solutions

---

### Apply SCAMPER (→ KR1)

**Goal:** Generate systematic variations of existing ideas

**When:** Building on existing solution or idea

**Protocol:** `protocols/thinking.md`

**Instructions:** Decompose idea into components, apply each transformation systematically: **S**ubstitute, **C**ombine, **A**dapt, **M**odify, **P**ut to other uses, **E**liminate, **R**everse. Use analytical thinking.

**Inputs:**
- Existing idea or solution (required)

**Default Output:** Variations generated through SCAMPER transformations

### Push to Extremes (→ KR1)

**Goal:** Generate boundary-case and unusual ideas

**When:** Seeking unusual solutions or when moderate ideas lack impact

**Protocol:** `protocols/thinking.md`

**Instructions:** Assess likelihood of each idea, deliberately generate low-likelihood alternatives (<0.3), explore simplest, most powerful, fastest, cheapest, and most unusual solutions. Use creative/lateral thinking and novelty assessment.

**Inputs:**
- Problem or topic (required)
- Current ideas (optional)

**Default Output:** Extreme and unusual solution ideas

---

### Build and Combine Ideas (→ KR1)

**Goal:** Generate evolved and hybrid ideas

**When:** Expanding on promising directions or seeking hybrids

**Protocol:** `protocols/thinking.md`

**Instructions:** Take existing ideas, generate variations (refine, extend, specialize). Merge aspects of different ideas into new combinations. Use synthesis techniques.

**Inputs:**
- Existing ideas (required)

**Default Output:** Refined, extended, and combined ideas

---

### Describe Each Idea (→ KR2)

**Goal:** Ensure all ideas are actionable

**When:** Documenting all generated ideas

**Protocols:** `protocols/tracking_and_recovery.md`, `protocols/pragmatics.md`, `protocols/transparency.md`

**Instructions:** State clearly for each: what it is, how it would work, why it might be viable. Explain the reasoning behind idea generation where helpful. Frame ideas appropriately for audience. Ensure every idea is concrete enough for later evaluation. Use tracking, pragmatic framing, and clear documentation techniques.

**Inputs:**
- Generated ideas (required)
- Audience context (optional)

**Default Output:** Clear descriptions of each idea with reasoning where helpful

---

### Group and Label (→ KR2)

**Goal:** Organize ideas for evaluation

**When:** Preparing ideas for evaluation

**Protocols:** `protocols/thinking.md`, `protocols/pragmatics.md`, `protocols/risk_management.md`

**Instructions:** Cluster related ideas into categories, identify key tradeoffs and risks that differentiate approaches, label each cluster, frame categories for clear presentation to user. Note significant risks in each category. Use analytical thinking, pragmatic framing, and risk assessment.

**Inputs:**
- All generated ideas (required)
- Known constraints or risks (optional)

**Default Output:** Categorized and labeled idea groups with risk notes

---

## Additional Notes and Terms

**Brainstorming vs Investigating:** Brainstorming generates from current knowledge. Investigating gathers new knowledge through research. If brainstorming reveals knowledge gaps, investigate first.

**Brainstorming vs Evaluating:** Brainstorming is judgment-free divergent thinking (expand options). Evaluating is analytical convergent thinking (select best). Defer evaluation until after brainstorming completes.

---

## References

- [IDEO Brainstorming Rules](https://www.ideou.com/blogs/inspiration/7-simple-rules-of-brainstorming)
- [IDEO Divergent Thinking](https://www.ideou.com/blogs/inspiration/brendan-boyle-on-divergent-thinking-and-the-innovation-funnel)
- [IxDF Ideation Methods](https://www.interaction-design.org/literature/article/learn-how-to-use-the-best-ideation-methods-brainstorming-braindumping-brainwriting-and-brainwalking)
