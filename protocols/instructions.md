# Instructions Protocol

Patterns for writing effective instructions that minimize cognitive load and maximize comprehension.

---

## Goal

Produce instructions that enable successful task completion on first attempt by managing cognitive load appropriately.

---

## Intent

Poorly designed instructions fail because they overload working memory or misalign with task complexity. Cognitive Load Theory identifies three load types: intrinsic (inherent task complexity), extraneous (poor presentation), and germane (learning-focused processing). Instructions succeed when they minimize extraneous load, especially for high intrinsic load tasks.

---

## Scope

**Addresses:** Instruction structure, cognitive load management, clarity patterns, chunking strategies, verification approaches

**Does not address:** Marketing copy, persuasive writing, general technical writing, API documentation structure

---

## Core Concepts

**Cognitive Load Types:**

1. **Intrinsic Load** - Inherent task complexity from interdependent parts or specialized knowledge required
2. **Extraneous Load** - Processing caused by poor instruction presentation (must minimize)
3. **Germane Load** - Mental effort toward learning and schema construction

**Critical relationship:** When intrinsic load is high, extraneous load must be minimized. Complex tasks require simpler presentation. Conversely, simple tasks can include learning-oriented content without overload

---

## Techniques

**Load-Based Adaptation:**
- Match instruction complexity inversely to task intrinsic load. High-load tasks get minimal presentation; low-load tasks can include rationale and alternatives.
- Assess intrinsic load by counting interdependencies and specialized knowledge requirements.

**Chunking:**
- Group steps by cognitive boundaries - where one complete mental model ends and another begins. Example: "Configure Service A" (one unit), "Connect Services A and B" (another unit).
- A cognitive boundary occurs when the reader must shift attention to a different system, concept, or decision context.

**Structure:**
- Gate with prerequisites before instructions. State "Stop if X is not true" upfront, not buried in step 3.
- Branch conditionals at decision points: "Check if X exists. If yes: [steps]. If no: [steps]." Not "In step 5, if the file from step 2..."
- Second person for instructions ("Run the test"), third person for explanations ("The system validates...")

**Verification:**
- After complex steps, state expected outcome: "Output should show X." Enables self-correction without re-reading.
- Include verification as final step for multi-step procedures.

**Safety:**
- Notice hierarchy: Note (informational) < Warning (potential error) < Caution (data loss) < Danger (safety risk). Place before relevant step, never after.
- Test instructions on target audience before deployment. Instructions are artifacts requiring validation.

---

## Use Cases and Triggers

**Apply when:**
- Writing skill instructions for this framework
- Creating workflow step descriptions with decision points
- Defining primitive execution patterns
- Documenting setup procedures requiring prerequisites
- Instruction failure rate is high or task has high intrinsic load

---

## Examples

### Example 1: High Intrinsic Load Task

**Context:** Configuring complex authentication system with multiple services

**Application:** Minimize extraneous load

```markdown
## Prerequisites
- Services A, B, C running
- Admin credentials available
- Port 8080 accessible

## Configuration Steps

1. Configure Service A:
   ```
   service-a config --auth-mode=token
   ```
   Output should show "Auth mode: token"

2. Generate shared secret:
   ```
   service-a secret generate
   ```
   Copy the output token.

3. Configure Service B with token:
   ```
   service-b config --shared-secret=<TOKEN>
   ```

Note: Services will restart automatically.
```

### Example 2: Low Intrinsic Load Task

**Context:** Adding a console log for debugging

**Application:** Can include context and learning

```markdown
## Add Debug Logging

Add a log statement to track user authentication attempts.

**Why:** Helps diagnose authentication failures in production.

**Location:** `auth.js`, inside `validateUser()` function, before the credential check.

**Code:**
```javascript
console.log('Auth attempt:', { username, timestamp: Date.now() });
```

**Verification:** Run the app and trigger login. Log should appear in console.

**Note:** Remove this log before production deployment.
```

---

## References

- Cognitive Load Theory (Sweller et al.) - Foundation for load management strategy
- Technical Writing Best Practices - Clarity and structure patterns
- Safety Notice Hierarchy - Standard from technical documentation practice
