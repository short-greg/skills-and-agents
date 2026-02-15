# Modularity Assessment Guidelines

Comprehensive framework for evaluating code modularity during implementation.

---

## Overview

Modularity assessment ensures code is maintainable, testable, and follows software engineering best practices. This guide defines **what to measure, how to measure it, and when to measure it**.

**Key principle:** Use **objective, tool-based measurements** instead of subjective self-assessment to avoid LLM over-evaluation bias.

---

## When to Assess Modularity

### Phase 2: Design (BEFORE Implementation)
**Purpose:** Catch modularity issues early in design

**Assess:**
- Proposed module boundaries make sense
- Dependencies are minimal
- Abstraction levels appropriate
- No circular dependencies in design

**Method:** Review scratchpad design, draw dependency graphs

---

### Phase 4: During Implementation (CONTINUOUS)
**Purpose:** Catch issues as they emerge

**Assess after each file/function:**
- Complexity metrics (run after each function)
- Dependency count (check imports)
- Function/class size (line count)

**Method:** Run tools incrementally, fix before moving on

---

### Phase 6: After Implementation (COMPREHENSIVE)
**Purpose:** Final verification before completion

**Assess:**
- All 8 modularity criteria (see below)
- Cross-module dependencies
- Overall architecture quality

**Method:** Full tool suite, comprehensive review

---

## The 8 Modularity Criteria

### 1. Cohesion

**Definition:** Each module/class/function does ONE thing well and all its parts contribute to that one purpose.

**Why it matters:** High cohesion makes code easier to understand, test, and modify.

**How to measure:**

**Automated Tools:**
```bash
# Python: Check for too many public methods (low cohesion indicator)
pylint --disable=all --enable=R0904 src/

# Python: Check for too many instance attributes (low cohesion)
pylint --disable=all --enable=R0902 src/

# JavaScript: Check for too many methods
eslint --rule 'max-classes-per-file: ["error", 1]'

# General: Count public functions per module
find src/ -name "*.py" -exec grep -c "^def " {} \; | awk '$1 > 5 {print}'
```

**Manual Checks:**
- [ ] Can you describe each module's purpose in <10 words?
- [ ] Do all functions in a module relate to the same concept?
- [ ] Could you split this module into smaller, focused modules?

**Thresholds:**
- ✅ **Good:** Module has <5 public functions, all related
- ⚠️ **Warning:** Module has 5-10 public functions
- ❌ **Poor:** Module has >10 public functions or unrelated functions

**Example:**

❌ **Low Cohesion:**
```python
class UserManager:
    def create_user(self):
        pass

    def send_email(self):  # Email logic doesn't belong here
        pass

    def generate_report(self):  # Reporting doesn't belong here
        pass

    def validate_password(self):
        pass
```

✅ **High Cohesion:**
```python
class UserManager:
    def create_user(self):
        pass

    def update_user(self):
        pass

    def delete_user(self):
        pass

class EmailService:  # Separate concern
    def send_email(self):
        pass

class ReportGenerator:  # Separate concern
    def generate_report(self):
        pass
```

---

### 2. Coupling

**Definition:** Modules have minimal dependencies on other modules. Changes in one module don't force changes in many others.

**Why it matters:** Low coupling makes code easier to modify, test in isolation, and reuse.

**How to measure:**

**Automated Tools:**
```bash
# Python: Generate dependency graph
pydeps src/ --show-deps --max-bacon=3
# Look for: Max depth, circular dependencies

# Python: Count imports per file
find src/ -name "*.py" -exec grep -c "^import\|^from" {} \;
# Threshold: <10 imports per file

# Python: Check for circular dependencies
python -m pytest --import-mode=importlib
# Should complete without errors

# JavaScript: Analyze dependencies
madge --circular src/
# Should return no circular dependencies
```

**Manual Checks:**
- [ ] Can you test each module in isolation (with mocking)?
- [ ] How many other modules does this module import? (<10 is good)
- [ ] If you change this module, how many others break? (0-2 is good)
- [ ] Are dependencies one-way (no circular imports)?

**Thresholds:**
- ✅ **Good:** <5 direct dependencies, no circular imports
- ⚠️ **Warning:** 5-10 dependencies
- ❌ **Poor:** >10 dependencies or circular imports

**Example:**

❌ **High Coupling:**
```python
# auth.py imports api.py, api.py imports auth.py (circular)
from api import validate_request  # Shouldn't depend on higher layer
from database import User, Session, Connection, Query, ...  # Too many
from utils import helper1, helper2, helper3, ...  # Too many
```

