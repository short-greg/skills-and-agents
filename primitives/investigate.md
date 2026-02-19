---
name: investigate
description: >
  Use when uncertain about something and need to gather information before proceeding.
  Triggers on: "investigate", "research", "look into", "find out", "what are the options",
  "what's the best practice", "check the docs", "find prior art", "why is this happening",
  "I'm not sure about", "what solutions exist", "how do others do this".
argument-hint: "[question or topic to investigate]"
disable-model-invocation: false
user-invocable: true
allowed-tools: Read, Grep, WebSearch, WebFetch
---

# Investigate

**Goal:** Mitigate uncertainty by gathering sufficient information to proceed with confidence.

**Intent:** Prevent acting on incomplete, incorrect, or outdated information. LLM knowledge has limits and becomes stale — investigation actively fills gaps. Poor investigation leads to reinventing existing solutions, choosing suboptimal approaches, solving symptoms instead of causes, or making decisions based on false assumptions.

**Scope:** Gathering and synthesizing information to reduce uncertainty. Includes: researching documentation and best practices, surveying available solutions and prior art, searching the web for current information, tracing code paths and data flows, forming and testing hypotheses, identifying root causes of issues, asking users for clarification when needed, and synthesizing findings into actionable conclusions.

---

## Preconditions

**Must be provided:**
- uncertainty assessment: the LLM must first reflect on what it does not know or is unsure about — this grounds the investigation in actual gaps rather than assumed ones
- question or topic: what needs to be investigated — ask if not clear from context

**Self-satisfiable:**
- relevant context: read related code, docs, logs, or error messages to ground the investigation
- prior investigations: check if this has been looked into before

**Non-essential:**
- initial hypotheses: if the user has guesses, can start from there
- reproduction steps: if investigating a bug, makes root cause identification faster

---

## Postconditions

**Success:**
- Uncertainty is mitigated — the question is answered or unknowns are resolved with evidence
- Findings are grounded in sources, not speculation
- Confidence level is stated — what is known vs. still uncertain
- Findings are documented clearly enough to inform next steps

**Failure:**
- Insufficient access to information (docs missing, code inaccessible, web search blocked)
- Investigation scope is too broad — needs narrowing before proceeding
- Question cannot be answered with available evidence — further action required (e.g., user must provide more context)

---

## Key Results

1. Uncertainty is reduced — the original question has a grounded answer
2. Sources are cited — where did the information come from
3. Confidence level is stated — what is certain vs. probable vs. still unknown
4. If researching solutions: options are enumerated with tradeoffs
5. If debugging: root cause is identified, not just symptoms
6. Findings are actionable — clear what to do next based on results

---

## Required Actions

**Expert Reasoning (REQUIRED FIRST)**
Before doing anything else, describe how an expert would approach investigating this
question or topic. Cover: their strategy, what sources they would consult first and why,
how they would evaluate the quality of information found, what would constitute sufficient
evidence, common pitfalls in this type of investigation, and what a conclusive finding looks like.
This is not an investigation — it is meta-reasoning about approach.

Output this reasoning before proceeding.

**Confirm (REQUIRED LAST)**
Before declaring done, verify against each key result:
- Is uncertainty mitigated with evidence?
- Are sources cited?
- Is the confidence level stated?
- Are findings actionable?

Report outcome explicitly: state whether the skill succeeded or failed, and why.

---

## Possible Actions

Select and sequence based on context and expert reasoning. Others may be used.

**Self-Reflection**
- **assess own uncertainty**: explicitly identify what the LLM does not know or is unsure about — LLM knowledge has limits and gaps; recognizing them is the first step
- **ask user for clarification**: when uncertain about context, intent, or constraints that only the user can provide — do not guess when asking is possible

**External Research**
- **search web**: find current documentation, best practices, known issues, solutions — critical because LLM training data becomes stale; prefer authoritative sources
- **survey solutions**: enumerate available approaches, libraries, or patterns — useful when deciding how to solve a problem, not just whether something exists
- **find prior art**: look for how others have solved similar problems — prevents reinventing and surfaces proven patterns
- **check documentation**: read official docs for APIs, libraries, or tools — primary source for intended behavior and current interfaces

**Codebase Research**
- **read code**: trace code paths, understand behavior, find where things happen — primary source for how the system actually works
- **search codebase**: find related code, usages, patterns — useful when you don't know where to look
- **read logs**: examine error messages, stack traces, system logs — primary source for runtime behavior

**Hypothesis-Driven**
- **form hypotheses**: based on evidence, generate possible explanations or candidate approaches — prevents aimless searching
- **test hypotheses**: design checks that would confirm or refute each hypothesis — prevents confirmation bias
- **compare states**: compare working vs. broken, before vs. after, expected vs. actual, option A vs. option B — often reveals key differences
- **reproduce issue**: attempt to reproduce a reported behavior — confirms the problem exists and provides data
- **identify root cause**: distinguish root cause from symptoms — the thing that, if addressed, would resolve the issue

**Synthesis**
- **document findings**: write up what was found, with evidence and sources
- **state confidence**: be explicit about what is known vs. suspected vs. still unknown
- **enumerate options**: if researching solutions, list viable approaches with tradeoffs
- **recommend next steps**: based on findings, what should happen next

---

## Use Cases

- Researching best practices before designing a solution
- Surveying available libraries or approaches before choosing one
- Finding current documentation when LLM knowledge may be outdated
- Debugging a bug when the cause is unknown
- Understanding how an unfamiliar part of the codebase works
- Determining whether a suspected issue is real
- Clarifying requirements when something is ambiguous

---

## Tools
WebSearch, WebFetch

## Hooks
None
