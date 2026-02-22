# Documentation Maintenance Protocol

Maintain documentation as first-class code infrastructure that serves both human developers and AI assistants.

---

## Goal

Ensure documentation remains accurate, synchronized with code, and serves both human developers and AI assistants effectively.

---

## Intent

Documentation is no longer optional—it's essential infrastructure. With AI assistants reading and reasoning about code, documentation quality directly impacts AI effectiveness. Outdated or missing documentation causes AI hallucination, poor code suggestions, and developer confusion. Good documentation enables AI to understand intent, suggest correct patterns, and assist effectively. Documentation must be treated as code: versioned, reviewed, tested, and maintained with the same rigor.

---

## Scope

This protocol addresses documentation maintenance: when to update, what to document, synchronization with code changes, documentation structure for AI comprehension, and prevention of documentation drift.

---

## Key Results

- Documentation stays synchronized with code (updated in same commit as code changes)
- Both humans and AI can quickly understand modules, functions, and dependencies
- AI assistants provide accurate suggestions based on correct context
- Developers can navigate and comprehend unfamiliar code efficiently
- Documentation is discoverable (co-located with code, not scattered in wikis)
- No documentation debt accumulates (drift is caught early)

---

## Essential Concepts

### 1. Documentation as Code

**What it is:** Documentation lives in version control alongside code, updated through the same workflows (pull requests, reviews, CI/CD).

**Why it matters:** Documentation scattered across Confluence, wikis, Jira tickets drifts immediately. Documentation in Git stays synchronized because it's reviewed with code changes.

**Key principle:** If the code changed but documentation didn't, the pull request is incomplete.

