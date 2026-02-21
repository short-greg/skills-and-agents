# Modularity Protocol

Enable effective system decomposition through use-case driven design and multi-objective optimization.

---

## Goal

Enable design of systems where components can be understood, modified, and composed independently while maintaining coherence.

---

## Intent

Modularity is not a set of rules to follow—it's a multi-objective optimization problem with inherent tradeoffs. Systems need the right balance of cohesion, coupling, encapsulation, composability, coherence, and traceability. These objectives often conflict: perfect encapsulation reduces traceability, zero coupling prevents composition, maximum cohesion creates fragmentation.

**The developer is the user.** Good modularity means good developer UX—code that is easy to use, understand, and modify. Expert modular design starts with **how the system will be used**, then optimizes component boundaries to serve those use cases.

---

## Scope

This protocol addresses modular design: use-case driven decomposition, component boundary definition, interface design, and tradeoff analysis across competing modularity objectives (cohesion, coupling, encapsulation, composability, coherence, traceability).

---

## Key Results

- Components can be understood independently without reading the entire system
- Common use cases flow naturally through clean interfaces
- Changes localize to relevant components without rippling
- Components compose together coherently following consistent patterns
- System behavior is traceable through explicit component interactions

---

## Essential Concepts

### 1. Use-Case Driven Design (Developer UX First)

**What it is:** Starting design by writing out how you want common use cases to work, then deriving component structure from those flows.

**Why it matters:** Components exist to serve use cases. The developer is the user—design for good developer UX (easy to use, clear, intuitive). Designing components first, then trying to make use cases work, often produces poor developer UX: awkward interfaces, unclear flows, surprising behavior.

**Approach:**
1. Write out 3-5 primary use cases as if the ideal API already exists
2. Identify natural groupings of responsibilities from those flows
3. Define component boundaries that make those flows clean
4. Only then design the full functionality of each component

**Example:**

```python
# Start here: How do we WANT to use this?
# Use case: Process an order
order = Order.create(items, customer)
order.calculate_totals()
payment = Payment.process(order, payment_method)
inventory.reserve(order.items)
shipping.schedule(order, address)

# This reveals natural components:
# - Order (domain logic)
# - Payment (external integration)
# - Inventory (resource management)
# - Shipping (external integration)

# NOT: Start with "let's make an OrderService class"
```

### 2. Cohesion

**What it is:** The degree to which elements within a component belong together—they serve a unified purpose.

**Why it matters:** High cohesion means changes to related functionality happen in one place. Low cohesion means unrelated changes collide in the same component.

**Tradeoff:** Too much cohesion fragments the system—every tiny responsibility becomes its own component, making composition harder.

**Sweet spot:** Components where "this is where X happens" is clear and unambiguous.

### 3. Coupling

**What it is:** The degree of interdependence between components.

**Why it matters:** Coupling that makes sense is not a bad thing. Components must interact to provide value. The question is whether dependencies are **intentional and necessary** or **accidental and brittle**.

**Good coupling:**
- Explicit dependencies on stable interfaces
- Components depend on what they actually need
- Dependencies flow in consistent directions

**Problematic coupling:**
- Hidden dependencies (global state, implicit assumptions)
- Depending on implementation details that change frequently
- Circular dependencies that prevent independent understanding

**Tradeoff:** Zero coupling means zero composition. The goal is **appropriate** coupling, not minimal coupling.

### 4. Encapsulation

**What it is:** Hiding implementation details behind interfaces.

**Why it matters:** Allows changing internals without breaking users.

**Tradeoff:** Too much hiding reduces **traceability**—you can't see what's actually happening. When debugging or understanding system behavior, complete encapsulation makes it opaque.

**Balance:**
- Hide details that are likely to change
- Expose details needed for understanding, debugging, or extension
- Don't dogmatically hide everything

**Example where hiding is wrong:**

