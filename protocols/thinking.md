# Thinking Protocol

Structured thinking techniques for problem-solving and decision-making.

Apply techniques to think effectively.

---

## Outline

- [Goal](#goal) | [Intent](#intent) | [Scope](#scope)
- [Artifacts and Outputs](#artifacts-and-outputs) — Thought Trace, First Principles, Hypothesis, Options Analysis, System Model
- [Core Approaches](#core-approaches) — Reasoning Techniques, Thinking Techniques
- [Example Thinking Patterns](#example-thinking-patterns) — Evidence-Based, Strategic, Creative, First Principles, Diagnostic, Critical, Systems

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

## Artifacts and Outputs

| Artifact/Output | Purpose | When to Use | Artifact? |
|-----------------|---------|-------------|-----------|
| **Thought Trace** | Explicit record of intentions, reasoning, ideas, and ponderings | When thinking process needs to be auditable or reviewable | Output |
| **First Principles** | Assumptions questioned + fundamental truths identified | When challenging conventions or rebuilding from fundamentals | Output |
| **Hypothesis** | Testable explanation for observed symptoms or patterns | When diagnosing problems or exploring unknowns | Output |
| **Options Analysis** | Alternatives evaluated with tradeoffs | When comparing approaches before committing | Output |
| **System Model** | Components, relationships, and decomposition mapped | When understanding complex interdependencies | Either |

---

## Core Approaches

### Reasoning Techniques

Use reasoning techniques to derive logically sound conclusions from premises and evidence.

| Technique | When | Why | How | Output/Artifact | Positive Validation | Negative Validation |
|-----------|------|-----|-----|-----------------|---------------------|---------------------|
| **Deductive** | Known rules/specs must be applied to specific cases | To ensure correct application of established rules | State rule explicitly, identify case, apply rule to case, verify conclusion follows logically | Thought Trace | Are premises valid and explicit? Do conclusions follow logically? | Are you applying rules when premises are uncertain? |
| **Inductive** | Multiple observations suggest a general pattern | To identify patterns that may explain or predict behavior | Gather observations, identify common pattern, state generalization, test against new cases, assess confidence | Thought Trace | Are sufficient observations gathered? Does pattern hold across diverse cases? | Are you generalizing from too few observations? |
| **Abductive** | Symptoms observed but cause is unknown | To find most plausible explanation for diagnosis | List symptoms, generate multiple explanations, assess plausibility of each, choose most likely, state as hypothesis | Hypothesis, Thought Trace | Are multiple explanations considered? Is most plausible chosen with justification? | Are you accepting first explanation without considering alternatives? |
| **Analogical** | Similar problem was solved successfully before | To leverage proven solutions from comparable contexts | Identify similar solved problem, map structural similarities, note key differences, adapt solution to current context, verify fit | Thought Trace | Are structural similarities identified? Are key differences noted? | Are you relying on superficial similarities while ignoring critical differences? |
| **Causal** | Need to understand why X causes Y (not just correlation) | To establish cause-effect relationships for interventions | Observe correlation, identify potential confounders, test intervention, verify effect, rule out alternative explanations | Thought Trace | Does evidence support causation? Are confounders ruled out? | Are you assuming causation from mere correlation? |
| **Counterfactual** | Multiple alternatives exist and need comparison | To evaluate tradeoffs before committing to approach | Define current approach, generate alternative scenarios, evaluate outcomes of each, compare tradeoffs explicitly, document reasoning | Options Analysis | Are alternative scenarios evaluated? Are tradeoffs stated explicitly? | Are you stuck in endless "what if" analysis without deciding? |

### Thinking Techniques

Use thinking techniques to approach problems strategically, creatively, or analytically.

| Technique | When | Why | How | Output/Artifact | Positive Validation | Negative Validation |
|-----------|------|-----|-----|-----------------|---------------------|---------------------|
| **First Principles** | Existing solutions are overly complex, expensive, or yield diminishing returns | To break free from conventional thinking and find optimal solutions | List current assumptions, break problem to fundamental truths, question each assumption, rebuild solution from scratch, test with small experiments | First Principles, Thought Trace | Is problem reduced to fundamental truths? Was solution rebuilt from scratch? | Are you reinventing when existing solutions work well? |
| **Strategic** | Planning approach for complex problem with long-term consequences | To align actions with goals and consider big picture implications | Define long-term goal, map current state, identify gaps, generate alternative approaches, evaluate tradeoffs, choose approach, plan milestones | Options Analysis, Thought Trace | Is long-term vision clear? Is systems perspective maintained? | Are you stuck in analysis without executing? |
| **Tactical** | Strategy is clear, need immediate concrete next steps | To translate strategy into executable actions | Review strategy and goals, identify next actionable steps, prioritize by impact and dependencies, define concrete tasks, set immediate timelines | Thought Trace | Are immediate next steps clear and actionable? | Are you losing sight of overall goal while executing? |
| **Creative/Lateral** | Conventional approaches tried and failed, or problem is novel | To generate non-obvious alternatives through unconventional thinking | Reframe problem from different angles, generate wild ideas without judgment, make unexpected connections, combine disparate concepts, suspend criticism | Options Analysis, Thought Trace | Are novel approaches explored? Are indirect solutions considered? | Are you pursuing novelty for its own sake without practical value? |
| **Critical** | Evaluating claims, proposals, or assumptions before accepting them | To prevent accepting flawed reasoning or weak evidence | Identify claim or assumption, gather supporting evidence, gather contradicting evidence, evaluate source reliability, check for biases, weigh evidence quality | Thought Trace | Are assumptions challenged? Is evidence rigorously evaluated? | Are you endlessly critiquing without offering alternatives? |
| **Analytical** | Problem has many interacting components that must be understood separately | To make complex problems tractable through systematic decomposition | Identify problem components, decompose into sub-problems, analyze each separately, identify relationships and dependencies, synthesize understanding | System Model, Thought Trace | Is problem decomposed systematically? Are cause-effects identified? | Are you over-fragmenting and losing sight of the whole? |
| **SCAMPER** | Existing idea or solution needs variations explored | To systematically generate alternatives through structured transformations | Decompose idea into components, apply each transformation: Substitute (replace component), Combine (merge with other ideas), Adapt (adjust for different context), Modify (change attributes), Put to other uses (repurpose), Eliminate (remove components), Reverse (invert or reorder) | Options Analysis, Thought Trace | Are all transformation types applied? Are variations distinct from original? | Are you mechanically applying transformations without evaluating viability? |
| **Synthesis** | Multiple ideas, components, or insights need integration | To combine elements into cohesive new solutions | Identify elements to combine, find complementary aspects, merge compatible parts, resolve conflicts between elements, create unified solution, validate coherence | Options Analysis, Thought Trace | Are elements meaningfully integrated? Is result coherent and viable? | Are you forcing incompatible elements together? |
| **Systems** | Changes may have ripple effects or unintended consequences across components | To prevent local optimizations that create global problems | Map system components, identify inputs/outputs for each, trace feedback loops, identify leverage points, predict second-order effects, consider unintended consequences | System Model, Thought Trace | Are interdependencies mapped? Are feedback loops identified? | Is complexity preventing any action? |
| **Novelty Assessment** | Generating solutions where obvious approaches likely exist but may not be best | To ensure creative, non-obvious alternatives are considered | For each idea assess likelihood (0-1 scale), identify high-likelihood ideas (>0.7), deliberately generate low-likelihood alternatives (<0.3), document estimates | Thought Trace | Are low-likelihood ideas (<0.3) generated? Are likelihood estimates explicit? | Are all ideas high-likelihood (>0.7), defaulting to obvious? |

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

## Possible Actions

Select or execute possible actions based on context; this list is not exhaustive. Actions describe what to do; use techniques from Core Approaches for how.

| Action | Goal | When | How |
|--------|------|------|-----|
| **Explore Possibilities** | To generate options without premature judgment | Use when ideating, when stuck, or when conventional approaches fail | Suspend evaluation; follow tangents; make unexpected connections; combine disparate concepts; generate quantity over quality |
| **Shift Perspective** | To see problem differently by changing viewpoint | Use when stuck, constraints seem contradictory, or need fresh insight | View from different stakeholders; challenge the problem frame; apply first principles; consider alternative contexts |
| **Reason Sequentially** | To build logically from premises to conclusions | Use when constructing arguments, verifying logic, or following chains of reasoning | Identify premises; apply rules step-by-step; follow cause-effect chains; document each step |
| **Validate Reasoning** | To check that conclusions follow from premises | Use when conclusions drawn and need verification | Check logical validity; verify each step follows; assess confidence level; identify gaps or leaps |
| **Identify Premises** | To surface assumptions and rules being relied upon | Use when starting reasoning or when conclusions seem shaky | List assumptions explicitly; identify rules or specs being applied; state what is taken as given |
| **Challenge Assumptions** | To question premises before building on them | Use when proceeding on inherited beliefs or solution feels over-constrained | Question each assumption for necessity; distinguish fundamental truths from conventions |
| **Generate Alternatives** | To produce diverse options for consideration | Use when options needed or only one approach visible | Generate realistic alternatives; apply SCAMPER; include low-probability ideas |
| **Decompose** | To break complex problems into manageable parts | Use when problem too large or components need separate analysis | Identify components; map relationships; analyze separately; identify dependencies |
| **Synthesize** | To combine elements into coherent wholes | Use when multiple ideas or insights need integration | Find complementary aspects; merge compatible parts; resolve conflicts; validate coherence |
| **Formulate Hypothesis** | To create testable explanation for observations | Use when diagnosing problems or explaining patterns | List symptoms; generate explanations; assess plausibility; state confirmation/refutation criteria |
| **Formulate Strategy** | To develop approach for complex goals | Use when planning multi-step or long-term objectives | Define goal; map current state; generate approaches; evaluate tradeoffs; plan milestones |
| **Analyze Strategy** | To evaluate soundness of a plan or approach | Use when reviewing strategy before execution | Check alignment with goals; identify gaps/risks; verify assumptions; assess feasibility |
| **Weigh Evidence** | To assess strength and relevance of evidence | Use when evidence gathered and verdict needed | List evidence for each possibility; assess quality; identify gaps; weigh for/against |
| **Evaluate Tradeoffs** | To compare options against criteria | Use when multiple alternatives exist and decision needed | Define criteria; assess each option; document tradeoffs; recommend with rationale |
| **Map System Dependencies** | To identify how parts affect each other | Use when changes may have ripple effects | Map components and relationships; trace feedback loops; predict second-order effects |

---

## Risks

These are risks that misuse of this protocol poses to a project with mitigation strategies.

| Risk | When | Mitigation Strategy |
|------|------|---------------------|
| **Analysis paralysis** | Excessive thinking without acting | Set thinking time limits; recognize when "good enough" is sufficient; bias toward action with course correction |
| **Premature conclusion** | Insufficient thinking leads to wrong direction | Ensure multiple alternatives considered; validate key assumptions before committing; test hypotheses |
| **Confirmation bias** | Seeking only supporting evidence | Actively seek disconfirming evidence; consider alternative explanations; weigh evidence objectively |
| **Tunnel vision** | Narrowing to one approach too early | Generate alternatives before evaluating; use lateral thinking techniques; defer judgment during ideation |
| **Complexity blindness** | Missing second-order effects and interactions | Apply systems thinking; trace feedback loops; consider unintended consequences before acting |
| **False confidence** | Concluding with certainty despite weak evidence | State confidence levels explicitly; acknowledge gaps in evidence; distinguish correlation from causation |

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
