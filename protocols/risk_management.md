# Managing Complexity, Uncertainty, and Risk

This protocol defines techniques for identifying and mitigating complexity, uncertainty, and risk in workflows.

---

## Overview

Workflows often face three interrelated challenges:

- **Complexity**: The workflow involves many parts, dependencies, or intricate logic
- **Uncertainty**: The workflow's requirements, approach, or outcomes are unclear or unknown
- **Risk**: The workflow has potential for failure, delays, or unintended consequences

These concepts are distinct but related:
- **Complexity** increases the likelihood of errors and makes failures harder to diagnose
- **Uncertainty** means we don't know what complexity exists or what risks we face
- **Risk** is the potential for failure — it's increased by both complexity and uncertainty

Effective workflows identify and address all three.

---

## Complexity

**Definition:** The degree of interconnectedness, dependencies, and intricate logic in a workflow.

### Types of Complexity

**Essential Complexity:**
- Inherent to the problem domain
- Cannot be eliminated, only managed
- Example: A compiler must handle complex grammar rules — that's essential

**Accidental Complexity:**
- Introduced by the solution approach
- Can be reduced or eliminated through better design
- Example: Overly nested conditionals that could be simplified with a lookup table

### Identifying Complexity

Look for:
- High number of steps (>6-7 steps suggests complexity)
- Deep dependencies (Step N depends on Steps A, B, C, D...)
- Conditional branching (multiple paths through workflow)
- Parallel execution with synchronization points
- External dependencies (APIs, services, databases)
- Data transformations between steps
- Large state space (many possible states workflow can be in)

### Mitigating Complexity

**Decomposition:**
- Break complex workflows into smaller sub-workflows or modules
- Each sub-workflow should have a single clear responsibility
- Use composition rather than monolithic workflows

**Abstraction:**
- Hide complexity inside primitives
- Workflow sees primitives as black boxes with clear contracts
- Internal complexity of primitives doesn't leak into workflow

**Simplification:**
- Question whether each step is necessary
- Combine steps where possible without losing clarity
- Remove conditional logic where possible (make deterministic workflows)

**Explicit Dependencies:**
- Make data flow and dependencies explicit in step definitions
- Document which steps depend on which outputs
- Order steps to minimize backtracking

**Modularization:**
- Group related steps together
- Use quality gates between modules (validate outputs before proceeding)
- Enable independent testing of modules

**When to apply:**
- During workflow design (before implementation)
- When workflows grow beyond 6-7 steps
- When failures are hard to diagnose
- When steps have unclear dependencies

---

## Uncertainty

**Definition:** The degree to which requirements, approach, or outcomes are unknown or unclear.

### Types of Uncertainty

**Requirements Uncertainty:**
- What needs to be built is unclear
- Success criteria are ambiguous
- Constraints are unknown
- Example: "Make it faster" — how fast? What defines success?

**Technical Uncertainty:**
- How to achieve the goal is unknown
- Multiple approaches exist, unclear which is best
- Dependencies or integrations are poorly understood
- Example: "Will this API support our use case?" — need to investigate

**Outcome Uncertainty:**
- What will happen when workflow executes is unclear
- Side effects are unknown
- Performance characteristics are unpredictable
- Example: "Will this scale to 1M users?" — need to test

### Identifying Uncertainty

Look for:
- Vague or missing requirements ("make it better")
- Multiple possible approaches with unclear tradeoffs
- Dependencies on external systems not fully understood
- Novel or untested technology
- Phrases like "I think", "probably", "should work"
- Missing success criteria or non-SMARB criteria
- Unknown unknowns (things we don't know we don't know)

### Reducing Uncertainty

**Investigation:**
- Use the `investigate` primitive to explore unknowns before committing
- Read documentation, code, external resources
- Identify what's known vs. unknown
- Surface unknowns explicitly rather than assuming

**Spikes/Prototypes:**
- Create throwaway implementations to test feasibility
- Build minimal proofs-of-concept before full implementation
- Time-box investigation (e.g., "spend 1 hour exploring, then decide")

**Defer Commitment:**
- Don't lock in decisions until uncertainty is resolved
- Use declarative workflows where approach can adapt
- Build flexibility into design (avoid premature optimization)

**Clarify Requirements:**
- Use the `define` primitive to establish SMARB criteria
- Interview users to surface implicit requirements
- Write user stories to clarify intended experience
- Make assumptions explicit and validate them

**Incremental Discovery:**
- Start with high-level design, fill in details progressively
- Use quality gates to validate each increment
- Allow refinement based on learnings

**Escalate to User:**
- When uncertainty can't be resolved, ask the user
- Don't guess or assume — surface the decision point
- Provide options with tradeoffs rather than asking open-ended questions

**When to apply:**
- At workflow start (define and orient reduce uncertainty)
- Before high-commitment steps (design before implement)
- When encountering blockers (investigate before proceeding)
- When assumptions are proven wrong (loop back, refine)

---

## Risk

**Definition:** The potential for failure, delays, or unintended consequences.

### Types of Risk

**Technical Risk:**
- Approach may not work
- Performance may be insufficient
- Integration may fail
- Example: "This algorithm might be too slow for production data"

**Requirements Risk:**
- Building the wrong thing
- Missing critical requirements
- Misunderstanding user needs
- Example: "User might actually need X, not Y"

