---
name: investigate
description: >
  Use when uncertain about something and need to gather information before proceeding. A primitive is an atomic cognitive action - it does one thing well.
  You MUST satisfy the Goal, Key Results and follow the Requirements of this primitive. They are specified in the instruction body.
  Triggers on: "investigate", "research", "look into", "find out", "what are the options", "what's the best practice", "check the docs", "find prior art", "why is this happening", "I'm not sure about", "what solutions exist", "how do others do this".
argument-hint: "[question or topic to investigate]"
disable-model-invocation: false
user-invocable: true
allowed-tools: Read, Grep, WebSearch, WebFetch
---

# Investigate

**Goal:** Mitigate uncertainty by gathering sufficient information to proceed with confidence.

**Intent:** Prevent acting on incomplete, incorrect, or outdated information. LLM knowledge has limits and becomes stale — investigation actively fills gaps.

**Scope:** Gathering and synthesizing information to reduce uncertainty. Includes researching documentation and best practices, surveying available solutions and prior art, searching web for current information, tracing code paths and data flows, forming and testing hypotheses, identifying root causes of issues, asking users for clarification when needed, and synthesizing findings into actionable conclusions. Investigation answers "what do we need to know?" not "what should we build?"

---

## Key Results - KR

You must satisfy these to complete the primitive successfully.

1. Uncertainty reduced with grounded answer (sources cited, confidence level stated, options enumerated if researching solutions, root cause identified if debugging)
2. Findings are actionable — clear what to do next based on results

## Requirements and Constraints - REQ

Constraints on how to execute the primitive.

1. Track what investigated vs what questions remain per `tracking.md`
2. Form hypotheses before investigating, verify findings grounded in evidence after per `reasoning.md`
3. Assess own uncertainty explicitly — identify what LLM does not know or is unsure about
4. Distinguish root cause from symptoms when debugging per `risk_management.md`
5. Cite sources — where did information come from

---

## Preconditions

Satisfy preconditions before beginning unless Optional.

**Required:** Uncertainty assessment (LLM reflects on what it does not know), question or topic to investigate

**Elicit if not provided:**
- Relevant context (read related code, docs, logs, error messages)
- Prior investigations (check if looked into before)

**Optional:** Initial hypotheses, reproduction steps (if investigating bug)

## Postconditions

The resulting state after the primitive is finished.

**Success:** Uncertainty mitigated with evidence. Findings grounded in sources, not speculation. Confidence level stated. Findings documented clearly enough to inform next steps.

**Failure:** Insufficient access to information (docs missing, code inaccessible, web search blocked), investigation scope too broad (needs narrowing), question cannot be answered with available evidence.

---

## Actions

Select and execute actions to achieve each Key Result. Each action shows which KR it serves.

### Gather Information (→ KR1)

**Self-reflection:** Assess own uncertainty (identify what LLM does not know or is unsure about). Ask user for clarification when uncertain about context, intent, or constraints only user can provide.

**External research:** Search web (find current documentation, best practices, known issues, solutions). Survey solutions (enumerate available approaches, libraries, patterns). Find prior art (how others solved similar problems). Check documentation (read official docs for APIs, libraries, tools).

**Codebase research:** Read code (trace code paths, understand behavior, find where things happen). Search codebase (find related code, usages, patterns). Read logs (examine error messages, stack traces, system logs).

### Test Hypotheses and Synthesize (→ KR1, KR2)

**Hypothesis-driven:** Form hypotheses (based on evidence, generate possible explanations or candidate approaches). Test hypotheses (design checks that confirm or refute each). Compare states (working vs broken, before vs after, expected vs actual). Reproduce issue. Identify root cause (distinguish from symptoms).

**Synthesis:** Document findings (write what was found with evidence and sources). State confidence (explicit about what is known vs suspected vs still unknown). Enumerate options (if researching solutions, list viable approaches with tradeoffs). Recommend next steps.

---

## Additional Notes and Terms

**Use cases:** Researching best practices before designing, surveying available libraries before choosing, finding current documentation when LLM knowledge may be outdated, debugging bug when cause is unknown, understanding unfamiliar codebase, determining whether suspected issue is real, clarifying ambiguous requirements.
