# Risk Management

Systematic techniques for identifying, assessing, and mitigating risks in workflows.

Identify risks early, mitigate proactively, fail fast when risks materialize.

---

## Outline

- [Goal](#goal) | [Intent](#intent) | [Scope](#scope)
- [Artifacts and Outputs](#artifacts-and-outputs) — Risk List, Dependency Map, Accepted Risk Record, Failure Report
- [Core Approaches](#core-approaches) — Risk Identification, Risk Mitigation, Risk Monitoring
- [Example Patterns](#example-patterns) — Workflow Risk Assessment, Fail-Fast Implementation

---

## Goal

Enable workflows to identify and address risks before they cause failures or require costly rework.

---

## Intent

Workflows fail for predictable reasons: unrecognized complexity, hidden uncertainty, and unmitigated risks. The cost of early identification is far less than the cost of late failure. Fail-fast principles ensure failures surface early before significant effort is invested. This protocol provides techniques to recognize, assess, and handle risks systematically.

---

## Scope

Techniques for identifying risks (complexity, uncertainty, dependencies, irreversibility), assessing risk severity, mitigating through ordering and design, and monitoring during execution. Applies to workflow design, technical decisions, and any multi-step process with failure potential.

---

## Artifacts and Outputs

| Artifact/Output | Purpose | When to Use | Artifact? |
|-----------------|---------|-------------|-----------|
| **Risk List** | Identified risks with impact × probability assessment | When documenting risks for a workflow | Either |
| **Dependency Map** | External systems, APIs, services the workflow relies on | When analyzing external failure points | Either |
| **Accepted Risk Record** | Risk accepted with rationale and contingency plan | When proceeding despite unmitigated risk | Yes |
| **Failure Report** | What failed, context, and diagnostic information | When fail-fast triggers and work stops | Output |

---

## Core Approaches

### Risk Identification

Use identification techniques to surface risks before they cause failures.

| Technique | When | Why | How | Output/Artifact | Positive Validation | Negative Validation |
|-----------|------|-----|-----|-----------------|---------------------|---------------------|
| **Complexity Indicators** | Workflow has many steps or conditionals | To identify where cognitive overload causes errors | Count steps (>7 is high), count conditionals/branches, identify nested logic, note state dependencies between steps | Risk List | Are complex areas identified? Is step count within limits? | Are there >7 steps without decomposition? Nested conditionals? |
| **Uncertainty Indicators** | Requirements or approach is unclear | To surface unknowns before they cause wrong work | Look for vague words ("probably", "should work"), missing success criteria, novel/untested approaches, assumptions without verification | Risk List | Are unknowns explicit? Are assumptions documented? | Are there unstated assumptions? Vague requirements? |
| **Dependency Analysis** | Workflow relies on external systems or outputs | To identify failure points outside your control | Map external dependencies (APIs, services, files), identify timing dependencies, note what happens if dependency fails | Dependency Map | Are dependencies explicit? Are failure modes known? | Are external dependencies assumed reliable? |
| **Irreversibility Assessment** | Actions may be hard to undo | To identify actions requiring extra caution | List destructive operations (delete, overwrite), identify actions affecting shared state, note actions visible to others | Risk List | Are irreversible actions flagged? Is confirmation required? | Can destructive actions happen without warning? |

### Risk Mitigation

Use mitigation techniques to reduce risk likelihood or impact.

| Technique | When | Why | How | Output/Artifact | Positive Validation | Negative Validation |
|-----------|------|-----|-----|-----------------|---------------------|---------------------|
| **Fail-Fast Ordering** | Some steps have higher failure probability | To surface failures before investing effort | Order risky/uncertain steps first, validate assumptions early, check prerequisites before starting, test integrations before building | — | Are high-risk steps ordered early? Are assumptions validated first? | Are risky steps deferred? Can you invest effort before knowing it's viable? |
| **Decomposition** | Complexity exceeds manageable threshold | To reduce cognitive load and isolate failures | Break into sub-workflows (<7 steps each), give each module single responsibility, define clear interfaces between modules | — | Are modules <7 steps? Is responsibility clear? Can failures be isolated? | Are modules too large? Is responsibility scattered? |
| **Defensive Measures** | Failures may occur despite mitigation | To limit damage when failures happen | Add input validation, implement retry logic for transient failures, include rollback/compensation for state changes, set timeouts | — | Are failure handlers present? Is rollback possible? | Can failures cascade without containment? |
| **Risk Avoidance/Transfer** | Risk can be eliminated or shifted | To remove risk rather than mitigate | Choose proven approaches over novel ones, use managed services for risky operations, defer risky features to later phases | — | Is the lowest-risk approach chosen? Are risky operations externalized? | Are unnecessary risks being taken? |

### Risk Monitoring

Use monitoring techniques to track and respond to risks during execution.

| Technique | When | Why | How | Output/Artifact | Positive Validation | Negative Validation |
|-----------|------|-----|-----|-----------------|---------------------|---------------------|
| **Quality Gates** | Workflow has multiple phases | To catch issues before they cascade | Add validation between major steps, define pass/fail criteria at gates, stop on failure rather than continuing | — | Are gates between phases? Are criteria explicit? Does failure stop execution? | Can issues cascade across phases? Are gates missing criteria? |
| **Verification Points** | Complex steps need confirmation | To confirm expected outcomes before proceeding | State expected outcome after risky steps, verify state before dependent steps, check external dependencies are available | — | Are outcomes verified? Are dependencies checked? | Are you assuming success without checking? |
| **Acceptance Documentation** | Risks must be accepted rather than eliminated | To make accepted risks explicit and reviewable | Document risk being accepted, state rationale for acceptance, describe contingency if risk materializes, get explicit approval | Accepted Risk Record | Are accepted risks documented? Is rationale clear? Is contingency defined? | Are risks implicitly accepted without documentation? |
| **Escalation Triggers** | Risk exceeds acceptable threshold | To involve appropriate decision-makers | Define when to escalate (impact threshold, uncertainty level), identify who to escalate to, provide options not just problems | Failure Report | Are escalation criteria defined? Is escalation path clear? | Can high-impact risks proceed without review? |

---

## Example Patterns

Structured example sequences for risk management in different contexts. Far more are possible.

### Workflow Risk Assessment

Use when designing a new workflow or evaluating an existing one.

1. Count steps and identify complexity indicators (Complexity Indicators)
2. Surface unknowns and assumptions (Uncertainty Indicators)
3. Map external dependencies (Dependency Analysis)
4. Flag irreversible actions (Irreversibility Assessment)
5. Assess each risk: impact (low/medium/high/critical) × probability
6. Order high-probability risks first (Fail-Fast Ordering)
7. Decompose complex sections (Decomposition)
8. Add quality gates between phases (Quality Gates)
9. Document accepted risks (Acceptance Documentation)

**Exit Conditions:**
- Critical impact risks without mitigation → stop, address before proceeding
- Uncertainty about requirements → stop, clarify with interviewing protocol
- Complexity cannot be decomposed → stop, reconsider approach

### Fail-Fast Implementation

Use when implementing a workflow with external dependencies or novel approaches.

1. List all external dependencies and assumptions
2. Order validation of dependencies first
3. Test novel approaches with spike/prototype before committing
4. Validate each assumption before dependent work
5. Add verification points after each risky step
6. Stop immediately on failure, don't continue hoping
7. Report failure with context for diagnosis

**Exit Conditions:**
- Dependency unavailable → stop, report and await resolution
- Assumption invalid → stop, reassess approach
- Novel approach fails spike → stop, consider alternatives

---

## References

**Anthropic/Claude documentation:**
- [Security - Claude Code Docs](https://docs.anthropic.com/en/docs/claude-code/security)
- [Configure permissions - Claude API Docs](https://docs.anthropic.com/en/docs/claude-code/sdk/sdk-permissions)
- [Subagents in the SDK - Claude API Docs](https://docs.anthropic.com/en/docs/claude-code/sdk/subagents)

**Risk management:**
- [Software Development Risks - Itransition](https://www.itransition.com/software-development/risks)
- [Risk Management in Software Development - Comidor](https://www.comidor.com/blog/productivity/risk-management-software-development/)
- [Software Risk Assessment - BrowserStack](https://www.browserstack.com/guide/software-risk-assessment)

**Fail-fast principle:**
- [Fail-fast system - Wikipedia](https://en.wikipedia.org/wiki/Fail-fast_system)
- [The Fail Fast Principle - CodeReliant](https://www.codereliant.io/p/fail-fast-pattern)
- [Fail Fast Principle - Enterprise Craftsmanship](https://enterprisecraftsmanship.com/posts/fail-fast-principle/)

**Complexity management:**
- [Cognitive Complexity - DX](https://getdx.com/blog/cognitive-complexity/)
- [A Guide to Cognitive Complexity - Typo](https://typoapp.io/blog/cognitive-complexity-in-software)
