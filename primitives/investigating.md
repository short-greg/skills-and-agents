---
name: investigating
description: >
  Researching the problem or solutions. Gathers information to reduce uncertainty before proceeding.
  You MUST satisfy the Goal, Key Results and follow the Requirements of this primitive.
  Triggers on: "investigate", "research", "look into", "find out", "why is this happening",
  "what's the best practice", "check the docs", "find prior art", "what solutions exist".
argument-hint: "[question or topic to investigate]"
disable-model-invocation: false
user-invocable: true
allowed-tools: Read, Grep, WebSearch, WebFetch
---

# Investigating

**Goal:** Mitigate uncertainty by gathering sufficient information to proceed with confidence.

**Intent:** Prevent acting on incomplete, incorrect, or outdated information. LLM knowledge has limits and becomes stale — investigation actively fills gaps.

**Scope:** Gathering and synthesizing information to reduce uncertainty. Includes researching documentation and best practices, surveying available solutions and prior art, searching web for current information, tracing code paths and data flows, forming and testing hypotheses, identifying root causes of issues, asking users for clarification when needed, and synthesizing findings into actionable conclusions. Investigation answers "what do we need to know?" not "what should we build?"

---

## Key Results - KR

1. Uncertainty reduced with grounded answer — sources cited, confidence stated, root cause identified if debugging
2. Findings are actionable — clear what to do next based on results

## Requirements and Constraints - REQ

1. Assess own uncertainty explicitly — identify what LLM does not know
2. Form hypotheses before investigating
3. Cite sources — where did information come from

---

## Protocols

Use these protocols to satisfy key results. Read each protocol before using it.

- **reasoning.md** - Must use for forming and testing hypotheses
- **checklists.md** - Must use to ensure completeness
- **risk_management.md** - Use when distinguishing root cause from symptoms
- **tracking.md** - Use when tracking what investigated vs questions remaining
- **recovery.md** - Use when resuming after interruption

---

## Preconditions

**Required:** Uncertainty assessment (LLM reflects on what it does not know), question or topic to investigate

**Elicit if not provided:**
- Relevant context (read related code, docs, logs, error messages)
- Prior investigations (check if looked into before)

**Optional:** Initial hypotheses, reproduction steps (if investigating bug)

## Postconditions

**Success:** Uncertainty mitigated with evidence, findings grounded in sources, confidence stated, next steps clear

**Failure:** Insufficient access to information, scope too broad, question cannot be answered

---

## Actions

Select and execute actions to achieve each Key Result. Each action shows which KR it serves.

### Gather Information (→ KR1)

**Self-reflection:** Output an assessment of own uncertainty (identify what LLM does not know or is unsure about). Ask user for clarification when uncertain about context, intent, or constraints only user can provide.

**External research:** Search web (find current documentation, best practices, known issues, solutions). Survey solutions (enumerate available approaches, libraries, patterns). Find prior art (how others solved similar problems). Check documentation (read official docs for APIs, libraries, tools).

**Codebase research:** Read code (trace code paths, understand behavior, find where things happen). Search codebase (find related code, usages, patterns). Read logs (examine error messages, stack traces, system logs).

### Test Hypotheses and Synthesize (→ KR1, KR2)

**Hypothesis-driven:** Form hypotheses (based on evidence, generate possible explanations or candidate approaches). Test hypotheses (design checks that confirm or refute each). Compare states (working vs broken, before vs after, expected vs actual). Reproduce issue. Identify root cause (distinguish from symptoms).

**Synthesis:** Document findings (write what was found with evidence and sources). State confidence (explicit about what is known vs suspected vs still unknown). Enumerate options (if researching solutions, list viable approaches with tradeoffs). Recommend next steps.

---

## Additional Notes and Terms

None

---

## References

- [Anthropic - Claude Code Documentation](https://docs.anthropic.com/en/docs/claude-code)
- [TapRoot - Six Root Cause Analysis Best Practices](https://taproot.com/root-cause-analysis-best-practices/)
- [TestDevLab - Core Principles of Root Cause Analysis](https://www.testdevlab.com/blog/what-is-root-cause-analysis)
- [Bugasura - Root Cause Analysis Guide](https://bugasura.io/blog/root-cause-analysis-for-bug-tracking/)
