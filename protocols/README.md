# Protocols

This directory contains protocols that define techniques and patterns used by workflow actions.

---

## What is a Protocol?

A protocol is a collection of techniques and patterns that actions in workflows apply to achieve specific outcomes. Protocols ensure consistency and quality across the framework.

**Protocols are:**
- **Reusable** — Referenced by multiple workflow actions
- **Technique-focused** — Define how to think/do, not what to build
- **Foundational** — Provide common patterns that actions depend on

**Protocols are NOT:**
- Skills (user-invocable)
- Workflows (multi-step processes with actions)

---

## Available Protocols

### Thinking & Reasoning

**[thinking.md](thinking.md)**
- Cognitive techniques for reasoning and analysis
- Generating alternatives, evaluating tradeoffs
- Strategic thinking, evidence-based reasoning

**[discipline.md](discipline.md)**
- Systematic enumeration and completeness
- MECE (Mutually Exclusive, Collectively Exhaustive) techniques
- Sequential processing patterns

**[pragmatics.md](pragmatics.md)**
- Context assessment and state summarization
- Option presentation techniques
- Context reconstruction

### Communication & Elicitation

**[elicitation.md](elicitation.md)**
- Techniques for gathering information through questioning
- Interview patterns and clarification strategies
- Inferring from context before asking

**[communication_style.md](communication_style.md)**
- Communication preferences and stance
- Verbosity, formality, technical level
- Mode selection (direct, exploratory, systematic)

**[instruction_giving.md](instruction_giving.md)**
- Techniques for giving clear instructions
- Imperative vs declarative approaches

### Goals & Criteria

**[goal_setting.md](goal_setting.md)**
- Problem/opportunity framing
- Goal decomposition techniques
- Boundary setting and scope definition

**[criteria_setting.md](criteria_setting.md)**
- Defining success criteria
- Operational definitions
- Binary criteria for pass/fail assessment

### Risk & Quality

**[risk_management.md](risk_management.md)**
- Risk assessment techniques
- Fail-fast ordering
- Contingency planning

**[software_quality.md](software_quality.md)**
- Quality assessment across dimensions
- Correctness, completeness, reliability
- Code quality evaluation

**[system_modularity.md](system_modularity.md)**
- Decomposition techniques
- Simplification patterns
- Anti-bloat actions

### Identity & Transparency

**[identity_and_profile.md](identity_and_profile.md)**
- Role adoption techniques
- AI personality configuration
- Persona consistency

**[transparency.md](transparency.md)**
- Decision documentation
- Rationale recording
- Audit trail patterns

### Execution & Recovery

**[tracking_and_recovery.md](tracking_and_recovery.md)**
- Progress tracking techniques
- Trace detection and recovery
- Dual tracking patterns
- Resume decision making

---

## When to Use Each Protocol

### During Action Execution

- **thinking.md** — When generating alternatives, evaluating options, strategic planning
- **discipline.md** — When systematic enumeration or sequential processing needed
- **pragmatics.md** — When assessing context or presenting options
- **elicitation.md** — When gathering information through questions
- **tracking_and_recovery.md** — REQUIRED for all workflows (progress tracking, recovery)

### During Design & Planning

- **goal_setting.md** — When defining problems, goals, or scope
- **criteria_setting.md** — When establishing success criteria
- **risk_management.md** — When assessing risks or ordering actions fail-fast
- **thinking.md** — When generating design alternatives

### During Implementation

- **discipline.md** — When implementing systematically
- **software_quality.md** — When assessing code quality
- **system_modularity.md** — When decomposing or simplifying
- **tracking_and_recovery.md** — When recording progress

### During Evaluation

- **software_quality.md** — When evaluating quality across dimensions
- **criteria_setting.md** — When applying pass/fail criteria
- **transparency.md** — When documenting decisions

### During Communication

- **communication_style.md** — When calibrating communication approach
- **elicitation.md** — When gathering requirements
- **identity_and_profile.md** — When adopting roles or personas

---

## Protocol Structure

Each protocol follows this pattern:

```markdown
# Protocol Name

Brief description.

---

## Techniques

Table of techniques with:
| Technique | Purpose | When to Use | How to Apply | Related | Success Indicators | Pitfalls |

---

## Possible Actions

Actions that apply the techniques for specific outcomes.

---

## Summary

Key takeaways and reference contexts.
```

---

## Creating New Protocols

**When to create a protocol:**
- Pattern is used by multiple workflow actions
- Techniques need standardization
- Quality concern needs systematic approach

**When NOT to create a protocol:**
- Used by only one workflow action (put it there instead)
- Too specific to a use case (belongs in a skill)
- No clear techniques to define (needs research first)

**How to create a protocol:**
1. Follow the protocol structure above
2. Define clear techniques with purpose and application
3. Include possible actions
4. Reference from workflow actions that use it

---

## Contributing

When adding a new protocol:
1. Use the structure above
2. Add entry to this README
3. Update "When to Use" section
4. Reference from relevant workflow actions