✅ **Low Coupling:**
```python
# Only import what you need, from appropriate layers
from database import User  # Single import
from .validators import validate_user  # Internal dependency
```

---

### 3. Coherence

**Definition:** Module boundaries make logical sense. Related functionality is grouped together, unrelated functionality is separated.

**Why it matters:** Coherent modules are easier to find, understand, and maintain.

**How to measure:**

**Automated Tools:**
```bash
# Check module organization
tree src/ -L 2
# Does structure match domain concepts?

# Check for "junk drawer" modules (utils, helpers, misc)
find src/ -name "utils.py" -o -name "helpers.py" -o -name "misc.py"
# These often indicate poor coherence
```

**Manual Checks:**
- [ ] Does the module name clearly indicate what's inside?
- [ ] Do all items in the module relate to the same domain concept?
- [ ] If someone needs functionality X, is it obvious which module to check?
- [ ] Are there "junk drawer" modules (utils, helpers, misc)?

**Thresholds:**
- ✅ **Good:** All modules named after domain concepts, no "utils"
- ⚠️ **Warning:** Some generic "utils" or "helpers" modules
- ❌ **Poor:** Many generic modules, unclear organization

**Example:**

❌ **Poor Coherence:**
```
src/
├── utils.py           # What's in here? Who knows!
├── helpers.py         # More mystery code
├── stuff.py           # Very unclear
└── functions.py       # Generic name, unclear purpose
```

✅ **Good Coherence:**
```
src/
├── authentication/    # Clear: handles auth
├── validation/        # Clear: handles validation
├── storage/          # Clear: handles data storage
└── api/              # Clear: API endpoints
```

---

### 4. Abstraction

**Definition:** Appropriate levels of abstraction. High-level modules don't deal with low-level details, and vice versa.

**Why it matters:** Proper abstraction makes code easier to reason about and maintain.

**How to measure:**

**Manual Checks:**
- [ ] Do high-level modules call low-level modules (not vice versa)?
- [ ] Are implementation details hidden behind interfaces?
- [ ] Can you understand what a module does without reading its internals?
- [ ] Are there clear abstraction layers (e.g., API → Business Logic → Data)?

**Thresholds:**
- ✅ **Good:** Clear layers, high-level doesn't know low-level details
- ⚠️ **Warning:** Some layer violations
- ❌ **Poor:** High-level modules manipulate low-level details directly

**Example:**

❌ **Poor Abstraction:**
```python
# High-level API endpoint manipulates database directly
@app.post("/users")
def create_user(data):
    conn = psycopg2.connect("dbname=...")  # Low-level detail in high-level code
    cursor = conn.cursor()
    cursor.execute("INSERT INTO users...")  # SQL in API layer
    conn.commit()
```

✅ **Good Abstraction:**
```python
# High-level API calls business logic
@app.post("/users")
def create_user(data):
    user_service.create_user(data)  # Abstraction hides details

# Business logic calls data layer
class UserService:
    def create_user(self, data):
        return user_repository.save(User(data))  # Another layer

# Data layer handles low-level details
class UserRepository:
    def save(self, user):
        # Database details hidden here
```

---

### 5. Encapsulation

**Definition:** Internal implementation details are hidden. Modules expose only necessary interfaces.

**Why it matters:** Encapsulation prevents external code from depending on internal details, making refactoring safer.

**How to measure:**

**Automated Tools:**
```bash
# Python: Check for private methods being accessed externally
grep -r "\._[a-z]" src/ --include="*.py"
# Should find minimal or no private method access

# Python: Check for protected attributes
pylint --disable=all --enable=W0212 src/
# W0212: Access to protected member

# Count public vs private members
grep -c "^    def [^_]" src/module.py  # Public methods
grep -c "^    def _" src/module.py     # Private methods
# Ratio should favor private (more internal than external)
```

