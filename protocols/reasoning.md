# Reasoning Protocol

Structured reasoning techniques for problem-solving and verification.

---

## Goal

Apply appropriate reasoning techniques before action and verify completion against success criteria.

---

## Intent

AI tends to rush into solutions without deliberate reasoning, and declare completion without verification. Explicit reasoning patterns prevent poor approaches and premature completion.

---

## Scope

**Addresses:** Reasoning techniques (deductive, inductive, abductive, analogical), pre-action reasoning patterns, post-action verification

**Does not address:** Domain-specific problem-solving methods, debugging strategies, code review patterns

---

## Core Concepts

### Reasoning Techniques

| Technique | Check for strength | Check for excess | When to use |
|-----------|-------------------|------------------|-------------|
| **Deductive** | Premises valid and explicit, conclusions follow logically | Applying rules when premises uncertain | Apply rules/specs to cases |
| **Inductive** | Sufficient observations, pattern holds across cases | Generalizing from too few observations | Identify patterns from observations |
| **Abductive** | Multiple explanations considered, most plausible chosen | Accepting first explanation | Diagnose root cause from symptoms |
| **Analogical** | Structural similarities identified, differences noted | Relying on superficial similarities | Apply lessons from similar cases |
| **Causal** | Intervention supports causation, confounders ruled out | Assuming causation from correlation | Understand why X causes Y |
| **Counterfactual** | Alternative scenarios evaluated, tradeoffs explicit | Endless "what if" analysis | Evaluate alternative scenarios |

### Thinking Techniques

| Technique | Check for strength | Check for excess | When to use |
|-----------|-------------------|------------------|-------------|
| **First Principles** | Problem reduced to fundamental truths, rebuilt from scratch | Reinventing when existing solutions work | Challenge assumptions, rebuild from fundamentals |
| **Strategic** | Long-term vision, systems perspective, big picture | Analysis paralysis, never executing | Plan approach, long-term goals |
| **Tactical** | Immediate next steps clear, actionable plan | Losing sight of overall goal | Determine immediate actions |
| **Creative/Lateral** | Novel approaches explored, indirect solutions | Novelty for novelty's sake | Generate novel solutions |
| **Critical** | Assumptions challenged, evidence evaluated | Endless critique, no construction | Challenge assumptions |
| **Analytical** | Problem decomposed, cause-effect identified | Over-fragmentation, missing gestalt | Break down complex problems |
| **Systems** | Interdependencies mapped, feedback loops identified | Complexity overwhelms, no action | Map interdependencies |

### First Principles Technique (Detail)

**Steps:**
1. **Identify assumptions:** Write down what you assume to be true. Assumptions are invisible until named.
2. **Break down to fundamentals:** Ask "What is this fundamentally made of?" Reduce to basic, undeniable truths.
3. **Rebuild from scratch:** Reason upward from fundamentals. Ignore how things have "always been done."
4. **Test with small experiments:** Validate rebuilt assumptions with pilots or prototypes.

**Example (Musk/SpaceX):** Industry price $65M/launch. First principles: What are rockets made of? Aluminum, titanium, carbon fiber — commodity prices ~2% of launch cost. Gap = inefficiency, not physics. Result: 10x cost reduction.

---

## Techniques

**Pre-action reasoning:**

1. **Assess complexity:** Complex/unfamiliar → strategic reasoning (approach, alternatives, tradeoffs). Moderate → tactical (key considerations, approach, risks). Simple → skip, proceed directly.

2. **Choose reasoning type:** Known rules → Deductive. Multiple observations → Inductive. Unexplained symptoms → Abductive. Similar cases → Analogical. Understanding causes → Causal. Exploring alternatives → Counterfactual. Complex problem → Decomposition. Multiple insights → Synthesis.

3. **Execute:** State premises/observations explicitly. Apply technique. Identify alternatives. Note limitations.

**Post-action verification (always required):**

1. Verify against each success criterion
2. Check completeness
3. Report outcome explicitly (success/failure, why)

**Evaluation criteria:**
- Reasoning type appropriate for problem?
- Premises/observations explicit?
- Conclusions supported?
- Verification complete?

---

## Use Cases and Triggers

Apply when:
- Starting primitives or workflows (choose appropriate reasoning technique)
- Debugging issues (use abductive reasoning to find root cause)
- Designing solutions (use strategic reasoning for approach, tactical for steps)
- Before declaring completion (always verify against success criteria)
- Analyzing patterns across multiple cases (use inductive reasoning)
- Solving new problems similar to past cases (use analogical reasoning)

---

## Patterns and Anti-Patterns

### Patterns (✅)

