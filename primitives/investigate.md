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

**Scope:** Gathering and synthesizing information to reduce uncertainty. Includes: researching documentation and best practices, surveying available solutions and prior art, searching the web for current information, tracing code paths and data flows, forming and testing hypotheses, identifying root causes of issues, asking users for clarification when needed, and synthesizing findings into actionable conclusions. Investigation is about reducing uncertainty — it answers "what do we need to know?" not "what should we build?"

---

## Key Results

1. Uncertainty is reduced — the original question has a grounded answer
2. Sources are cited — where did the information come from
3. Confidence level is stated — what is certain vs. probable vs. still unknown
4. If researching solutions: options are enumerated with tradeoffs
5. If debugging: root cause is identified, not just symptoms
6. Findings are actionable — clear what to do next based on results

---

## Protocols

Protocols are reusable patterns that ensure consistent behavior. They are in `protocols/`. You must comply with these. If you do not understand a protocol, read it.

- `tracking.md` — Track what has been investigated vs what questions remain
- `recovery.md` — Resume investigation from where it was interrupted
- `reasoning.md` — Form hypotheses before investigating, verify findings are grounded in evidence after
- `goals_and_objectives.md` — Investigate questions about requirements or success criteria
- `risk_management.md` — Investigation reduces uncertainty before committing to an approach
- `documentation.md` — Flag documentation gaps or inaccuracies discovered during investigation

---

## Preconditions

**Must be provided:**
- uncertainty assessment: the LLM must first reflect on what it does not know or is unsure about
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
- Question cannot be answered with available evidence — further action required

---

## Possible Actions

Select and sequence based on context and your reasoning. Others may be used.

**Self-Reflection**
- **assess own uncertainty**: explicitly identify what the LLM does not know or is unsure about
- **ask user for clarification**: when uncertain about context, intent, or constraints that only the user can provide

**External Research**
- **search web**: find current documentation, best practices, known issues, solutions
- **survey solutions**: enumerate available approaches, libraries, or patterns
- **find prior art**: look for how others have solved similar problems
- **check documentation**: read official docs for APIs, libraries, or tools

**Codebase Research**
- **read code**: trace code paths, understand behavior, find where things happen
- **search codebase**: find related code, usages, patterns
- **read logs**: examine error messages, stack traces, system logs

**Hypothesis-Driven**
- **form hypotheses**: based on evidence, generate possible explanations or candidate approaches
- **test hypotheses**: design checks that would confirm or refute each hypothesis
- **compare states**: compare working vs. broken, before vs. after, expected vs. actual
- **reproduce issue**: attempt to reproduce a reported behavior
- **identify root cause**: distinguish root cause from symptoms

**Synthesis**
- **document findings**: write up what was found, with evidence and sources
- **state confidence**: be explicit about what is known vs. suspected vs. still unknown
- **enumerate options**: if researching solutions, list viable approaches with tradeoffs
- **recommend next steps**: based on findings, what should happen next

---

## Confirm

Before declaring done, verify against each key result:
- Is uncertainty mitigated with evidence?
- Are sources cited?
- Is the confidence level stated?
- Are findings actionable?

Report outcome explicitly: state whether the skill succeeded or failed, and why.

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