**Manual Checks:**
- [ ] Are internal implementation details marked private (_ prefix)?
- [ ] Do external modules access public interfaces only?
- [ ] Can you change internal implementation without breaking external code?
- [ ] Is the public API minimal (only what's needed)?

**Thresholds:**
- ✅ **Good:** Clear public/private separation, no external access to private
- ⚠️ **Warning:** Some private method access from outside
- ❌ **Poor:** Most methods public, widespread access to internals

**Example:**

❌ **Poor Encapsulation:**
```python
class UserValidator:
    def validate(self, user):
        return self.check_email(user.email)  # Public, but should be private

    def check_email(self, email):  # Should be private
        # Implementation details exposed
        pass

# External code
validator = UserValidator()
validator.check_email(email)  # Accessing internals directly
```

✅ **Good Encapsulation:**
```python
class UserValidator:
    def validate(self, user):  # Public interface
        return self._check_email(user.email)

    def _check_email(self, email):  # Private implementation
        pass

# External code
validator = UserValidator()
validator.validate(user)  # Only uses public interface
```

---

### 6. Testability

**Definition:** Code can be tested in isolation without complex setup. Dependencies can be easily mocked or stubbed.

**Why it matters:** Testable code is easier to verify, debug, and maintain.

**How to measure:**

**Automated Tools:**
```bash
# Run unit tests in isolation (no database, network, filesystem)
pytest tests/unit/ --verbose
# All should pass without external dependencies

# Check test setup complexity
grep -A 10 "def setup\|@pytest.fixture" tests/ | wc -l
# Less setup code = more testable
```

**Manual Checks:**
- [ ] Can you test each function without starting database/server?
- [ ] Is test setup <10 lines per test?
- [ ] Can you mock external dependencies easily?
- [ ] Are side effects (I/O, state changes) isolated?

**Thresholds:**
- ✅ **Good:** Unit tests run instantly, minimal setup, easy mocking
- ⚠️ **Warning:** Some tests need external resources
- ❌ **Poor:** Tests require database/network, complex setup

**Example:**

❌ **Poor Testability:**
```python
class UserService:
    def create_user(self, data):
        # Hard-coded database connection (can't mock)
        conn = psycopg2.connect("postgresql://localhost/db")
        # Direct file I/O (can't test without filesystem)
        with open("/var/log/users.log", "a") as f:
            f.write(f"Created {data['name']}")
        # Global state (can't isolate)
        global_user_count += 1
```

✅ **Good Testability:**
```python
class UserService:
    def __init__(self, db_connection, logger):
        # Injected dependencies (easy to mock)
        self.db = db_connection
        self.logger = logger

    def create_user(self, data):
        # Uses injected dependencies
        self.db.insert(data)
        self.logger.info(f"Created {data['name']}")
        return User(data)

# Test
def test_create_user():
    mock_db = Mock()
    mock_logger = Mock()
    service = UserService(mock_db, mock_logger)  # Easy to mock
    service.create_user({"name": "Alice"})
    mock_db.insert.assert_called_once()  # Easy to verify
```

---

### 7. Reusability

**Definition:** Code can be used in multiple contexts without modification. No tight coupling to specific use cases.

**Why it matters:** Reusable code reduces duplication and makes the codebase more maintainable.

**How to measure:**

**Manual Checks:**
- [ ] Could another part of the system use this module?
- [ ] Are there hard-coded assumptions about the caller?
- [ ] Are interfaces generic, not specific to one use case?
- [ ] Can you use this module without importing the entire application?

**Thresholds:**
- ✅ **Good:** Generic interfaces, no caller assumptions, standalone
- ⚠️ **Warning:** Some context-specific code
- ❌ **Poor:** Tightly coupled to specific use case

**Example:**

❌ **Poor Reusability:**
```python
def send_welcome_email(user_id):
    # Hard-coded to specific use case
    user = get_user_from_database(user_id)
    template = "welcome_email.html"  # Hard-coded template
    send_email(user.email, template)  # Can't reuse for other emails
```

✅ **Good Reusability:**
```python
def send_email(recipient, template_name, context):
    # Generic interface, many use cases
    template = load_template(template_name)
    body = template.render(context)
    mailer.send(recipient, body)

# Can be reused for any email type
send_email(user.email, "welcome.html", {"name": user.name})
send_email(admin.email, "alert.html", {"issue": issue_desc})
```

---

### 8. Complexity

**Definition:** Code is simple and understandable. No overly complex logic, deep nesting, or giant functions.

**Why it matters:** Simple code is easier to understand, debug, and maintain.

**How to measure:**

**Automated Tools:**
```bash
# Python: Cyclomatic complexity
radon cc src/ -a -nb
# Threshold: A-B rating (complexity 1-10)

# Python: Maintainability index
radon mi src/ -nb
# Threshold: A-B rating (MI > 20)

# JavaScript: Complexity
eslint --rule 'complexity: ["error", 10]' src/

# Check function length
find src/ -name "*.py" -exec awk '/^def /,/^def / {count++} END {if(count > 50) print FILENAME, count}' {} \;

# Check nesting depth
grep -E "    ( ){4,}" src/**/*.py  # 4+ levels of indentation
```

**Manual Checks:**
- [ ] Cyclomatic complexity <15 per function
- [ ] Function length <50 lines
- [ ] Nesting depth <4 levels
- [ ] No "god classes" (>500 lines per class)
- [ ] No deeply nested conditionals (if/else pyramids)

**Thresholds:**
- ✅ **Good:** Complexity <10, functions <30 lines, nesting <3
- ⚠️ **Warning:** Complexity 10-15, functions 30-50 lines, nesting 3-4
- ❌ **Poor:** Complexity >15, functions >50 lines, nesting >4

**Example:**

❌ **High Complexity:**
```python
def process_data(data):  # 80 lines, complexity 25
    if data:
        if data.get('type') == 'user':
            if data.get('status') == 'active':
                if data.get('permissions'):
                    if 'admin' in data['permissions']:
                        if validate_admin(data):
                            # 6 levels of nesting!
                            do_admin_stuff()
                        else:
                            return error()
                    else:
                        do_user_stuff()
                else:
                    return no_permissions()
            else:
                return inactive()
        else:
            return wrong_type()
    else:
        return no_data()
```

✅ **Low Complexity:**
```python
def process_data(data):  # 15 lines, complexity 5
    # Guard clauses reduce nesting
    if not data:
        return no_data()

    if data.get('type') != 'user':
        return wrong_type()

    if data.get('status') != 'active':
        return inactive()

    # Delegate to helper functions
    if is_admin(data):
        return process_admin(data)
    else:
        return process_user(data)

def is_admin(data):  # Separate function, simple
    return 'admin' in data.get('permissions', [])
```

---

## Comprehensive Assessment Template

Use this template for final modularity assessment (Phase 6):

```markdown
# Modularity Assessment: [Feature/Module Name]

**Date:** YYYY-MM-DD
**Assessor:** [Skill name]

---

## 1. Cohesion

**Automated Check:**
```bash
pylint --disable=all --enable=R0904,R0902 src/module.py
```
Result: [Output]

**Manual Check:**
- [ ] Module purpose in <10 words: "[Purpose]"
- [ ] All functions related to same concept: ✅ / ❌
- [ ] Could split into smaller modules: ✅ / ❌

**Rating:** ✅ Good / ⚠️ Warning / ❌ Poor
**Evidence:** [Specific evidence]

---

## 2. Coupling

**Automated Check:**
```bash
pydeps src/module.py --show-deps
grep -c "^import\|^from" src/module.py
```
Result: [Number] imports, max depth [Number]

**Manual Check:**
- [ ] Can test in isolation: ✅ / ❌
- [ ] Dependencies <10: ✅ / ❌
- [ ] No circular imports: ✅ / ❌

**Rating:** ✅ Good / ⚠️ Warning / ❌ Poor
**Evidence:** [Specific evidence]

---

## 3. Coherence

**Automated Check:**
```bash
tree src/ -L 2
find src/ -name "utils.py" -o -name "helpers.py"
```
Result: [Output]

**Manual Check:**
- [ ] Module name indicates contents: ✅ / ❌
- [ ] Items relate to same domain: ✅ / ❌
- [ ] No "junk drawer" modules: ✅ / ❌

**Rating:** ✅ Good / ⚠️ Warning / ❌ Poor
**Evidence:** [Specific evidence]

---

## 4. Abstraction

**Manual Check:**
- [ ] Clear abstraction layers: ✅ / ❌
- [ ] High-level doesn't know low-level details: ✅ / ❌
- [ ] Implementation details hidden: ✅ / ❌

**Rating:** ✅ Good / ⚠️ Warning / ❌ Poor
**Evidence:** [Specific evidence]

---

## 5. Encapsulation

**Automated Check:**
```bash
pylint --disable=all --enable=W0212 src/module.py
grep -r "\._[a-z]" src/ --include="*.py"
```
Result: [Output]

**Manual Check:**
- [ ] Private methods marked with _: ✅ / ❌
- [ ] No external access to private: ✅ / ❌
- [ ] Minimal public API: ✅ / ❌

**Rating:** ✅ Good / ⚠️ Warning / ❌ Poor
**Evidence:** [Specific evidence]

---

## 6. Testability

**Automated Check:**
```bash
pytest tests/unit/ --verbose
```
Result: [All pass / Some fail]

**Manual Check:**
- [ ] Tests run without external dependencies: ✅ / ❌
- [ ] Test setup <10 lines: ✅ / ❌
- [ ] Easy to mock dependencies: ✅ / ❌

**Rating:** ✅ Good / ⚠️ Warning / ❌ Poor
**Evidence:** [Specific evidence]

---

## 7. Reusability

**Manual Check:**
- [ ] Could be used elsewhere: ✅ / ❌
- [ ] No hard-coded assumptions: ✅ / ❌
- [ ] Generic interfaces: ✅ / ❌

**Rating:** ✅ Good / ⚠️ Warning / ❌ Poor
**Evidence:** [Specific evidence]

---

## 8. Complexity

**Automated Check:**
```bash
radon cc src/module.py -a -nb
radon mi src/module.py -nb
```
Result: Complexity [Score], MI [Score]

**Manual Check:**
- [ ] Complexity <15 per function: ✅ / ❌
- [ ] Functions <50 lines: ✅ / ❌
- [ ] Nesting depth <4: ✅ / ❌

**Rating:** ✅ Good / ⚠️ Warning / ❌ Poor
**Evidence:** [Specific evidence]

---

## Overall Modularity Rating

**Summary:**
- Cohesion: [✅ / ⚠️ / ❌]
- Coupling: [✅ / ⚠️ / ❌]
- Coherence: [✅ / ⚠️ / ❌]
- Abstraction: [✅ / ⚠️ / ❌]
- Encapsulation: [✅ / ⚠️ / ❌]
- Testability: [✅ / ⚠️ / ❌]
- Reusability: [✅ / ⚠️ / ❌]
- Complexity: [✅ / ⚠️ / ❌]

**Overall Rating:** [Count of each]
- ✅ Good: [Number]
- ⚠️ Warning: [Number]
- ❌ Poor: [Number]

**Pass/Fail:**
- [ ] ✅ **PASS** - All criteria ✅ or ⚠️, no ❌
- [ ] ❌ **FAIL** - One or more criteria ❌

**Required Actions:**
[List specific improvements needed if any ❌ ratings]
```

---

## Common Over-Evaluation Traps

**⚠️ LLMs tend to over-evaluate modularity. Watch for these patterns:**

### Trap 1: "Looks Organized" ≠ Good Modularity

❌ **LLM might say:**
> "Modularity: Good - code is organized into multiple files"

✅ **Require evidence:**
> "Cohesion check: `pylint --enable=R0904` shows 0 violations. Each module has <5 public methods. Module purposes: auth.py (authentication), data.py (data access), api.py (endpoints). ✅ PASS"

### Trap 2: "No Errors" ≠ Low Coupling

❌ **LLM might say:**
> "Coupling: Low - code runs without import errors"

✅ **Require evidence:**
> "Coupling check: `pydeps --max-bacon=2` shows max depth 2. Import count per file: auth.py (3), api.py (4). No circular dependencies detected by pytest. ✅ PASS"

### Trap 3: "Functions Are Small" ≠ Low Complexity

❌ **LLM might say:**
> "Complexity: Low - functions are short"

✅ **Require evidence:**
> "Complexity check: `radon cc -a` shows average complexity 6.2 (Grade A). No functions >15 complexity. Nesting depth check: `grep -E '    ( ){4,}'` finds 0 instances. ✅ PASS"

### Trap 4: "Tests Exist" ≠ Testable

❌ **LLM might say:**
> "Testability: Good - we have tests"

✅ **Require evidence:**
> "Testability check: `pytest tests/unit/` passes all 24 tests without database. Test setup averages 4 lines. All dependencies successfully mocked. ✅ PASS"

---

## Integration with Implementation Skills

Implementation skills (feature-impl, bugfix-impl, refactor-impl) should reference this guide:

**Phase 3: Design Scratchpad** - Assess design modularity BEFORE coding
**Phase 5: Implementation** - Run complexity checks DURING coding
**Phase 7: Final Assessment** - Comprehensive modularity review AFTER coding

See individual skill guidelines for specific integration points.

---

## Tools by Language

### Python
- `pylint` - Style, complexity, design checks
- `radon` - Complexity, maintainability metrics
- `pydeps` - Dependency visualization
- `pytest` - Test isolation verification

### JavaScript/TypeScript
- `eslint` - Complexity rules
- `madge` - Circular dependency detection
- `jscpd` - Code duplication detection
- `complexity-report` - Complexity metrics

### Java
- `checkstyle` - Design rules
- `PMD` - Complexity detection
- `JDepend` - Dependency metrics

### Go
- `gocyclo` - Cyclomatic complexity
- `golint` - Style checker
- `go test` - Test isolation

### Rust
- `cargo clippy` - Linting and complexity
- `cargo test` - Test isolation

---

## Summary

**Modularity is NOT subjective.** Use:
1. **Automated tools** for objective measurement
2. **Concrete thresholds** for pass/fail decisions
3. **Evidence-based assessment** to avoid LLM over-evaluation
4. **Multiple assessment points** (design, during, after implementation)

**All 8 criteria must pass** for good modularity rating.