**Process Risk:**
- Workflow might not complete (interruptions, resource constraints)
- Dependencies might block progress
- Deadlines might be missed
- Example: "External API might be unavailable"

**Quality Risk:**
- Output might not meet standards
- Bugs might escape to production
- Technical debt might accumulate
- Example: "Without tests, regressions are likely"

**Operational Risk:**
- Deployment might break existing systems
- Rollback might be difficult or impossible
- Data loss or corruption
- Example: "Schema migration might fail, corrupting data"

### Identifying Risk

Look for:
- Steps with side effects (database writes, file deletion, API calls)
- Dependencies on external systems (APIs, databases, services)
- Novel or untested approaches
- High-complexity steps
- Steps with uncertain outcomes
- Irreversible actions (data deletion, production deployment)
- Time constraints or deadlines
- Resource constraints (memory, CPU, budget)

### Assessing Risk

For each identified risk, assess:
- **Impact**: How bad if this fails? (low / medium / high / critical)
- **Probability**: How likely is failure? (low / medium / high)
- **Detectability**: Will we know if this fails? (easy / moderate / hard to detect)

Prioritize risks: High impact + High probability = Address immediately

### Mitigating Risk

**Avoidance:**
- Choose a different approach that eliminates the risk
- Simplify to remove risky steps
- Example: Use proven library instead of custom implementation

**Reduction:**
- Decrease probability or impact of failure
- Add validation, testing, quality gates
- Example: Add integration tests to catch API failures early

**Transfer:**
- Move risk to another component or system
- Use external services that handle risky operations
- Example: Use managed database instead of running your own

**Acceptance:**
- Acknowledge risk and proceed anyway
- Document the risk and its potential impact
- Have mitigation plan ready if it occurs
- Example: "API might be slow, but we'll add timeout handling"

**Fail-Fast:**
- Order steps so risky/uncertain items come first
- Surface failures early before investing effort
- Example: Validate API access before building entire integration

**Defensive Measures:**
- Add retry logic for transient failures
- Implement compensation/rollback for side effects
- Add timeouts to prevent hangs
- Validate inputs before processing
- Example: Retry network calls with exponential backoff

**Quality Gates:**
- Validate outputs before they become inputs to next step
- Use `validate` primitive to check success criteria
- Use `critique` primitive to identify issues
- Example: Validate design before implementing

**Incremental Approach:**
- Break risky steps into smaller, safer steps
- Validate each increment before proceeding
- Example: Deploy to staging before production

**Monitoring & Observability:**
- Add logging, metrics, traces
- Enable detection of failures
- Example: Log each step's start/completion for diagnosis

**When to apply:**
- During workflow design (identify risks upfront)
- Before high-impact steps (validate before destructive actions)
- When uncertainty is high (risks are often hidden in unknowns)
- After failures (learn what risks were missed)

---

## Relationships Between Complexity, Uncertainty, and Risk

**Complexity → Risk:**
- More complex workflows have more potential failure points
- Complex dependencies make failures harder to recover from
- **Mitigation**: Reduce complexity through decomposition and abstraction

**Uncertainty → Risk:**
- Unknown requirements increase risk of building wrong thing
- Unknown approaches increase risk of technical failure
- **Mitigation**: Reduce uncertainty through investigation and clarification

**Complexity + Uncertainty → High Risk:**
- Complex and uncertain workflows are most risky
- Failures are both likely and hard to diagnose
- **Mitigation**: Reduce uncertainty first (investigate), then tackle complexity

**Risk → Complexity:**
- Mitigation strategies (retries, compensation, validation) add complexity
- Trade-off: More robust but more complex
- **Mitigation**: Only add complexity where risk justifies it

---

## Applying This Protocol

### During Workflow Design

1. **Identify** complexity, uncertainty, and risk
2. **Assess** which are highest priority to address
3. **Mitigate** using techniques from this protocol
4. **Document** remaining risks and accepted tradeoffs

### During Workflow Execution

1. **Monitor** for unexpected complexity, uncertainty, or risk
2. **Escalate** to user when mitigation strategies fail
3. **Adapt** workflow based on what's discovered
4. **Learn** from failures to improve future workflows

### When to Reference This Protocol

- When designing adaptive workflows
- When workflows have >6-7 steps
- When workflows involve external dependencies
- When requirements are unclear or changing
- When failures occur and root cause is complex
- When deciding between workflow approaches

---

## Summary

| Concept | Definition | Key Techniques |
|---------|------------|----------------|
| **Complexity** | Many parts, dependencies, intricate logic | Decomposition, abstraction, simplification |
| **Uncertainty** | Unknown requirements, approach, or outcomes | Investigation, spikes, defer commitment, clarify |
| **Risk** | Potential for failure or unintended consequences | Fail-fast, quality gates, retry/compensation, incremental |

**General strategy:**
1. Reduce **uncertainty** first (investigate, clarify, prototype)
2. Simplify **complexity** where possible (decompose, abstract, simplify)
3. Mitigate **risk** with appropriate techniques (gates, retries, compensation)
4. Accept remaining complexity/uncertainty/risk with documentation and monitoring


[COMMENT: add more strategies for managing risk such as adding redundancy]