**Tools:** Markdown/MDX in Git, docs-as-code platforms ([Mintlify](https://www.mintlify.com/blog/adopt-docs-as-code), [Fumadocs](https://beta.buildwithfern.com/post/docs-as-code-solutions-api-teams)), automated documentation generation.

### 2. Documentation for Two Consumers

**What it is:** Documentation serves both human developers and AI assistants equally. Clear hierarchy, consistent terminology, explicit context, and self-contained examples benefit both.

**Why it matters:** Humans need to understand code quickly. AI needs context to assist accurately. Good documentation structure serves both: [AI requires specific patterns](https://aronhack.com/llm-friendly-documentation-creating-content-that-ai-can-understand-and-process-effectively/) (sufficient context prevents hallucination, concise structure fits token limits), while humans need the same clarity (purpose, examples, intent).

**Three elements both need:**
1. **Facts** - What the code does (signatures, parameters, return values)
   - Humans: Quick reference for how to call functions
   - AI: Accurate suggestions for correct API usage
2. **Philosophy** - Why it exists (intent, design decisions, tradeoffs)
   - Humans: Understanding design rationale for maintenance
   - AI: Context for suggesting appropriate patterns
3. **Patterns** - How to use it (examples, common use cases, anti-patterns)
   - Humans: Learning how to use code correctly
   - AI: Training data for generating correct usage

**Per-module documentation:** Each package/folder needs a README or PACKAGE.md explaining:
- Purpose of this module
- Key dependencies and why
- How it relates to other modules
- Common entry points

This enables AI to quickly understand context without reading entire codebase.

### 3. Proximity Principle

**What it is:** Documentation lives as close as possible to what it documents.

**Why it matters:** Documentation close to code is more likely to be updated. Developers see it during changes. AI can easily find relevant context.

**Hierarchy:**
- **Inline comments** - Complex logic, non-obvious decisions
- **Docstrings** - Function/class behavior, parameters, returns
- **Module docs** - Package/folder README, how components relate
- **Architecture docs** - High-level design, system relationships
- **API docs** - External interfaces, contracts

**Anti-pattern:** Documentation in separate wiki/repo that diverges from code.

### 4. Documentation at Every Level

**What it is:** Every module, class, and function is documented.

**Why it matters:** AI needs complete context to reason correctly. Missing documentation creates blind spots where AI hallucinates or developers guess.

**Required documentation:**

**Module level (README.md or PACKAGE.md in each folder):**
```markdown
# [Module Name]

## Purpose
What this module does and why it exists.

## Dependencies
Key external dependencies and internal module dependencies.

## Structure
Main components and how they relate.

## Entry Points
Common use cases and where to start.
```

**Function level (docstrings):**
```python
def process_order(order: Order, payment: PaymentMethod) -> OrderResult:
    """
    Process customer order with payment.

    Validates order, charges payment, reserves inventory, schedules shipping.
    This is the main orchestration function for order processing.

    Args:
        order: Customer order with items and shipping address
        payment: Payment method (card, PayPal, etc.)

    Returns:
        OrderResult with confirmation number and estimated delivery

    Raises:
        InvalidOrderError: If order fails validation
        PaymentError: If payment processing fails
        InventoryError: If items unavailable

    Example:
        >>> result = process_order(order, payment_method)
        >>> print(result.confirmation_number)
        'ORD-12345'
    """
```

**Class level:**
```python
class OrderProcessor:
    """
    Orchestrates order processing workflow.

    Coordinates validation, payment, inventory, and shipping services.
    Uses saga pattern for transaction management.

    Dependencies:
        - PaymentService: Charges customer
        - InventoryService: Reserves items
        - ShippingService: Schedules delivery

    Thread-safe: No
    Idempotent: Yes (retrying with same order ID is safe)
    """
```

### 5. Synchronization Timing

**What it is:** Documentation updates happen **in the same commit** as code changes.

**Why it matters:** Separating documentation updates from code changes guarantees drift. The documentation is either forgotten or done later without full context.

**Rule:** Pull request that changes behavior without updating docs should be rejected in review.

**Triggers for documentation updates:**
- API signature changes → update docstrings
- New module/package → create README
- Behavior changes → update function docs
- Dependency changes → update module docs
- Architecture changes → update architecture docs

### 6. Testable Documentation

**What it is:** Documentation examples are executable and tested.

**Why it matters:** Outdated examples mislead developers and AI. Testing docs proves they're correct.

**Approaches:**
- **Doctest** - Test code examples in docstrings
- **Example validation** - CI runs documentation examples
- **Link checking** - Detect broken references

**Example:**
```python
def calculate_tax(amount: Decimal, rate: float) -> Decimal:
    """
    Calculate tax on amount.

    >>> calculate_tax(Decimal('100.00'), 0.08)
    Decimal('8.00')

    >>> calculate_tax(Decimal('50.00'), 0.10)
    Decimal('5.00')
    """
```

Run `pytest --doctest-modules` to verify examples work.

---

## Documentation Types

**Standard documentation types** (can be overridden by repo conventions):

### 1. README Files

**Purpose:** First point of contact for understanding a module/package/project

**Where:** Root of project, root of each major package/folder

**Contains:**
- What this module/project does
- Quick start / how to use
- Key dependencies
- Links to other documentation

**Example structure:**
```markdown
# Project/Module Name

## What This Does
Brief description (2-3 sentences)

## Quick Start
Minimal example to get started

## Dependencies
Key external and internal dependencies

## Documentation
- Architecture: See docs/architecture/
- API: See docs/api/
- ADRs: See docs/adr/
```

### 2. Architecture Decision Records (ADRs)

**Purpose:** [Document significant architecture decisions](https://adr.github.io/) with context and consequences

**Where:** `docs/adr/` or `adr/` directory

**When to create:** Every architecturally significant decision (microservices, database choice, framework selection, patterns)

**Format ([MADR template](https://adr.github.io/madr/)):**
```markdown
# ADR-001: [Decision Title]

**Status:** proposed | accepted | deprecated | superseded

**Date:** 2026-02-21

## Context
What forces led to this decision? (technological, political, social, project constraints)

## Decision
What we decided to do (full sentences, active voice)

## Consequences
What happens as a result (positive and negative)

### Positive
- Benefit 1
- Benefit 2

### Negative
- Tradeoff 1
- Tradeoff 2
```

**Benefits:** New team members learn project history, decisions are searchable, prevents re-litigating settled questions

### 3. API Documentation

**Purpose:** External interface contracts

**Where:**
- Inline (docstrings) for individual endpoints/functions
- `docs/api/` for comprehensive API reference
- OpenAPI/Swagger specs for REST APIs

**Must include:** Request/response formats, authentication, error codes, examples

### 4. Module/Package Documentation

**Purpose:** Explain structure and purpose of code organization

**Where:** README.md or PACKAGE.md in each significant folder

**Required for:** Every package/folder with >3 files or >1 responsibility

**Contains:**
- Purpose of this module
- Key dependencies (external and internal)
- Structure (main components)
- Entry points (where to start)
- Design decisions specific to this module

### 5. Inline Documentation

**Purpose:** Explain code logic and intent

**Where:** Comments and docstrings in code files

**When to add:**
- Function/class docstrings (always)
- Complex logic (when not obvious)
- Non-obvious design decisions (why this approach)
- Workarounds or hacks (why it exists, what it fixes)

### 6. Runbooks / How-To Guides

**Purpose:** Step-by-step procedures for common operations

**Where:** `docs/runbooks/` or `docs/how-to/`

**Examples:** Deployment procedures, debugging guides, incident response

### 7. Architecture Diagrams

**Purpose:** Visual representation of system structure

**Where:** `docs/architecture/` with accompanying markdown

**Best practice:** [Diagrams as code](https://www.graphapp.ai/blog/software-documentation-best-practices-a-comprehensive-guide) (Mermaid, PlantUML) - versioned and reviewable

---

## Preventing Documentation Conflicts (SSOT)

### Single Source of Truth Principle

**What it is:** [Each piece of information lives in exactly one authoritative place](https://www.atlassian.com/work-management/knowledge-sharing/documentation/building-a-single-source-of-truth-ssot-for-your-team)

**Why it matters:** Duplicate information drifts immediately. Which copy is correct? SSOT eliminates confusion.

**Rules:**

1. **One canonical location per concept**
   - API behavior → docstring in code (truth is the code)
   - Architecture decisions → ADR (historical record)
   - Module purpose → README in that folder (proximity)
   - High-level architecture → docs/architecture/ (system overview)

2. **Link, don't duplicate**
   ```markdown
   <!-- Bad: Duplicating content -->
   # Project README
   Our authentication uses JWT tokens with 1-hour expiry...

   # auth/README.md
   Our authentication uses JWT tokens with 1-hour expiry...

   <!-- Good: Link to authoritative source -->
   # Project README
   For authentication details, see [auth/README.md](auth/README.md)

   # auth/README.md (SSOT for auth)
   Our authentication uses JWT tokens with 1-hour expiry...
   ```

3. **Explicit ownership**
   - Assign clear ownership for each doc type
   - Example: Module maintainer owns module README, architect owns ADRs

**Preventing conflicts:**
- Reject PRs that duplicate information
- Regular doc audits to find duplication
- Templates that enforce SSOT (link to details, don't repeat)

---

## Keeping Documentation Clean

### 1. Delete Outdated Docs

**Problem:** Stale docs are worse than no docs (mislead readers)

**Solution:** Delete confidently, rely on Git history

**When to delete:**
- Feature removed → delete its docs
- Architecture superseded → mark ADR as deprecated, link to replacement
- Old approach no longer used → remove, don't just mark "deprecated"

**Example:**
```markdown
<!-- Bad: Keeping stale content -->
## Old Authentication (Deprecated - Don't Use)
We used to use sessions but now use JWT...

## New Authentication
We use JWT tokens...

<!-- Good: Delete old, keep only current -->
## Authentication
We use JWT tokens...

<!-- Reference old approach in ADR if history matters -->
See ADR-005 for why we switched from sessions to JWT
```

### 2. Avoid Redundancy

**Problem:** Same information in multiple places drifts

**Solution:** DRY (Don't Repeat Yourself) applies to docs too

**Pattern:**
- Details live in one place (SSOT)
- Other docs link to details
- Summaries can repeat, but reference source

### 3. Version Documentation

**Problem:** Docs for which version of the code?

**Solution:**
- Docs in Git are automatically versioned with code
- For user-facing docs, explicit version tagging
- Tag deprecated APIs in docstrings

**Example:**
```python
def old_api(x):
    """
    Legacy API - use new_api() instead.

    Deprecated in v2.0, will be removed in v3.0.
    """

def new_api(x):
    """
    Improved API with better error handling.

    Added in v2.0.
    """
```

### 4. Enforce Documentation Standards

**In code review:**
- Check docs updated with code
- Check for duplication
- Check for clarity (would AI/human understand?)

**Automated checks:**
- Docstring coverage tools
- Link checkers (catch broken references)
- API spec validation (does implementation match docs?)

---

## Making Information Findable

### Information Architecture Principles

**Goal:** [Users can find information without knowing where it lives](https://www.nngroup.com/articles/ia-study-guide/)

**Key concepts:**
- **Findability:** Locating known information (search, navigation)
- **Discoverability:** [Finding information you didn't know existed](https://www.interaction-design.org/literature/topics/discoverability)

### 1. Consistent Structure

**Pattern:** Same structure in every module

**Template for module README:**
```markdown
# Module Name

## Purpose
What and why (2-3 sentences)

## Dependencies
External and internal deps

## Structure
Key components

## Entry Points
Where to start

## Design Decisions
Link to relevant ADRs
```

**Benefit:** Once you've seen one module's docs, you know how to navigate any module

### 2. Hierarchical Organization

**Principle:** Information organized by scope

```
project/
├── README.md                 # Project overview, links to details
├── docs/
│   ├── architecture/         # System-level architecture
│   ├── adr/                  # Architecture decisions
│   ├── api/                  # API reference
│   └── runbooks/             # Operational procedures
├── module_a/
│   ├── README.md             # Module A specifics
│   └── submodule/
│       └── README.md         # Submodule specifics
└── module_b/
    └── README.md             # Module B specifics
```

**Rule:** Start broad (project README), drill down to specifics (module README)

### 3. Clear Navigation

**In every README:**
- Link to parent (where you came from)
- Link to children (where you can go)
- Link to related (siblings, dependencies)

**Example:**
```markdown
# Module: Payment Processing

⬆️ [Back to Project](../../README.md)

## Structure
- [Stripe Integration](providers/stripe/) - Stripe-specific logic
- [PayPal Integration](providers/paypal/) - PayPal-specific logic
- [Validators](validators/) - Payment validation

## Related Modules
- [Orders](../orders/) - Order processing (uses payments)
- [Inventory](../inventory/) - Inventory management
```

### 4. Search Optimization

**For AI and human search:**
- Use consistent terminology (don't vary terms for same concept)
- Include keywords in headings
- Provide examples (AI learns from examples)

**Example:**
```markdown
<!-- Good: Clear, searchable headings -->
## Authentication: JWT Tokens

## Database: PostgreSQL Connection Pooling

<!-- Bad: Vague headings -->
## How We Do Auth

## DB Stuff
```

### 5. Entry Points

**Every level needs an entry point:**
- **Project:** Main README
- **Documentation:** docs/README.md or docs/index.md
- **Module:** module/README.md

**Each entry point answers:**
- What is this?
- Why does it exist?
- Where do I go next?

---

## Common Documentation Protocols

**These are defaults - repos can override:**

### Protocol 1: Docs-as-Code

- Documentation in Git with code
- Markdown/MDX format
- Updated in same PR as code
- Reviewed like code

### Protocol 2: Proximity

- Documentation lives close to what it documents
- Function docs → docstrings
- Module docs → README in module folder
- Architecture docs → docs/architecture/

### Protocol 3: README Hierarchy

- Every significant folder has README
- README links to parent, children, siblings
- Consistent structure (Purpose, Dependencies, Structure, Entry Points)

### Protocol 4: ADRs for Decisions

- Significant decisions documented in ADRs
- Stored in docs/adr/ or adr/
- Numbered sequentially (ADR-001, ADR-002, ...)
- Include context, decision, consequences

### Protocol 5: SSOT

- Each fact lives in one authoritative place
- Other docs link, don't duplicate
- Delete outdated docs, don't just deprecate

### Protocol 6: Versioned with Code

- Docs in same repo as code
- Tagged with code releases
- Deprecated APIs documented with removal timeline

**Repos can override:** Document repo-specific protocols in top-level DOCUMENTATION.md or CONTRIBUTING.md

---

## Processes

### When Code Changes

1. **Make code change**
2. **Identify affected documentation** - What docs mention this?
3. **Update documentation** - In same commit, update all affected docs
4. **Review together** - Code reviewer checks both code and docs

### During Code Review

**Reviewer checks:**
- Does code change match documentation change?
- Are all affected docs updated?
- Are examples still correct?
- Is documentation clear for both humans and AI?

**Red flags:**
- API changed but docstring unchanged
- New function without docstring
- Behavior changed but docs still describe old behavior

### Documentation Maintenance Checklist

Before merging:
- [ ] Function/class docstrings updated for API changes
- [ ] Module README updated for new components or dependencies
- [ ] Architecture docs updated for structural changes
- [ ] Examples still work (tested via doctest or CI)
- [ ] Comments updated for logic changes

---

## Patterns and Anti-Patterns

### Patterns (✅)

**Pattern 1: Co-located Updates**
- Update docs in same commit as code
- Makes drift impossible
- Example: Feature branch includes both implementation and docs

**Pattern 2: Structured Module Docs**
- Every folder has README.md with: Purpose, Dependencies, Structure, Entry Points
- Enables AI to quickly understand any part of codebase
- Example: See module structure template above

**Pattern 3: Example-Driven Docs**
- Show how to use code, not just what it is
- [Self-contained, testable examples](https://docs.digitalocean.com/products/gradient-ai-platform/concepts/context-management/)
- AI learns correct usage patterns from examples

**Pattern 4: Intent Documentation**
- Document **why**, not just **what**
- Helps AI understand design decisions
- Example: "Uses saga pattern for transaction management" explains approach

### Anti-Patterns (❌)

**Anti-Pattern 1: Documentation Debt**
- Postponing docs "until later"
- Later never comes, documentation drifts
- **Instead:** Update docs in same commit

**Anti-Pattern 2: Scattered Documentation**
- README here, wiki there, Confluence somewhere, Jira tickets
- Impossible to keep synchronized
- **Instead:** Docs-as-code, everything in Git

**Anti-Pattern 3: Implementation Details in Docs**
- Documenting how it works internally (implementation)
- Creates maintenance burden, exposes details that change
- **Instead:** Document behavior and contracts, not internals

**Anti-Pattern 4: Missing Context for AI**
- No module-level docs explaining purpose and dependencies
- AI can't understand how pieces fit together
- **Instead:** README in every folder with purpose, deps, structure

**Anti-Pattern 5: Untested Examples**
- Code examples that don't run or are outdated
- Misleads developers and AI
- **Instead:** Use doctest, validate examples in CI

---

## Examples

### Example 1: API Signature Change

**Context:** Adding optional parameter to existing function

**Code change:**
```python
# Before
def send_email(to: str, subject: str) -> None:
    ...

# After (added body parameter)
def send_email(to: str, subject: str, body: str = "") -> None:
    ...
```

**Documentation update (same commit):**
```python
# Before docstring
"""
Send email to recipient.

Args:
    to: Recipient email address
    subject: Email subject line
"""

# After docstring (updated)
"""
Send email to recipient.

Args:
    to: Recipient email address
    subject: Email subject line
    body: Email body text (default: empty)
"""
```

**Result:** Docs stay synchronized, AI knows about new parameter

### Example 2: New Module Added

**Context:** Creating new payment processing module

**Code:**
```
payments/
├── __init__.py
├── processor.py
├── validators.py
└── providers/
    ├── stripe.py
    └── paypal.py
```

**Documentation (created with module):**

`payments/README.md`:
```markdown
# Payment Processing Module

## Purpose
Handles payment processing for orders. Supports multiple payment providers
(Stripe, PayPal) with unified interface.

## Dependencies
- External: stripe, paypal-sdk
- Internal: orders (validates payment matches order total)

## Structure
- `processor.py` - Main PaymentProcessor orchestrator
- `validators.py` - Payment validation logic
- `providers/` - Provider-specific implementations

## Entry Points
```python
from payments import PaymentProcessor

processor = PaymentProcessor()
result = processor.process(order, payment_method)
```

## Design Decisions
- Strategy pattern for providers (easy to add new payment methods)
- Idempotent processing (safe to retry with same payment_id)
- Saga pattern for rollback if order processing fails after charge
```

**Result:** AI understands module structure, dependencies, and design without reading all code

### Example 3: Behavior Change

**Context:** Changing error handling from returning None to raising exception

**Code change:**
```python
# Before
def get_user(id: str) -> Optional[User]:
    user = db.query(User).filter_by(id=id).first()
    return user  # Returns None if not found

# After
def get_user(id: str) -> User:
    user = db.query(User).filter_by(id=id).first()
    if not user:
        raise UserNotFoundError(f"No user with id {id}")
    return user
```

**Documentation update (same commit):**
```python
# Before
"""
Get user by ID.

Args:
    id: User ID

Returns:
    User if found, None otherwise
"""

# After
"""
Get user by ID.

Args:
    id: User ID

Returns:
    User object

Raises:
    UserNotFoundError: If no user with given ID

Example:
    >>> user = get_user("user123")
    >>> print(user.name)
    'Alice'

    >>> get_user("nonexistent")
    UserNotFoundError: No user with id nonexistent
"""
```

**Result:** Breaking change documented, AI suggests correct error handling

---

## Key Takeaways

- **Documentation is code infrastructure** - treat with same rigor as code
- **AI needs complete context** - module docs, function docs, examples all matter
- **Update docs with code** - same commit, not "later"
- **Proximity wins** - docs close to code stay synchronized
- **Test documentation** - examples should run and be verified
- **Every level matters** - module README, class docs, function docs all essential
- **Structure for AI** - Facts (what), Philosophy (why), Patterns (how)

---

## When to Apply This Protocol

- When implementing code (`implement` primitive) - update docs in same commit
- When designing systems (`design` primitive) - create module structure docs
- During code review (`critique` primitive) - verify docs match code
- When adding new modules - create README with purpose, deps, structure
- When changing APIs - update all affected docstrings and examples
- When refactoring - update docs to reflect new structure

---

## References

- [Software Documentation Best Practices 2026](https://www.qodo.ai/blog/code-documentation-best-practices-2026/)
- [Good Documentation Practices](https://technicalwriterhq.com/documentation/good-documentation-practices/)
- [Documentation as Code](https://medium.com/level-up-web/what-is-documentation-as-code-and-why-do-you-need-it-587b1e5421b2)
- [LLM-Friendly Documentation](https://aronhack.com/llm-friendly-documentation-creating-content-that-ai-can-understand-and-process-effectively/)
- [Context Management for AI Agents](https://docs.digitalocean.com/products/gradient-ai-platform/concepts/context-management/)
- [Docs as Code - Write the Docs](https://www.writethedocs.org/guide/docs-as-code/)
