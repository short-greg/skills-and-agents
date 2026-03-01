# Thinking Protocol

Structured thinking techniques for problem-solving and decision-making.

Apply techniques to think effectively.

---

## Goal

Think effectively with techniques before and after actions to optimize the outcome.

---

## Intent

Thinking allows AI to dissect problems and solutions to optimize for a good output. Explicit reasoning patterns prevent poor approaches and premature completion.

---

## Scope

General problem-solving techniques including reasoning methods, thinking approaches, and verification patterns.

---

## Core Approaches

### Reasoning Techniques

Use reasoning techniques to derive logically sound conclusions from premises and evidence.

| Technique | When | Why | How | Positive Validation | Negative Validation |
|-----------|------|-----|-----|---------------------|---------------------|
| **Deductive** | Known rules/specs must be applied to specific cases | To ensure correct application of established rules | State rule explicitly, identify case, apply rule to case, verify conclusion follows logically | Are premises valid and explicit? Do conclusions follow logically? | Are you applying rules when premises are uncertain? |
| **Inductive** | Multiple observations suggest a general pattern | To identify patterns that may explain or predict behavior | Gather observations, identify common pattern, state generalization, test against new cases, assess confidence | Are sufficient observations gathered? Does pattern hold across diverse cases? | Are you generalizing from too few observations? |
| **Abductive** | Symptoms observed but cause is unknown | To find most plausible explanation for diagnosis | List symptoms, generate multiple explanations, assess plausibility of each, choose most likely, state as hypothesis | Are multiple explanations considered? Is most plausible chosen with justification? | Are you accepting first explanation without considering alternatives? |
| **Analogical** | Similar problem was solved successfully before | To leverage proven solutions from comparable contexts | Identify similar solved problem, map structural similarities, note key differences, adapt solution to current context, verify fit | Are structural similarities identified? Are key differences noted? | Are you relying on superficial similarities while ignoring critical differences? |
| **Causal** | Need to understand why X causes Y (not just correlation) | To establish cause-effect relationships for interventions | Observe correlation, identify potential confounders, test intervention, verify effect, rule out alternative explanations | Does evidence support causation? Are confounders ruled out? | Are you assuming causation from mere correlation? |
| **Counterfactual** | Multiple alternatives exist and need comparison | To evaluate tradeoffs before committing to approach | Define current approach, generate alternative scenarios, evaluate outcomes of each, compare tradeoffs explicitly, document reasoning | Are alternative scenarios evaluated? Are tradeoffs stated explicitly? | Are you stuck in endless "what if" analysis without deciding? |

### Thinking Techniques

Use thinking techniques to approach problems strategically, creatively, or analytically.

| Technique | When | Why | How | Positive Validation | Negative Validation |
|-----------|------|-----|-----|---------------------|---------------------|
| **First Principles** | Existing solutions are overly complex, expensive, or yield diminishing returns | To break free from conventional thinking and find optimal solutions | List current assumptions, break problem to fundamental truths, question each assumption, rebuild solution from scratch, test with small experiments | Is problem reduced to fundamental truths? Was solution rebuilt from scratch? | Are you reinventing when existing solutions work well? |
| **Strategic** | Planning approach for complex problem with long-term consequences | To align actions with goals and consider big picture implications | Define long-term goal, map current state, identify gaps, generate alternative approaches, evaluate tradeoffs, choose approach, plan milestones | Is long-term vision clear? Is systems perspective maintained? | Are you stuck in analysis without executing? |
| **Tactical** | Strategy is clear, need immediate concrete next steps | To translate strategy into executable actions | Review strategy and goals, identify next actionable steps, prioritize by impact and dependencies, define concrete tasks, set immediate timelines | Are immediate next steps clear and actionable? | Are you losing sight of overall goal while executing? |
| **Creative/Lateral** | Conventional approaches tried and failed, or problem is novel | To generate non-obvious alternatives through unconventional thinking | Reframe problem from different angles, generate wild ideas without judgment, make unexpected connections, combine disparate concepts, suspend criticism | Are novel approaches explored? Are indirect solutions considered? | Are you pursuing novelty for its own sake without practical value? |
| **Critical** | Evaluating claims, proposals, or assumptions before accepting them | To prevent accepting flawed reasoning or weak evidence | Identify claim or assumption, gather supporting evidence, gather contradicting evidence, evaluate source reliability, check for biases, weigh evidence quality | Are assumptions challenged? Is evidence rigorously evaluated? | Are you endlessly critiquing without offering alternatives? |
| **Analytical** | Problem has many interacting components that must be understood separately | To make complex problems tractable through systematic decomposition | Identify problem components, decompose into sub-problems, analyze each separately, identify relationships and dependencies, synthesize understanding | Is problem decomposed systematically? Are cause-effects identified? | Are you over-fragmenting and losing sight of the whole? |
| **Systems** | Changes may have ripple effects or unintended consequences across components | To prevent local optimizations that create global problems | Map system components, identify inputs/outputs for each, trace feedback loops, identify leverage points, predict second-order effects, consider unintended consequences | Are interdependencies mapped? Are feedback loops identified? | Is complexity preventing any action? |
| **Novelty Assessment** | Generating solutions where obvious approaches likely exist but may not be best | To ensure creative, non-obvious alternatives are considered | For each idea assess likelihood (0-1 scale), identify high-likelihood ideas (>0.7), deliberately generate low-likelihood alternatives (<0.3), document estimates | Are low-likelihood ideas (<0.3) generated? Are likelihood estimates explicit? | Are all ideas high-likelihood (>0.7), defaulting to obvious? |