```python
# Over-encapsulated (hard to debug)
class Cache:
    def __init__(self):
        self._data = {}  # Hidden
        self._hits = 0   # Hidden
        self._misses = 0 # Hidden

    def get(self, key):
        # No way to see what's happening!
        ...

# Better (traceable)
class Cache:
    def __init__(self):
        self._data = {}
        self.stats = CacheStats()  # Public for observability

    def get(self, key):
        # Behavior is visible
        ...
```

### 5. Composability

**What it is:** Components can be combined to create new functionality.

**Why it matters:** Composition enables building complex behaviors from simple parts.

**Requirements:**
- Consistent interfaces across similar components
- Components don't make assumptions about their context
- Clear contracts about inputs/outputs

**Tradeoff:** Highly composable systems often require more abstraction, which can reduce directness and clarity.

### 6. Coherence

**What it is:** The system follows consistent conventions, patterns, and logic throughout. Components work in predictable ways.

**Why it matters:** Coherence means once you understand one part, you can predict how other parts work. Reduces cognitive load and learning curve. Makes the system feel well-designed rather than cobbled together.

**Applies to:**
- **Naming conventions** - consistent terminology (get_by_id vs find vs retrieve)
- **Interface patterns** - similar operations have similar signatures
- **Function design** - functions work consistently (pure vs side-effects, sync vs async)
- **Error handling** - consistent approach to failures (exceptions vs results, validation)
- **System design logic** - consistent architectural patterns (how components relate, where logic lives)
- **Abstraction levels** - consistent granularity across similar components

**Example - Interface Coherence:**

```python
# Coherent - consistent pattern
user_service.get_by_id(id)
order_service.get_by_id(id)
product_service.get_by_id(id)

# Incoherent - unpredictable
user_service.find(id)
order_service.retrieve_by_identifier(id)
product_service.fetch_single(id)
```

**Example - Function Design Coherence:**

```python
# Coherent - consistent function style
def calculate_total(items):  # Pure function, no side effects
    return sum(item.price for item in items)

def calculate_tax(amount):  # Pure function, no side effects
    return amount * TAX_RATE

def calculate_shipping(weight):  # Pure function, no side effects
    return weight * SHIPPING_RATE

# Incoherent - inconsistent function behavior
def calculate_total(items):
    return sum(item.price for item in items)  # Pure

def calculate_tax(amount):
    self.tax_total = amount * TAX_RATE  # Side effect!
    return self.tax_total

def calculate_shipping(order):
    shipping = order.weight * SHIPPING_RATE
    order.save()  # Side effect in calculation function!
    return shipping
```

**Example - System Design Logic Coherence:**

```python
# Coherent - consistent responsibility placement
# Business logic always lives in domain objects, services orchestrate
class Order:
    def is_valid(self):  # Business logic in domain
        return self.total > 0 and self.items

class Payment:
    def is_authorized(self):  # Business logic in domain
        return self.status == "authorized"

class OrderService:
    def process(self, order, payment):  # Service orchestrates
        if order.is_valid() and payment.is_authorized():
            ...

# Incoherent - logic scattered inconsistently
# Some logic in domain, some in service
class Order:
    pass  # Just data

class Payment:
    def is_authorized(self):  # Logic here
        return self.status == "authorized"

class OrderService:
    def process(self, order, payment):
        # Order logic leaked into service
        if order.total > 0 and order.items and payment.is_authorized():
            ...
```

**Key aspect:** Coherence means **conventions are established and followed**. Conventions can be explicit (documented) or implicit (emergent from the codebase itself). Both types provide the unwritten (or written) rules that make code manageable. The problem arises when conventions are inconsistently applied: half the code follows pattern A, half follows pattern B. This creates confusion about which pattern is "correct" and forces every reader to hold multiple mental models.

**Explicit conventions:**
- Documented in style guides, architecture docs, or code comments
- Intentionally chosen and communicated
- Example: "All services use dependency injection"

**Implicit conventions:**
- Emerge naturally from existing code patterns
- Discoverable by reading the codebase
- Example: Noticing all error handlers follow the same try/catch/log pattern

