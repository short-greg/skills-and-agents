# Spec/Plan Maintenance

How to keep specification and plan documents updated with progress, design changes, challenges, and solutions during work.

---

## Overview

Spec/plan documents (PRDs, technical plans, implementation plans) must be kept up-to-date as work progresses. This creates an audit trail, documents decisions, and helps with handover or resumption.

**Key principle:** Document as you go, don't wait until the end.

---

## Core Requirement

### Every Skill Checklist MUST Include

**A progress update item:**

```markdown
- [ ] N. Always: Update ${SPEC_DIR}/[document-name].md with:
  - Progress made in this phase
  - Design changes or pivots
  - Challenges encountered
  - Solutions implemented
  - Next steps
```

**Why this matters:**
- Creates audit trail of work
- Documents decision rationale
- Shows learning and adaptation
- Helps with handover/resumption
- Prevents forgetting important context

---

## What to Document

### 1. Progress

**What was accomplished in this phase:**

```markdown
**Progress:**
- Completed initial design for authentication module
- Implemented 3 of 5 planned features
- All unit tests written and passing
- 70% of integration tests complete
```

**Include:**
- What phase completed
- What work was done
- Current state (percentages, counts)
- What's working

---

### 2. Design Changes

**Original plan vs actual implementation:**

```markdown
**Design changes:**
- Originally planned synchronous API, switched to async for better performance
- Added Redis cache for session storage (not in original plan)
- Simplified auth flow from 4 steps to 2 steps based on UX feedback
- Removed planned feature X due to technical constraints
```

**Include:**
- What changed from original plan
- Why the change was made
- Impact of the change
- Tradeoffs considered

**Why this matters:**
- Shows adaptation and learning
- Documents why code doesn't match original spec
- Helps future maintainers understand rationale

---

### 3. Challenges

**Problems encountered during work:**

```markdown
**Challenges:**
- Circular dependency between auth and API modules (spent 2 hours debugging)
- Requirement ambiguity: token expiration time not specified in PRD
- Test framework incompatibility with Python 3.12 (workaround needed)
- Performance bottleneck in database queries (N+1 problem)
```

**Include:**
- Technical problems
- Requirement ambiguities
- Tool/framework issues
- How long issues took to resolve
- What was learned

**Why this matters:**
- Documents blockers for future reference
- Shows where requirements need clarification
- Helps estimate similar work later
- Identifies recurring problems

---

### 4. Solutions

**How challenges were resolved:**

```markdown
**Solutions:**
- Circular dependency: Created separate validation module to break cycle
- Token expiration: Asked user, decided on 1 hour (updated PRD)
- Test framework: Pinned to Python 3.11 until framework updates
- N+1 problem: Added select_related() and prefetch_related() calls
```

**Include:**
- How each challenge was solved
- Alternatives considered
- Trade-offs made
- Why this solution was chosen

**Why this matters:**
- Documents solution rationale
- Shows problem-solving approach
- Helps when similar problems arise
- Creates knowledge base

---

### 5. Next Steps

**What comes next:**

```markdown
**Next steps:**
- Proceed to Phase 4 (Write Integration Tests)
- Need to clarify: should refresh tokens be rotating or static?
- Blocked on: Database schema approval from DBA
- Planning to: Add logging for better debugging
```

**Include:**
- Next phase or task
- Open questions
- Blockers
- Planned improvements

**Why this matters:**
- Clear continuation point
- Documents blockers early
- Shows forward planning
- Helps with resumption

---

## Format Example

### Standard Progress Log Format

Add to the end of spec/plan document:

