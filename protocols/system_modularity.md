# System Modularity

Techniques for decomposing systems into components that can be understood, modified, and composed independently.

Design from use cases, then derive component boundaries to serve those use cases.

---

## Outline

- [Goal](#goal) | [Intent](#intent) | [Scope](#scope)
- [Artifacts and Outputs](#artifacts-and-outputs) — Structural Design, Process Design, Use Case Diagram, Example Workflow, Tradeoff Assessment
- [Core Approaches](#core-approaches) — Decomposition Techniques, Interface Design, Tradeoff Evaluation
- [Example Patterns](#example-patterns) — Designing Component Boundaries, Refactoring Unclear Boundaries

---

## Goal

Enable design of systems where components can be understood, modified, and composed independently while maintaining coherence.

---

## Intent

Balance cohesion, coupling, encapsulation, composability, coherence, and traceability based on project needs—these objectives often conflict. The developer is the user. Modular systems optimize developer UX: code that's easy to use, understand, and modify. Start with how the system will be used, then derive component boundaries to serve those use cases.

---

## Scope

Techniques for use-case driven decomposition, component boundary definition, interface design, and tradeoff analysis across modularity objectives. Does not address performance optimization, deployment architecture, or team organization.

---

## Artifacts and Outputs

| Artifact/Output | Purpose | Examples | Artifact? |
|-----------------|---------|----------|-----------|
| **Structural Design** | Understand what components make up the system and how they relate | Dependency diagram, UML class/component diagram, entity-relationship diagram | Either |
| **Process Design** | Understand the mechanics and behavior of the system and how it flows | Algorithm flowchart, data-flow diagram, sequence diagram, state machine | Either |
| **Use Case Diagram** | Understand how actors interact with the system | UML use case diagram, user journey map | Either |
| **Example Workflow** | Test usability by showing how interfaces compose in code | Code snippet demonstrating typical usage | Output |
| **Tradeoff Assessment** | Document modularity decisions and rationale | Cohesion vs coupling analysis, encapsulation choices | Output |

---

## Core Approaches

### Decomposition Techniques

Use decomposition techniques to break systems into well-bounded components.

| Technique | When | Why | How | Output/Artifact | Positive Validation | Negative Validation |
|-----------|------|-----|-----|-----------------|---------------------|---------------------|
| **Use-Case First** | Designing new system or component | To derive boundaries from actual usage patterns | Write 3-5 primary use cases as ideal API calls, identify what responsibilities appear together, let usage patterns reveal natural groupings | Use Case Diagram | Do use cases flow naturally through interfaces? Were boundaries derived from usage? | Were components named before understanding flows (UserManager, OrderManager)? |
| **Natural Boundaries** | Multiple responsibilities in one place | To identify where to split components | Group responsibilities that appear together repeatedly, separate responsibilities that change for different reasons, look for clusters in call patterns | Structural Design | Do changes localize without rippling? Is each component's purpose unambiguous? | Do changes require modifying multiple unrelated components? |
| **Single Responsibility** | Component does too many things | To ensure each component has clear purpose | Define what component does AND doesn't do, ensure one reason to change, keep component explainable in one sentence | Structural Design | Can you explain purpose in one sentence? Does component have one reason to change? | Does component have multiple unrelated reasons to change? |
| **Dependency Mapping** | Relationships between components unclear | To make coupling explicit and manageable | List what each component depends on, distinguish internal vs external dependencies, depend on stable interfaces not implementations | Structural Design | Are dependencies explicit and necessary? Do dependencies point toward stability? | Are there circular dependencies? Dependencies on implementation details? |

### Interface Design

Use interface design techniques to define clear contracts between components.

| Technique | When | Why | How | Output/Artifact | Positive Validation | Negative Validation |
|-----------|------|-----|-----|-----------------|---------------------|---------------------|
| **Explicit Dependencies** | Component needs external resources | To make coupling visible and testable | Pass dependencies as parameters, avoid hidden globals or singletons, declare what component needs to function | Structural Design | Are all dependencies declared? Can component be tested in isolation? | Does component reach into global state? Are dependencies hidden? |
| **Stable Interfaces** | Interface may need to evolve | To allow internals to change without breaking users | Expose what's needed for use cases, hide what changes frequently, version interfaces when breaking changes needed | Example Workflow | Can internals change without breaking users? Is interface minimal? | Are implementation details exposed? Does internal change break callers? |
| **Change Isolation** | Some parts change more than others | To prevent changes from rippling | Put volatile logic behind stable abstractions, isolate external integrations, separate configuration from logic | Structural Design | Do changes stay contained? Are external systems isolated? | Do small changes require updates across codebase? |
| **Convention Following** | Team needs consistent patterns | To make behavior predictable across components | Establish naming conventions, use consistent error handling, follow same patterns for similar operations | — | Are patterns consistent? Can developers predict behavior? | Does each component follow different conventions? |

### Tradeoff Evaluation

Use tradeoff evaluation techniques to balance competing modularity objectives.

| Technique | When | Why | How | Output/Artifact | Positive Validation | Negative Validation |
|-----------|------|-----|-----|-----------------|---------------------|---------------------|
| **Cohesion Assessment** | Deciding what belongs together | To keep related functionality in one place | Check if responsibilities are related, verify component isn't too fragmented, ensure concept isn't split across components | Tradeoff Assessment | Is related functionality in one place? | Is system fragmented into tiny components? Is concept scattered? |
| **Coupling Assessment** | Deciding component independence | To allow independent modification | Check if components can change independently, verify interactions don't create hidden dependencies, assess impact of change | Tradeoff Assessment | Can components be modified independently? | Do components share too much internal knowledge? |
| **Encapsulation Assessment** | Deciding what to hide vs expose | To balance information hiding with debuggability | Hide what changes frequently, expose what needs debugging visibility, provide observability without coupling | Tradeoff Assessment | Can internals change safely? Can you observe behavior? | Is everything hidden (can't debug)? Is everything exposed (can't change)? |
| **Traceability Assessment** | Deciding abstraction level | To maintain ability to understand and debug | Check if execution can be followed, verify debugging is possible, ensure behavior is predictable | Tradeoff Assessment | Can you trace execution flow? Can you debug issues? | Is abstraction so deep you can't see what's happening? |

---

## Example Patterns

Structured example sequences for system modularity in different contexts. Far more are possible.

### Designing Component Boundaries

Use when creating a new system or major feature.

1. Write 3-5 primary use cases as ideal API calls (Use-Case First)
2. Identify responsibilities that appear together repeatedly (Natural Boundaries)
3. Define each component: purpose, what it does/doesn't do (Single Responsibility)
4. Map dependencies: internal, external, direction (Dependency Mapping)
5. Design interfaces: minimal, stable, explicit (Stable Interfaces)
6. Evaluate tradeoffs: which objectives optimized? sacrificed? (Tradeoff Evaluation)
7. Iterate when boundaries feel wrong or debugging is hard

**Exit Conditions:**
- Can't write clear use cases → stop, clarify requirements first
- Component boundaries feel forced → stop, revisit with different grouping
- Changes ripple across components → stop, boundaries may be wrong

### Refactoring Unclear Boundaries

Use when existing code has modularity problems.

1. Identify symptoms: awkward interfaces, rippling changes, hard debugging
2. Write how code should ideally be used (Use-Case First)
3. Compare ideal usage to current structure
4. Identify natural boundaries from usage patterns (Natural Boundaries)
5. Plan incremental refactoring toward new boundaries
6. Add stable interfaces before moving logic (Stable Interfaces)
7. Move logic behind interfaces incrementally

**Exit Conditions:**
- Ideal usage unclear → stop, interview users of the code
- Too many dependencies to untangle → stop, extract smaller piece first
- Tests insufficient for safe refactoring → stop, add tests first

---

## References

- [Cohesion and Coupling - Wikipedia](https://en.wikipedia.org/wiki/Cohesion_(computer_science))
- [Single Responsibility Principle - Wikipedia](https://en.wikipedia.org/wiki/Single-responsibility_principle)
- [Dependency Inversion Principle - Wikipedia](https://en.wikipedia.org/wiki/Dependency_inversion_principle)
- [Information Hiding - Wikipedia](https://en.wikipedia.org/wiki/Information_hiding)
