---
name: orienting
description: >
  Understanding the current state of a system, project, or task. Understands current state before deciding what to do.
  You MUST satisfy the Goal, Key Results and follow the Requirements of this mode.
  Triggers on: "orient me", "where are we", "what's the current state", "what's the situation",
  "take stock", "size this up", "gap analysis", "assess the situation".
  keywords: mapping, inventorying, locating, surveying, assessing
argument-hint: "[subject to orient on]"
disable-model-invocation: false
user-invocable: true
allowed-tools: Read, Grep
---

# Orienting

**Goal:** Produce accurate picture of current state so next steps can be decided with confidence.

**Intent:** Prevent acting on assumptions. Poor orientation leads to solving the wrong problem, missing real issues, or duplicating existing work.

**Scope:** Understanding current situation before taking action. Includes inventorying what exists (code, docs, infrastructure), evaluating what works vs broken, identifying gaps between current and target state, surfacing existing issues and technical debt, noting risks in current state. Orientation answers "where are we now?" not "where should we go?"

---

## Key Results - KR

1. Current state described accurately — gaps, issues, risks, and strengths identified
2. Orientation is actionable — reader knows what decisions need to be made

## Requirements and Constraints - REQ

1. No significant omissions — inventory comprehensively
2. Distinguish what works from what doesn't clearly
3. Be aware of cognitive biases that might affect assessment

---

## Steps

MUST read and follow steps in `base_mode.md`

---

## Terms

**Orienting Target:** What is being oriented on (system, project, task, codebase, infrastructure, process, etc.)

**Orienting Method:** How to orient (mapping structure, inventorying available assets, locating relevant information, assessing state against criteria, etc.)

---

## Preconditions

**Required:** Subject to orient on

**Optional:**
- Current state (read relevant files, code, docs, outputs)
- Prior work (look for existing assessments, specs, decisions)
- Target state (if known, enables gap identification), success criteria

## Postconditions

**Success:** Current state documented accurately, gaps and issues surfaced, orientation is actionable

**Failure:** Cannot access subject, subject too large or ill-defined. Output a request listing the information that was missing and what is needed to succeed.

---

## Possible Actions

**IMPORTANT:** Each action specifies protocols to use. When executing an action you MUST read those protocols if you haven't already, and MUST choose the appropriate techniques from those protocols to achieve the key results of this mode.

Select or propose actions based on context. Each action shows which KR it serves.

### Map Structure (→ KR1)

**Goal:** Understand layout or topology of orienting target

**When:** Understanding layout or topology of orienting target

**Protocols:** `protocols/discipline.md`, `protocols/system_modularity.md`

**Instructions:** Systematically enumerate ALL components: directory structure, component relationships, module dependencies, data flows, or process sequences. Document hierarchy, connections, boundaries. Assess architectural structure and modularity. Create representation showing how parts relate to whole. Use MECE enumeration and modularity assessment.

**Inputs:**
- Subject to orient on (required)
- Scope boundaries (optional)

**Default Output:** Structure diagram or description with modularity assessment

---

### Inventory Assets (→ KR1)

**Goal:** Catalog existing resources

**When:** Cataloging existing resources

**Protocol:** `protocols/discipline.md`

**Instructions:** Systematically list ALL files, components, modules, dependencies, configurations, documentation, tests, or infrastructure. Categorize by type, purpose, or area. Track coverage to ensure no significant omissions. Note what exists, what's complete, what's partial, what's missing.

**Inputs:**
- Subject to orient on (required)
- Categories to track (optional)

**Default Output:** Asset inventory with categories and status

---

### Locate Information (→ KR1)

**Goal:** Find specific information needed for orientation

**When:** Specific information is needed for orientation

**Protocol:** `protocols/elicitation.md`

**Instructions:** Search for documentation, implementation details, configuration, test coverage, known issues, or prior decisions. Apply interviewing when user has context not available in artifacts. Document where critical information resides. Use open questions and inference first techniques.

**Inputs:**
- Information needed (required)
- Known locations (optional)

**Default Output:** Information found with locations

---

### Identify Gaps and Issues (→ KR1)

**Goal:** Find what's missing or broken

**When:** Target state is known or standards exist

**Protocols:** `protocols/thinking.md`, `protocols/discipline.md`

**Instructions:** Systematically enumerate ALL gaps: missing components, incomplete implementations, broken functionality, inconsistencies, technical debt, deviations from requirements. Distinguish what's present from what's needed. Note severity and impact of each. Use analytical thinking and MECE enumeration.

**Inputs:**
- Current state (required)
- Target state or standards (required)

**Default Output:** Gap analysis with severity and impact

---

### Assess Risks and Strengths (→ KR1)

**Goal:** Understand stability and reliability

**When:** Understanding stability and reliability

**Protocols:** `protocols/thinking.md`, `protocols/risk_management.md`

**Instructions:** Identify risks (fragile code, lack of tests, tight coupling, security vulnerabilities, performance bottlenecks). Identify strengths (well-tested code, clear documentation, good separation). Categorize by likelihood and impact. Use analytical thinking and risk assessment techniques.

**Inputs:**
- Current state (required)
- Quality criteria (optional)

**Default Output:** Risk and strength assessment with categorization

---

### Summarize State and Flag Decisions (→ KR1, KR2)

**Goal:** Produce actionable summary of current state

**When:** Orientation is complete

**Protocol:** `protocols/pragmatics.md`

**Instructions:** Produce concise summary of current state with supporting evidence. Group findings by category or area. Flag decision points — identify what decisions need to be made based on findings (what to fix first, what approach to take, what to investigate further). Frame findings appropriately for audience. Use directness calibration.

**Inputs:**
- All findings (required)
- Audience context (optional)

**Default Output:** Summary with flagged decision points

---

## Additional Notes and Terms

None

---

## References

- [Anthropic - Claude Code Documentation](https://docs.anthropic.com/en/docs/claude-code)
- [Farnam Street - The OODA Loop](https://fs.blog/ooda-loop/)
- [Corporate Finance Institute - OODA Loop Guide](https://corporatefinanceinstitute.com/resources/management/ooda-loop/)