Both are valid. What matters is **consistency**—whether the convention is written down or not, following it throughout the codebase enables coherence.

**Coherence enables:**
- Fast learning - understand one component, predict others
- Easy debugging - know where to look for problems
- Confident refactoring - changes follow predictable patterns
- Team efficiency - everyone uses same mental model
- Reduced decision fatigue - conventions answer "how should I do this?"

### 7. Traceability

**What it is:** The ability to follow what the system is doing—see execution flow, understand behavior, debug issues.

**Why it matters:** Systems you can't trace are systems you can't maintain or debug.

**Tension with encapsulation:** Perfect encapsulation hides everything. Perfect traceability exposes everything. Real systems need both.

**Balance:**
- Provide observability hooks (logging, metrics, debug modes)
- Make critical flows explicit and visible
- Allow controlled access to internals for diagnosis

---

## The Multi-Objective Problem

Modularity involves optimizing across competing objectives:

| Objective | Increases | Decreases |
|-----------|-----------|-----------|
| **Cohesion** | Focus, clarity | Composability (too fragmented) |
| **Low Coupling** | Independence | Integration capability |
| **Encapsulation** | Changeability | Traceability, debuggability |
| **Composability** | Reusability | Directness, simplicity |
| **Coherence** | Predictability | Flexibility (convention constrains) |
| **Traceability** | Debuggability | Encapsulation |

**There is no single "best" design.** The right balance depends on:
- How the system will be used
- What kinds of changes are expected
- What failure modes need to be debuggable
- What patterns the team knows

---

## Processes

### 1. Start with Use Cases

Before designing components, write out 3-5 primary use cases as if the ideal API already exists.

Ask: "How would we WANT to use this system?"

### 2. Identify Natural Boundaries

Look at the use cases and find natural groupings:
- What responsibilities appear together repeatedly?
- What operations always happen as a unit?
- What changes for the same reasons?

### 3. Define Component Contracts

For each identified component:
- What is its single clear purpose?
- What operations does it provide?
- What does it depend on?

### 4. Evaluate Tradeoffs

For each design decision, ask:
- Which modularity objectives does this optimize?
- Which does it sacrifice?
- Is that tradeoff appropriate for how we'll use this?

### 5. Iterate Based on Real Use

The first design is rarely optimal. As you use the system:
- Which boundaries feel wrong?
- Where is coupling causing problems?
- Where is abstraction obscuring behavior?
- Refactor to better balance the objectives

---

## Patterns and Anti-Patterns

### Patterns (✅)

**Pattern 1: Use-Case First Design**
- Write how you want to use it before designing components
- Derive boundaries from flow, not from guesses about what "should" be separate
- Example: Write `order.process()` flow, see what components fall out naturally

**Pattern 2: Intentional Coupling**
- Make dependencies explicit and intentional
- Depend on stable interfaces, not implementation details
- Example: `class OrderService: def __init__(self, payment: PaymentInterface)` - explicit, intentional

**Pattern 3: Observability over Opacity**
- Provide ways to see what's happening (logs, metrics, debug modes)
- Don't hide everything behind interfaces
- Example: Expose stats, provide explain() methods, allow introspection

**Pattern 4: Consistent Conventions**
- Use the same patterns for similar operations across components
- Reduces learning curve, increases predictability
- Example: All services use `get_by_id()`, not `find()` vs `retrieve()` vs `fetch()`

### Anti-Patterns (❌)

**Anti-Pattern 1: Component-First Design**
- Designing components without knowing how they'll be used
- Results in awkward interfaces and poor boundaries
- **Instead:** Start with use cases, derive components from flows

**Anti-Pattern 2: Dogmatic Encapsulation**
- Hiding everything "because encapsulation is good"
- Makes debugging impossible, obscures behavior
- **Instead:** Hide what will change, expose what needs to be visible

**Anti-Pattern 3: Minimizing Coupling at All Costs**
- Avoiding dependencies even when they make sense
- Creates awkward indirection and breaks natural composition
- **Instead:** Embrace appropriate coupling with explicit, stable dependencies