```markdown
---

## Progress Log

### 2026-02-15 14:30 - Phase 3: Design Scratchpad

**Progress:**
- Completed initial design for authentication module
- Identified 2 potential issues with token refresh flow
- Created sequence diagrams for all auth flows

**Design changes:**
- Originally planned synchronous API, switched to async for better performance (10x faster in benchmarks)
- Added Redis cache for session storage (not in original plan, needed for horizontal scaling)

**Challenges:**
- Circular dependency between auth and API modules (spent 2 hours debugging)
  - auth.py needed validate_request() from api.py
  - api.py needed authenticate() from auth.py
- Requirement ambiguity: token expiration time not specified in PRD

**Solutions:**
- Circular dependency: Created separate validation module (validation.py) to break cycle
  - Moved shared validation logic out of API layer
  - Both auth and API now depend on validation (one-way dependencies)
- Token expiration: Asked user, decided on 1 hour for access tokens, 30 days for refresh (documented in PRD)

**Next steps:**
- Proceed to Phase 4 (Write Tests)
- Need to clarify: should refresh tokens be rotating or static? (ask user in next phase)

---

### 2026-02-15 16:45 - Phase 4: Write Tests

**Progress:**
- All unit tests written (24 tests)
- Coverage: 89% (exceeds 85% threshold)
- All tests passing

**Design changes:**
- None (implementation matched design)

**Challenges:**
- Test framework incompatibility with Python 3.12
  - pytest-asyncio not yet compatible
- Mocking Redis proved difficult
  - Standard mocking didn't work with async operations

**Solutions:**
- Python version: Pinned to Python 3.11 in requirements.txt until pytest-asyncio updates
  - Documented in CLAUDE.md as temporary constraint
- Redis mocking: Used fakeredis library instead of unittest.mock
  - Works better with async code
  - Faster than mocking (no network calls)

**Next steps:**
- Proceed to Phase 5 (Implement Feature)
- User confirmed: refresh tokens should be rotating (updated design)

---

### 2026-02-16 10:15 - Phase 5: Implementation

**Progress:**
- Implementation 100% complete
- All unit and integration tests passing
- Code review requested

**Design changes:**
- Added rate limiting (not in original spec)
  - Security best practice for auth endpoints
  - User approved addition

**Challenges:**
- Database query performance issue (N+1 problem)
  - Loading user + permissions + roles took 45 queries
  - Response time 800ms (exceeds 100ms target)

**Solutions:**
- N+1 problem: Added select_related('permissions', 'roles') to queries
  - Reduced to 3 queries total
  - Response time now 42ms (meets target)

**Next steps:**
- Waiting for code review approval
- Will proceed to Phase 6 (Final Assessment) after approval
```

---

## Frequency

### When to Update

**After each major phase:**
- ✅ Design phase complete
- ✅ Implementation phase complete
- ✅ Assessment phase complete
- ✅ When encountering blockers
- ✅ When making significant pivots

**During long phases:**
- ✅ At natural checkpoints (end of day, end of feature)
- ✅ When discovering important information
- ✅ Before pausing work

**Don't wait until the end** - context is lost, details forgotten.

---

## Examples by Context

### PRD (feature-define) Progress Log

```markdown
## Progress Log

### 2026-02-15 - PRD Draft Complete

**Progress:**
- First draft of PRD complete
- All sections filled in
- User stories defined

**Design changes:**
- Scope reduced: Removed batch password reset (deferred to Phase 2)
  - User feedback: "Start with single user reset first"

**Challenges:**
- Ambiguous requirement: "secure" password reset
  - Needed to clarify: email-based? SMS? Security questions?

**Solutions:**
- Clarified with user: email-based with expiring token
- Documented security requirements explicitly

**Next steps:**
- Stakeholder review scheduled for tomorrow
- Will incorporate feedback and finalize
```

### Technical Plan Progress Log

```markdown
## Progress Log

### 2026-02-15 - Architecture Design Complete

**Progress:**
- High-level architecture designed
- Technology stack selected
- Database schema designed
- API contracts defined

**Design changes:**
- Database: Switched from MongoDB to PostgreSQL
  - Need ACID transactions for payment processing
  - Better query performance for analytics

**Challenges:**
- Scaling concern: single database might not handle 100k users
- Security review flagged JWT storage in localStorage

**Solutions:**
- Scaling: Design includes read replicas, can add sharding later if needed
  - Start simple, optimize when needed
- JWT storage: Moved to httpOnly cookies (more secure)

**Next steps:**
- Get architecture review approval
- Start Phase 1: Core Infrastructure Setup
- Question: Should we use Docker Compose or Kubernetes? (ask user)
```

