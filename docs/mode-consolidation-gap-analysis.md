# Mode-to-Protocol Consolidation Gap Analysis

This document identifies content in modes that is not yet captured in protocols, enabling prioritized consolidation.

---

## Background

The framework has consolidated from three layers (Protocols, Modes, Workflows) to two layers (Protocols, Workflows). This analysis identifies what valuable content from modes would be lost if modes were deleted without consolidation.

**Current Architecture:**
- **Protocols** — Techniques + Possible Actions (how to think/do)
- **Workflows** — Orchestration of Actions that reference Protocols

**Modes are deprecated.** The `modes/` directory is retained for reference but should not be used for new development. See [mode-creation-guideline.md](../guidelines/mode-creation-guideline.md) for deprecation notice.

**Analysis Method:**
1. Read all protocols (with their Possible Actions and Risks sections)
2. Read all modes (with their Actions tables)
3. Identify items in modes that aren't adequately covered by existing protocol content

**Note:** Many modes already reference protocols appropriately. This table focuses on **gaps** — content unique to modes that adds value beyond what protocols currently provide.

**Style Layer Note:** Some items below (marked with *) may belong in a future "Styles" layer that defines output patterns/structures rather than thinking techniques. This distinction is being developed.

---

## Gap Analysis Table

| Mode | Item | Target Protocol | Importance | Risk of Losing |
|------|------|-----------------|------------|----------------|
| **base_mode** | Expert-reasoned action plan step (output ideal expert type and how they'd tackle) | identity_and_profile | High | High — Explicit expert framing improves output quality |
| **base_mode** | Evaluate actions step (output table: Action / Why Use / Why Not) | thinking | Medium | Medium — Structured action comparison is useful but can be derived |
| **base_mode** | Inline tracking requirement (output checklist in conversation, not files) | tracking_and_recovery | High | High — Distinguishes mode tracking from workflow tracking |
| **brainstorming** | Generate Unlikely Alternatives action (explicitly target <0.3 probability ideas)* | styles (future) | Medium | Low — Novelty Assessment technique exists; threshold is style |
| **brainstorming** | Decompose and Recombine action (SCAMPER-style component manipulation)* | styles (future) | Medium | Low — SCAMPER exists in thinking protocol; format is style |
| **brainstorming** | Organize Ideas action (group, identify tradeoffs, structure for presentation)* | styles (future) | Medium | Medium — Useful for decision-making output format |
| **maintaining** | Fix Code Issues action (warnings, dead code, security vulnerabilities) | software_quality | Medium | Low — Covered by quality dimensions |
| **maintaining** | Update Dependencies action (prioritizing security patches) | risk_management | Medium | Medium — Specific maintenance activity not covered |
| **maintaining** | Record Issues and Decisions action (documenting as technical debt) | transparency | Medium | Low — Decision Recording covers this |
| **orienting** | Map Structure action (enumerate components, document hierarchy) | system_modularity | Medium | Low — Decomposition techniques exist |
| **orienting** | Inventory Assets action (catalog by type/purpose, note completeness) | discipline | Medium | Medium — Enumeration exists but asset inventory is specific |
| **orienting** | Summarize State and Flag Decisions action | pragmatics | High | High — Connects orientation to next steps |
| **interviewing** | All actions | elicitation | Low | Low — Elicitation protocol is comprehensive |
| **defining** | State the Problem action (articulate problem with context) | goal_setting | Medium | Low — Problem/Opportunity Framing exists |
| **defining** | Define Success action (testable success criteria) | criteria_setting | Low | Low — Protocol is comprehensive |
| **defining** | Define Boundaries action (scope in/out, constraints) | goal_setting | Low | Low — Boundary Setting exists |
| **defining** | Validate Definition action (check completeness, consistency, feasibility) | thinking | Medium | Medium — Validation pattern useful |
| **implementing** | Gather Context action (interpret screenshots, error messages, bug reports) | elicitation | High | High — Visual interpretation not explicitly covered |
| **implementing** | Analyze Existing Code action (trace paths, identify dependencies) | thinking | Medium | Low — Systems thinking covers this |
| **implementing** | Simplify action (reduce complexity, combat bloat post-implementation) | system_modularity | High | High — Anti-bloat action is critical for AI coding |
| **implementing** | Self-Review action (quality + modularity assessment) | software_quality | Medium | Low — Quality protocol covers this |
| **designing** | Propose Alternatives action (generate design options) | thinking | Low | Low — Generate Alternatives exists |
| **designing** | Evaluate Alternatives and Tradeoffs action (compare with rationale) | thinking | Low | Low — Evaluate Tradeoffs exists |
| **designing** | Design Structure action (components, interfaces, data) | system_modularity | Low | Low — Decomposition techniques exist |
| **designing** | Design Process action (behavior, flows, state transitions) | system_modularity | Medium | Medium — Process design less covered than structure |
| **planning** | Decompose Work action (100% rule — enumerate ALL actions) | discipline | High | Medium — Discipline has enumeration but not "100% rule" emphasis |
| **planning** | Perform Risk Analysis action (scope, complexity, risks, contingencies) | risk_management | Low | Low — Protocol is comprehensive |
| **planning** | Map Dependencies action (check ALL action pairs) | discipline | Medium | Medium — Systematic pair-checking is specific |
| **planning** | Sequence and Prioritize action (critical path, fail-fast ordering) | risk_management | Medium | Low — Fail-Fast Ordering exists |
| **planning** | Allocate Resources action (parallel vs sequential, sub-agents) | tracking_and_recovery | High | High — Multi-agent work allocation not covered |
| **positioning** | Assess Context action (analyze task domain, complexity, stakes) | identity_and_profile | Medium | Low — Context analysis implied in techniques |
| **positioning** | Assume Role and Ground action (expert, mentor, peer, assistant + domain) | identity_and_profile | Low | Low — Role Adoption exists |
| **positioning** | Monitor, Adjust, or Escalate action (detect misalignment, self-correct) | identity_and_profile | Medium | Low — Persona Consistency techniques exist |
| **investigating** | Frame Investigation action (assess uncertainty, formulate hypotheses) | thinking | Medium | Low — Diagnostic pattern exists |
| **investigating** | Search and Survey action (breadth-first across sources) | discipline | Medium | Medium — Source enumeration useful |
| **investigating** | Trace and Analyze action (depth-first path following) | thinking | Medium | Low — Causal reasoning covers this |
| **investigating** | Diagnose Causes action (distinguish causes from symptoms) | thinking | Low | Low — Diagnostic pattern exists |
| **evaluating** | Select Criteria action (identify from standards, specs, conventions) | criteria_setting | Low | Low — Select Criteria exists |
| **evaluating** | Apply Evaluation Methods action (tests, metrics, benchmarks, linting) | software_quality | Medium | Medium — Specific methods useful |
| **evaluating** | Collect and Assess Evidence action (list for all possibilities, weigh) | thinking | Medium | Low — Weigh Evidence exists |

---

## Priority Summary

### High Importance + High Risk (Must Consolidate into Protocols)

| Mode | Item | Target Protocol |
|------|------|-----------------|
| base_mode | Expert-reasoned action plan | identity_and_profile |
| base_mode | Inline tracking (conversation not files) | tracking_and_recovery |
| orienting | Summarize State and Flag Decisions | pragmatics |
| implementing | Gather Context (screenshots, errors, bug reports) | elicitation |
| implementing | Simplify (combat bloat post-implementation) | system_modularity |
| planning | Allocate Resources (sub-agents, parallelization) | tracking_and_recovery |

### High Importance + Medium/Low Risk (Should Consolidate)

| Mode | Item | Target Protocol |
|------|------|-----------------|
| planning | Decompose Work with 100% rule | discipline |

### Items for Future Styles Layer

| Mode | Item | Notes |
|------|------|-------|
| brainstorming | Generate Unlikely Alternatives (<0.3 threshold) | Threshold is output constraint, not technique |
| brainstorming | SCAMPER format | Format is style, technique already in thinking |
| brainstorming | Organize Ideas | Output structure for decision-making |
| base_mode | Expert-reasoned action plan output format | Expert framing may be style + technique |

---

## Recommendations

1. **Prioritize "Must Consolidate" items** — These represent unique value that would be lost.

2. **Simplify action is critical** — LLMs tend toward bloat; explicit anti-bloat action in system_modularity is essential.

3. **Allocate Resources action fills a gap** — No protocol currently addresses multi-agent work distribution.

4. **Screenshot/error interpretation** — elicitation protocol should explicitly cover visual and error message context gathering.

5. **Inline tracking distinction** — tracking_and_recovery should distinguish workflow tracking (files) from action tracking (conversation).

6. **Styles layer** — Several items are output patterns/constraints rather than thinking techniques. Consider a separate "styles" layer for these.

---

## Related Documents

- [workflow-creation-guideline.md](../guidelines/workflow-creation-guideline.md) — How to create workflows with actions
- [mode-creation-guideline.md](../guidelines/mode-creation-guideline.md) — Deprecation notice
- [mode-to-protocol-consolidation-guideline.md](../guidelines/mode-to-protocol-consolidation-guideline.md) — How modes were consolidated