**Match Reasoning to Problem:** Use deductive for rule application, inductive for pattern finding, abductive for diagnosis, analogical for similar cases.

**Explicit Premises:** State observations, assumptions, and constraints before reasoning. Prevents hidden assumptions.

**Scale Depth to Complexity:** Complex tasks need strategic reasoning (alternatives, tradeoffs). Simple tasks can proceed directly.

**Always Verify:** Verification is non-optional. Check completion against all success criteria.

### Anti-Patterns (❌)

**Wrong Reasoning Type:** Using inductive reasoning (patterns) when deductive (applying rules) is needed. **Fix:** Identify problem type first.

**Skipping Reasoning on Complex Tasks:** Jumping to implementation without considering approach. **Fix:** Use strategic reasoning for high-level plan.

**Skipping Verification:** Declaring completion without checking success criteria. **Fix:** Verification is always required.

**Analysis Paralysis:** Over-reasoning simple tasks. **Fix:** Match reasoning depth to task complexity.

---

## Examples

**Deductive:** "All endpoints must validate (rule). This is endpoint (case). Therefore, add validation."

**Inductive:** "Bug with >10MB files, images, videos. Pattern: Large files → bug. Hypothesis: Buffer overflow."

**Abductive:** "Crashes at 2AM daily. Cron at 2AM. Most plausible: Cron causes crash. Test by disabling."

**Analogical:** "Previous auth used JWT successfully. Similar requirements. Likely: JWT works here. Verify first."

**Causal:** "Cache TTL 5min→1min → response +200ms. Causal: Short TTL → more DB queries."

**Counterfactual:** "Slow performance. What if Redis vs memcache? Evaluate tradeoffs before switch."

**First Principles:** "API slow. Assumption: Need bigger servers. First principles: What does API do? Fetch, transform, return. Fundamentals: DB query 500ms, transform 10ms, network 50ms. Rebuild: Fix query, not servers."

**Strategic:** "Scale to 10M users. Approach: Microservices, caching, DB optimization. Timeline: 6 months."

**Tactical:** "Next: 1) Profile bottleneck 2) Add user table index 3) Deploy and measure."

**Creative/Lateral:** "Login slow, optimization failed. Lateral: Cache entire session? Novel approach works."

**Critical:** "Assumption: Real-time updates needed. Challenge: Really? Survey: Hourly fine. Simplify."

**Analytical:** "Auth system → decompose: user model, hash, sessions, middleware, routes, tests."

**Systems:** "Cache change affects: DB load, response time, memory, cost. Feedback: less cache → more DB → slower."

---

## References

**First principles:**
- [First Principles: The Building Blocks of True Knowledge - Farnam Street](https://fs.blog/first-principles/)
- [Elon Musk's First Principles Thinking - James Clear](https://jamesclear.com/first-principles)

**Foundational reasoning:**
- [Deductive, Inductive and Abductive Reasoning - Butte College](https://www.butte.edu/departments/cas/tipsheets/thinking/reasoning.html)
- [Types of Reasoning - Mathematics LibreTexts](https://math.libretexts.org/Courses/Coalinga_College/Math_for_Educators_(MATH_010A_and_010B_CID120)/06:_Mathematical_Reasoning/6.02:_Types_of_Reasoning)
- [Critical Thinking - Internet Encyclopedia of Philosophy](https://iep.utm.edu/critical-thinking/)
- [Informal Logic - Stanford Encyclopedia of Philosophy](https://plato.stanford.edu/entries/logic-informal/)

**Causal reasoning:**
- [Judea Pearl on LLMs, Causal Reasoning, and AI](https://causalai.causalens.com/resources/blog/judea-pearl-on-the-future-of-ai-llms-and-need-for-causal-reasoning/)
- [Causal AI: How cause and effect will change AI - S&P Global](https://www.spglobal.com/en/research-insights/special-reports/causal-ai-how-cause-and-effect-will-change-artificial-intelligence)

**Computational thinking:**
- [Problem-Solving Techniques in Computational Thinking - Sage Journals](https://journals.sagepub.com/doi/10.1177/21582440241249897)
- [Computational Thinking in Complex Problem Solving - Springer](https://link.springer.com/article/10.1186/s13662-025-03866-3)

**Thinking techniques:**
- [Lateral Thinking - Edward de Bono](https://www.interaction-design.org/literature/topics/lateral-thinking)
- [Strategic Thinking Training - Six Paths Consulting](https://www.sixpathsconsulting.com/strategic-thinking-training/)
- [Critical, Creative, Systems Thinking - ScienceDirect](https://www.sciencedirect.com/science/article/pii/S1871187125002627)