---

## Example Thinking Patterns

Structured example sequences of steps for different thinking contexts. Far more are possible.

### Evidence-Based Reasoning

Use when making conclusions based on available evidence.

1. State premises and observations explicitly
2. Validate evidence strength (sufficient, reliable, relevant)
3. If evidence is incomplete or weak → stop, request more evidence or acknowledge uncertainty
4. If evidence is sufficient → state conclusions with confidence level
5. Document reasoning and evidence trail

### Strategic Thinking

Use when planning approach for complex, long-term goals.

1. Define long-term goal and success criteria
2. Map current state and constraints
3. Identify gaps between current and target state
4. Generate alternative approaches
5. Evaluate tradeoffs (cost, time, risk, complexity)
6. Choose approach with clear rationale
7. Plan major milestones and checkpoints

**Exit Conditions:**
- Cannot define goal or context insufficient → stop, return to calling skill
- Current state cannot be determined → stop, request more information
- No viable approaches can be generated → stop, escalate to user

### Creative Problem-Solving

Use when generating novel solutions or exploring alternatives.

1. Define problem and constraints clearly
2. Generate diverse ideas without judgment (defer evaluation)
3. For each idea, assess novelty (0-1 likelihood scale)
4. If all ideas high-likelihood (> 0.7) → force generation of low-likelihood alternatives
5. Build on and combine ideas to create hybrids
6. Evaluate ideas only after sufficient diversity achieved
7. Select promising directions based on criteria

### First Principles

Use when challenging assumptions or rebuilding from fundamentals.

1. Identify current assumptions about the problem
2. Break down to fundamental truths (what is undeniably true)
3. Question each assumption (is this actually necessary?)
4. Rebuild solution from fundamentals, ignoring conventions
5. Test rebuilt approach with small experiments
6. Validate against reality, iterate as needed

**Exit Conditions:**
- Cannot identify any assumptions → stop, problem may not benefit from first principles
- Cannot identify fundamental truths → stop, problem too abstract or ill-defined

### Diagnostic

Use when finding root cause of problems or explaining symptoms.

1. Observe and document symptoms clearly
2. Generate multiple possible explanations (hypotheses)
3. For each hypothesis:
   - State what would confirm it
   - State what would refute it
   - Assess initial plausibility
4. Test most likely hypothesis first
5. Verify explanation accounts for all symptoms
6. If refuted, test next hypothesis
7. Document root cause with supporting evidence

**Exit Conditions:**
- Cannot generate any plausible hypotheses → stop, request more information or context
- Cannot test hypotheses (no access, insufficient resources) → stop, escalate

### Critical Analysis

Use when evaluating claims, assumptions, or proposed solutions.

1. Identify claim or assumption to evaluate
2. Gather supporting evidence
3. Gather contradicting evidence (actively seek disconfirmation)
4. Evaluate evidence quality (reliable sources, reproducible)
5. Check for hidden assumptions or biases
6. Weigh evidence for multiple interpretations
7. State conclusion with confidence level and caveats

**Exit Conditions:**
- No evidence available (supporting or contradicting) → stop, return inconclusive result

### Systems Thinking

Use when understanding complex interdependencies.

1. Map components and their relationships
2. Identify inputs, outputs, and transformations for each
3. Trace feedback loops (positive and negative)
4. Identify leverage points (high impact interventions)
5. Predict second-order effects of changes
6. Consider unintended consequences
7. Document system model and key insights

**Exit Conditions:**
- System too complex to map or components cannot be identified → stop, simplify scope or escalate

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