### Implementation Progress Log

(See standard format example above)

---

## Integration with Checklists

### Required Checklist Item

Every skill phase checklist must include:

```markdown
- [ ] N. Always: Update ${SPEC_DIR}/[document-name].md with progress log entry
```

**Example in Phase 3:**

```markdown
## Phase 3: Design Scratchpad

**Checklist:**
- [ ] 1. Design module structure
- [ ] 2. Create sequence diagrams
- [ ] 3. Identify potential issues
- [ ] 4. (If production repo) Assess design modularity
- [ ] 5. Always: Update ${SPEC_DIR}/plan.md with progress log entry
```

---

## Benefits

### Creates Audit Trail

**Know what happened and when:**
- When was each phase completed?
- What decisions were made?
- Who was consulted?

### Documents Decision Rationale

**Know why things are the way they are:**
- Why async instead of sync?
- Why Redis instead of in-memory?
- Why simplified from original design?

### Helps with Handover

**Others can continue your work:**
- Current state clear
- Blockers documented
- Next steps defined
- Context preserved

### Shows Learning

**Demonstrates problem-solving:**
- Challenges encountered
- Solutions implemented
- Tradeoffs considered
- Adaptation and iteration

### Prevents Context Loss

**Details don't get forgotten:**
- Document while fresh
- Capture important decisions
- Record gotchas and workarounds

---

## Common Mistakes

### ❌ Mistake 1: Waiting Until End

**Bad:** "I'll document everything when I'm done"
**Why bad:** You'll forget details, context, rationale

**Good:** Document after each phase
**Why good:** Context is fresh, details are accurate

### ❌ Mistake 2: Only Recording "What"

**Bad:**
```markdown
Progress: Implemented authentication
```

**Good:**
```markdown
**Progress:**
- Implemented JWT-based authentication
- Token expiration: 1 hour access, 30 days refresh
- Refresh tokens are rotating (security best practice)

**Design changes:**
- Switched from session-based to JWT (better for horizontal scaling)

**Challenges:**
- Circular dependency issue (2 hours debugging)

**Solutions:**
- Created separate validation module to break cycle
```

**Why good:** Captures not just what, but how, why, and challenges.

### ❌ Mistake 3: Vague Descriptions

**Bad:**
```markdown
**Challenges:** Had some issues
**Solutions:** Fixed them
```

**Good:**
```markdown
**Challenges:**
- N+1 query problem causing 800ms response time (target: <100ms)
- Circular dependency between auth and API modules

**Solutions:**
- N+1: Added select_related() to queries → 42ms response time
- Circular dependency: Created validation module, broke cycle
```

**Why good:** Specific, measurable, helpful for future reference.

### ❌ Mistake 4: No Next Steps

**Bad:**
```markdown
**Progress:** Phase complete
```

**Good:**
```markdown
**Progress:** Phase 3 complete

**Next steps:**
- Proceed to Phase 4 (Write Tests)
- Need to clarify: refresh token rotation (ask user)
- Blocked on: None
```

**Why good:** Clear continuation point, blockers identified.

---

## Summary

**Every skill checklist MUST include:**
- Progress update item with all 5 components (progress, changes, challenges, solutions, next steps)

**Update frequency:**
- After each major phase
- When encountering blockers or pivots
- Don't wait until end

**What to document:**
1. **Progress** - What was accomplished
2. **Design changes** - What changed from plan and why
3. **Challenges** - Problems encountered
4. **Solutions** - How problems were solved
5. **Next steps** - What comes next, blockers, questions

**Benefits:**
- Audit trail
- Decision rationale
- Easier handover
- Shows learning
- Prevents context loss

**Skills should reference this document:**
- `[See references/spec-maintenance.md for what to document]`
- Required in every checklist: `Update ${SPEC_DIR}/plan.md with progress`
