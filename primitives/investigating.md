---
name: investigating
description: >
  Researching the problem or solutions. Gathers information through systematic investigation to reduce uncertainty before proceeding.
  You MUST satisfy the Goal, Key Results and follow the Requirements of this primitive.
  Triggers on: "investigate", "research", "look into", "find out", "why is this happening",
  "what's the best practice", "check the docs", "find prior art", "what solutions exist".
  keywords: searching, surveying, diagnosing, analyzing, synthesizing
argument-hint: "[question or topic to investigate]"
disable-model-invocation: false
user-invocable: true
allowed-tools: Read, Grep, WebSearch, WebFetch
---

# Investigating

**Goal:** Reduce uncertainty by gathering and synthesizing information to enable informed decisions.

**Intent:** Prevent acting on incomplete, incorrect, or outdated information. LLM knowledge has limits and becomes stale — investigation actively fills gaps through systematic research.

**Scope:** Gathering and synthesizing information to answer questions and reduce uncertainty. Covers researching documentation and best practices, surveying solutions and prior art, tracing code paths and data flows, forming and testing hypotheses, diagnosing root causes, and synthesizing findings with actionable recommendations. Investigation answers "what do we need to know?" not "what should we build?"

---

## Key Results - KR

1. Uncertainty reduced with grounded findings — sources cited, confidence stated, root cause identified if debugging
2. Recommendations provided — clear next steps based on investigation results

## Requirements and Constraints - REQ

1. Assess own uncertainty explicitly — identify what LLM does not know
2. Form hypotheses before testing (when debugging or exploring causes)
3. Cite sources — where information came from
4. Always output recommendations for next steps

---

## Protocols

Use these protocols to satisfy key results. Read each protocol before using it.

- **reasoning.md** - Must use for forming and testing hypotheses
- **tracking.md** - Must use to track what investigated vs questions remaining
- **recovery.md** - Use when resuming after interruption

---

## Terms

**Investigation Target:** What is being investigated (problem, root cause, solution landscape, best practices, how something works, etc.)

**Investigation Method:** How to investigate (web search, documentation review, code tracing, hypothesis testing, comparison, reproduction, etc.)

---

## Preconditions

**Required:** Question or topic to investigate

**Elicit if not provided:**
- Context (related code, docs, logs, error messages)
- Investigation scope (what specifically needs to be known)
- Prior investigations (has this been looked into before)

**Optional:** Initial hypotheses, reproduction steps (if investigating bug), constraints

## Postconditions

**Success:** Uncertainty mitigated with evidence, findings grounded in sources, confidence stated, recommendations for next steps provided

**Failure:** Insufficient access to information, scope too broad, question cannot be answered with available resources

---

## Actions

Select actions based on context. Each action shows which KR it serves.

### Assess Uncertainty (→ KR1)

Execute uncertainty assessment using reasoning.md to identify knowledge gaps when beginning investigation. Explicitly identify what LLM does not know or is unsure about. Ask user for clarification when uncertain about context, intent, or constraints only user can provide. Document what needs to be learned.

### Search Information (→ KR1)

Execute information search using WebSearch, WebFetch, Grep, and Read to gather evidence when information is not available in current context. Search web for current documentation, best practices, known issues, solutions. Survey available approaches, libraries, frameworks, patterns. Find prior art and existing implementations. Read official documentation for APIs, libraries, tools. Search codebase for related code, usages, patterns. Read code to trace paths and understand behavior. Examine logs for error messages, stack traces, system logs. Document what was searched and what was found with sources.

### Trace Paths (→ KR1)

Execute path tracing using Read and Grep to understand execution flows when investigating how something works or where something happens. Follow execution paths (function calls, method chains, event handling), data flows (inputs → processing → outputs), system interactions (API calls, database queries, service dependencies), or configuration propagation. Document the path taken and key observations at each step.

### Form Hypotheses (→ KR1)

Execute hypothesis formation using reasoning.md to generate explanations when evidence exists but explanation is unclear. Generate possible explanations based on gathered evidence. Required before hypothesis testing. For each hypothesis: state it clearly, identify what evidence would confirm it, identify what evidence would refute it, estimate likelihood based on current evidence.

### Test Hypotheses (→ KR1)

Execute hypothesis testing using reasoning.md to validate explanations when hypotheses exist. Design checks to confirm or refute each hypothesis. Reproduce issue or conditions to test prediction. Compare differences (working vs broken, before vs after, expected vs actual). Make controlled changes and observe results. Verify hypothesis predictions against reality. Document test results and which hypotheses are confirmed, refuted, or remain uncertain.

### Diagnose Root Cause (→ KR1)

Execute root cause diagnosis using reasoning.md to identify underlying issues when investigating bugs, failures, or unexpected behavior. Distinguish root cause from symptoms. Work backwards from symptom to cause. Verify diagnosis explains all observed symptoms. Check that fixing root cause would prevent recurrence. Document root cause with evidence and reasoning.

### Synthesize Findings and Recommend (→ KR1, KR2)

Execute synthesis and recommendation using reasoning.md and tracking.md to produce actionable conclusions when investigation completes. Summarize what was found with supporting evidence. Cite sources for all claims. State confidence level (known vs suspected vs still unknown). If researching solutions, enumerate viable approaches with tradeoffs. If diagnosing, state root cause and supporting evidence. Provide specific next steps based on findings. Recommend what to do next (implement solution X, investigate further Y, clarify requirement Z). Note what still needs investigation if uncertainty remains. Prioritize recommendations by importance.

---

## Additional Notes and Terms

**Investigation vs Evaluation:** Investigation gathers new information to reduce uncertainty. Evaluation assesses existing work against criteria. If evaluation reveals knowledge gaps, investigation fills them.

---

## References

- [Anthropic - Claude Code Documentation](https://docs.anthropic.com/en/docs/claude-code)
- [TapRoot - Six Root Cause Analysis Best Practices](https://taproot.com/root-cause-analysis-best-practices/)
- [TestDevLab - Core Principles of Root Cause Analysis](https://www.testdevlab.com/blog/what-is-root-cause-analysis)
- [Bugasura - Root Cause Analysis Guide](https://bugasura.io/blog/root-cause-analysis-for-bug-tracking/)
