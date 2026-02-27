# Modularity Protocol

Enable effective system decomposition through use-case driven design.

---

## Goal

Enable design of systems where components can be understood, modified, and composed independently while maintaining coherence.

---

## Intent

Balance cohesion, coupling, encapsulation, composability, coherence, and traceability based on project needs—these objectives often conflict. **The developer is the user.** Modular systems optimize developer UX: code that's easy to use, understand, and modify. Start with **how the system will be used**, then derive component boundaries to serve those use cases.

---

## Scope

**Addresses:** Use-case driven decomposition, component boundary definition, interface design, tradeoff analysis across modularity objectives

**Does not address:** Performance optimization, deployment architecture, team organization, project management

---

## Core Concepts

**Modularity objectives - evaluation criteria:**

| Objective | Check for strength | Check for excess |
|-----------|-------------------|------------------|
| **Cohesion** | Related functionality in one place | System too fragmented into tiny components |
| **Coupling** | Components can be modified independently | Components can't interact to provide value |
| **Encapsulation** | Internals can change without breaking users | Can't see what's happening during debugging |
| **Composability** | Components combine to create new functionality | Too much abstraction obscures directness |
| **Coherence** | Consistent patterns make behavior predictable | Conventions too rigid, prevent necessary variation |
| **Traceability** | Can follow execution flow and debug issues | Implementation details exposed, high coupling to internals |

Balance based on: expected changes, debugging needs, team knowledge, usage patterns.

---

## Techniques

**Use-case first design:**

Write how you want to use the system before designing components:

```python
# Step 1: Write desired usage
order = Order.create(cart, customer)
order.validate()
payment = PaymentProcessor.charge(order.total, method)
inventory.reserve(order.items)
order.confirm()

# Step 2: Identify natural boundaries from flow
# - Order: domain logic, validation, state
# - PaymentProcessor: external integration
# - Inventory: resource management
```

Then apply design process:

1. **Write 3-5 primary use cases** as ideal API calls
2. **Identify natural boundaries** - Group responsibilities appearing together repeatedly
3. **Define components** - Purpose (what it does/doesn't do), dependencies (internal/external)
4. **Evaluate tradeoffs** - Which objectives optimized? Which sacrificed? Appropriate?
5. **Iterate** - Refactor when boundaries feel wrong or debugging is hard

**Design decisions:**

- Make dependencies explicit, depend on stable interfaces not implementation details
- Hide what changes frequently, expose what needs debugging visibility
- Establish and follow conventions (naming, patterns, error handling)
- Start direct and simple, abstract when patterns emerge

**Evaluation criteria:**

- Can you understand a component without reading the entire system?
- Do common use cases flow naturally through interfaces?
- Do changes localize without rippling?
- Do components compose following consistent patterns?
- Can you trace execution flow and debug issues?
- Is each component's purpose unambiguous?
- Are dependencies explicit and necessary?

---

## Use Cases and Triggers

Apply when:
- Designing new systems or components
- Refactoring code with unclear boundaries
- Component boundaries feel wrong (awkward interfaces, changes ripple)
- Debugging is difficult (can't trace behavior)
- New developers struggle to understand what goes where

---

## Patterns and Anti-Patterns

### Anti-Patterns (❌)

**Dogmatic Encapsulation:**

```python
# Bad: Everything hidden, can't observe or debug
class Cache:
    def __init__(self):
        self._data = {}
        self._hits = 0       # Hidden
        self._misses = 0     # Hidden

# Good: Internals hidden, behavior observable
class Cache:
    def __init__(self):
        self._data = {}
        self.stats = CacheStats()  # Exposed for observability
```

**Component-First Design:** Designing "UserManager, OrderManager, PaymentManager" before understanding flows. Results in awkward interfaces. **Fix:** Use-case first design (see Techniques).

**Premature Abstraction:** Creating flexible components before knowing what variations are needed. **Fix:** Build for current use case, abstract when second real use case emerges.

---

## References

- See `protocols/documentation.md` - Package documentation supports modularity
- See `protocols/quality.md` - Modularity assessment criteria