**Anti-Pattern 4: Premature Abstraction**
- Creating flexible, composable components before knowing what variations are needed
- Over-engineers simple problems
- **Instead:** Start direct and simple, abstract when patterns emerge

**Anti-Pattern 5: God Objects**
- One component doing everything
- Low cohesion, changes collide
- **Instead:** Split when a component has multiple independent purposes

---

## Examples

### Example 1: E-commerce Order Processing

**Context:** Designing order processing system

**Use-case first approach:**

```python
# Step 1: How do we WANT to use this?
order = Order.create(cart, customer)
order.validate()  # Business rules
payment = PaymentProcessor.charge(order.total, payment_method)
inventory.reserve(order.items)
shipping.schedule(order, address)
order.confirm()

# Step 2: Natural boundaries emerge
# - Order: domain logic, validation, state
# - PaymentProcessor: external payment integration
# - Inventory: resource management
# - Shipping: external shipping integration

# Step 3: Define components based on use case
class Order:
    def validate(self): ...
    def confirm(self): ...

class PaymentProcessor:
    def charge(self, amount, method): ...

class Inventory:
    def reserve(self, items): ...

class Shipping:
    def schedule(self, order, address): ...
```

**Result:** Clean flow, clear responsibilities, appropriate coupling

### Example 2: Traceability vs Encapsulation

**Context:** Cache implementation - need both performance and debuggability

**Bad (over-encapsulated):**

```python
class Cache:
    def __init__(self):
        self._data = {}
        self._hits = 0
        self._misses = 0

    def get(self, key):
        # Everything hidden - can't see what's happening
        ...
```

**Good (traceable):**

```python
class Cache:
    def __init__(self):
        self._data = {}
        self.stats = CacheStats()  # Exposed for observability

    def get(self, key):
        result = self._data.get(key)
        self.stats.record_access(key, hit=result is not None)
        return result

    def debug_info(self):
        # Controlled visibility for debugging
        return {
            'size': len(self._data),
            'hit_rate': self.stats.hit_rate,
            'keys': list(self._data.keys())
        }
```

**Result:** Implementation details hidden, but behavior is traceable

### Example 3: Coherence Through Consistent Interfaces

**Context:** Multiple service classes need consistent patterns

**Bad (incoherent):**

```python
class UserService:
    def find_user(self, id): ...
    def search_users(self, query): ...

class OrderService:
    def get(self, id): ...
    def query(self, filters): ...

class ProductService:
    def retrieve_by_id(self, product_id): ...
    def find_all_matching(self, criteria): ...
```

**Good (coherent):**

```python
class UserService:
    def get_by_id(self, id): ...
    def search(self, criteria): ...

class OrderService:
    def get_by_id(self, id): ...
    def search(self, criteria): ...

class ProductService:
    def get_by_id(self, id): ...
    def search(self, criteria): ...
```

**Result:** Predictable patterns, easy to learn, consistent conventions

---

## Key Takeaways

- **The developer is the user** - good modularity means good developer UX (easy to use, understand, modify)
- **Modularity is multi-objective optimization**, not rule-following
- **Start with use cases** - design how you want to use it, then derive components
- **Tradeoffs are inherent** - perfect cohesion, zero coupling, total encapsulation, and full traceability are mutually exclusive
- **Coupling that makes sense is good** - the goal is appropriate coupling, not minimal coupling
- **Encapsulation vs traceability** - both matter, balance based on what you need to debug
- **Coherence matters** - consistent patterns reduce cognitive load
- **Iterate** - first design is rarely optimal, refine based on actual use

---

## When to Apply This Protocol

- When designing new systems or components
- When refactoring existing code with unclear boundaries
- When evaluating whether code is "well-structured"
- When component boundaries feel wrong (awkward interfaces, changes ripple)
- When debugging is difficult (can't trace what's happening)
- When new team members struggle to understand component purposes
