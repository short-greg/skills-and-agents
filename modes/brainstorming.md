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

Select actions based on context to achieve the Key Results.

In: The inputs to the action
Out: The outputs to the action

| Action | KR | Goal | When | Instructions | Inputs | Output |
|--------|----|----|------|--------------|--------|--------|
| Generate Plausible Alternatives | KR1 | To produce realistic, high-likelihood options | Use when no options generated yet or conventional solutions needed | Read `protocols/thinking.md` and apply appropriate techniques to generate realistic alternatives given current constraints | In: Problem or topic (required), Known constraints (optional) | Out: Set of viable, realistic alternative approaches |
| Reframe the Problem | KR1 | To generate new perspectives by changing problem understanding | Use when initial problem statement yields no viable options or constraints appear contradictory or alternative perspective requested | Read `protocols/thinking.md` and apply appropriate techniques to output multiple framings of the problem | In: Current problem statement (required) | Out: Alternative problem framings with implications |
| Decompose and Recombine | KR1 | To generate variations by breaking down and manipulating components | Use when existing solution or idea needs exploration of variations | Read `protocols/thinking.md` and apply appropriate techniques to decompose components and explore recombinations | In: Problem, solution, or idea to decompose (required) | Out: Component variations and recombinations |
| Generate Unlikely Alternatives | KR1 | To produce non-obvious, low-probability ideas | Use when conventional ideas not producing positive results or significant uncertainty exists | Read `protocols/thinking.md` and apply appropriate techniques to generate unlikely alternatives. Consider assessing and outputting your confidence that each idea is common | In: Problem or topic (required), Existing ideas to contrast against (optional) | Out: Set of unusual, low-probability alternative approaches |
| Build on Existing Ideas | KR1 | To generate evolved and hybrid ideas from current options | Use when initial ideas exist and need refinement or combination | Read `protocols/thinking.md` and apply appropriate techniques to refine, extend, and combine existing ideas | In: Existing ideas (required) | Out: Refined, extended, and hybrid ideas |
| Describe Each Idea | KR2 | To make all ideas concrete enough for later evaluation | Use when ideas generated and need documentation | Read `protocols/pragmatics.md` and `protocols/transparency.md` and apply appropriate techniques to output clear descriptions stating what each idea is, how it would work, and why it might be viable | In: Generated ideas (required), Audience context (optional) | Out: Clear, evaluable descriptions of each idea |
| Organize Ideas | KR2 | To structure ideas for evaluation and decision-making | Use when multiple ideas exist and need presentation or comparison | Read `protocols/thinking.md`, `protocols/pragmatics.md`, and `protocols/risk_management.md` and apply appropriate techniques to group related ideas, identify tradeoffs and risks, and structure for clear presentation | In: All generated ideas (required), Known constraints or risks (optional) | Out: Categorized idea groups with tradeoffs and risk notes |

---

## References

- [IDEO Brainstorming Rules](https://www.ideou.com/blogs/inspiration/7-simple-rules-of-brainstorming)
- [IDEO Divergent Thinking](https://www.ideou.com/blogs/inspiration/brendan-boyle-on-divergent-thinking-and-the-innovation-funnel)
- [IxDF Ideation Methods](https://www.interaction-design.org/literature/article/learn-how-to-use-the-best-ideation-methods-brainstorming-braindumping-brainwriting-and-brainwalking)
