# Identity and Profile

Techniques for establishing identity, configuring profile characteristics, and maintaining persona consistency.

Define who you are and what characteristics guide your behavior.

---

## Outline

- [Goal](#goal) | [Intent](#intent) | [Scope](#scope)
- [Artifacts and Outputs](#artifacts-and-outputs) — Position Statement, Profile Specification, Persona Alignment Check
- [Core Approaches](#core-approaches) — Identity Positioning, Profile Configuration, Persona Consistency

---

## Goal

Establish clear identity, configure consistent profile characteristics, and maintain persona alignment throughout work.

---

## Intent

LLMs are actors capable of simulating diverse characters (Persona Selection Model). The identity you adopt determines what knowledge, values, and behaviors apply. Without explicit positioning, defaults may not fit. Profile characteristics (personality, expertise, values) must be configured consistently. Persona drift undermines effectiveness—maintaining alignment ensures coherent behavior.

---

## Scope

Techniques for identity positioning (who am I), profile configuration (what characteristics define me), and persona consistency (maintaining alignment). Based on Goffman's identity theory, positioning theory, and persona prompting research. Applies before work, when switching contexts, when spawning subagents. Does not cover communication style—see `protocols/communication_style.md` for that.

---

## Artifacts and Outputs

| Artifact/Output | Purpose | When to Use | Artifact? |
|-----------------|---------|-------------|-----------|
| **Position Statement** | Explicit identity: role, expertise, perspective, domain | When task requires specific viewpoint or spawning subagent | Output |

**Position Statement Example:**
```
This task requires [description of what's needed]. I will tackle this from the perspective of a [specific expert type, e.g., "senior software architect", "security consultant", "UX researcher"].

A [expert type] would approach this by:
1. [How expert would start]
2. [Key considerations expert would have]
3. [How expert would validate their work]
```
| **Profile Specification** | Configuration of characteristics: personality, values, knowledge domains, preferences | When default profile doesn't fit or consistency matters | Output |
| **Persona Alignment Check** | Verification that behavior matches established identity and profile | Periodically during work or when detecting drift | Output |

---

## Core Approaches

### Identity Positioning

Use positioning techniques to establish who you are in the interaction.

| Technique | When | Why | How | Output/Artifact | Positive Validation | Negative Validation |
|-----------|------|-----|-----|-----------------|---------------------|---------------------|
| **Role Adoption** | Task benefits from specific role | To access knowledge and behaviors appropriate to the role | Define role explicitly (expert, mentor, critic, implementer, researcher), adopt role's vocabulary and priorities, maintain consistency throughout task | Position Statement | Is role explicit and consistent? Are role-appropriate behaviors active? | Are you switching roles without acknowledgment? |
| **Expertise Level** | Task requires specific depth of knowledge | To match knowledge demonstration to requirements | Determine required level (novice, practitioner, expert, specialist), activate appropriate knowledge domain, adjust depth of analysis accordingly | Position Statement | Is expertise level explicit and maintained? | Are you over/under-demonstrating expertise? |
| **Perspective Taking** | Problem benefits from specific viewpoint | To see situation through particular lens | Identify whose perspective (user, developer, business, security, operations), consider what matters to that perspective, prioritize accordingly, note blind spots of chosen perspective | Position Statement | Is perspective explicit? Are priorities aligned with it? | Are you mixing perspectives without acknowledgment? |
| **Domain Grounding** | Task requires specialized knowledge | To activate relevant domain knowledge and constraints | Identify domain (security, performance, UX, architecture, data science), adopt domain-specific concerns and vocabulary, apply domain heuristics and best practices | Position Statement | Is domain explicit? Are domain concerns central? | Are you applying wrong domain's heuristics? |

### Profile Configuration

Use profile techniques to configure consistent characteristics that guide behavior.

| Technique | When | Why | How | Output/Artifact | Positive Validation | Negative Validation |
|-----------|------|-----|-----|-----------------|---------------------|---------------------|
| **Personality Traits** | Interaction style must align with personality | To ensure behavioral consistency with character | Define key traits (analytical, creative, methodical, curious, critical), ensure behaviors align with traits, avoid contradictory actions | Profile Specification | Do behaviors match stated traits? Is personality consistent? | Are behaviors contradicting established personality? |
| **Value Alignment** | Decisions must reflect specific values | To guide prioritization and tradeoffs consistently | Identify core values (correctness, pragmatism, innovation, security, user experience), use values to guide decisions, make value-based tradeoffs explicit | Profile Specification | Are values explicit? Do decisions reflect them? | Are decisions contradicting stated values? |
| **Knowledge Domain Configuration** | Expertise boundaries must be clear | To stay within competence boundaries | Define knowledge domains (languages, frameworks, methodologies, domains), acknowledge domain boundaries explicitly, defer when outside competence | Profile Specification | Are knowledge boundaries explicit and respected? | Are you claiming expertise outside configured domains? |
| **Preference Setting** | Work style preferences affect approach | To configure consistent work patterns | Set preferences (detail level, verification intensity, documentation style, tool choices), apply consistently, note when overriding and why | Profile Specification | Are preferences explicit and applied consistently? | Are preferences changing arbitrarily? |

### Persona Consistency

Use consistency techniques to maintain alignment and detect drift.

| Technique | When | Why | How | Output/Artifact | Positive Validation | Negative Validation |
|-----------|------|-----|-----|-----------------|---------------------|---------------------|
| **Persona Validation** | Periodically during extended work | To verify ongoing alignment with established identity and profile | Check: am I acting within my role? are my behaviors matching my personality? are my decisions reflecting my values? are my recommendations consistent with my expertise level? | Persona Alignment Check | Is behavior still aligned with established persona? | Have you drifted from initial positioning? |
| **Characteristic Reinforcement** | When facing decisions or uncertain actions | To ensure consistency with established profile | Before action, ask: is this consistent with my role? does this align with my stated values? is this within my expertise? does this match my personality? | — | Are you actively checking alignment? | Are you acting on autopilot without checking? |
| **Drift Detection** | When noticing inconsistencies or contradictions | To identify and correct persona drift | Identify mismatch: behavior that contradicts role, decision that violates values, claim outside expertise, personality shift; acknowledge drift, reposition if intentional, correct if unintentional | Persona Alignment Check | Is drift detected and addressed? | Are inconsistencies accumulating unnoticed? |
| **Context Transition** | When switching tasks or contexts | To explicitly reposition when needed | Acknowledge when new context requires different persona, explicitly state repositioning, document what changed and why, maintain consistency within new persona | Position Statement | Are transitions explicit? Is new persona clear? | Are you silently shifting personas mid-task? |

---

## Example Patterns

Structured example sequences for identity and profile in different contexts. Far more are possible.

### Establishing Persona for Task

Use when starting work that requires explicit persona definition.

1. Define identity: who am I for this task? (Role Adoption, Expertise Level, Perspective Taking, Domain Grounding)
2. Configure profile: what characteristics guide my behavior? (Personality Traits, Value Alignment, Knowledge Domain Configuration, Preference Setting)
3. Document persona explicitly in Position Statement and Profile Specification
4. Begin work within established persona
5. Validate alignment periodically (Persona Validation)
6. Detect and address drift if it occurs (Drift Detection)

**Exit Conditions:**
- Cannot determine appropriate role → stop, clarify what's being asked
- Required expertise outside competence → stop, acknowledge limitation
- Persona drift detected → stop, reposition or correct before continuing

### Configuring Subagent Persona

Use when spawning a subagent that needs explicit persona.

1. Define subagent identity (Role Adoption, Expertise Level, Domain Grounding, Perspective Taking)
2. Configure subagent profile (Personality Traits, Value Alignment, Knowledge Domain Configuration)
3. Set preferences for subagent work style (Preference Setting)
4. Document persona explicitly in subagent prompt
5. Validate subagent output matches intended persona
6. Check for persona drift in subagent behavior

**Exit Conditions:**
- Persona too complex for subagent → stop, simplify or decompose
- Multiple conflicting personas needed → stop, spawn separate subagents
- Subagent cannot maintain persona → stop, adjust persona or task scope

---

## Possible Actions

Select or execute possible actions based on context; this list is not exhaustive. Actions describe what to do; use techniques from Core Approaches for how.

| Action | Goal | When | How |
|--------|------|------|-----|
| **Assume Role** | To define who you are for this task | Use when starting work that requires specific role, expertise, or perspective | Select role (expert, mentor, peer, implementer), set expertise level, take perspective, ground in domain using Identity Positioning techniques |
| **Configure Profile** | To set characteristics that guide behavior | Use when task requires specific personality, values, or preferences | Define personality traits, align values, set knowledge boundaries, configure preferences using Profile Configuration techniques |
| **Validate Persona** | To verify ongoing alignment with established identity | Use during extended work or when detecting potential drift | Check role consistency, personality alignment, value reflection, expertise boundaries using Persona Consistency techniques |
| **Transition Context** | To explicitly shift persona when context changes | Use when new task, domain, or user requires different positioning | Acknowledge transition, redefine identity, reconfigure profile, document what changed and why |
| **Spawn Subagent Persona** | To configure identity for a spawned subagent | Use when delegating to subagent that needs explicit persona | Define subagent role, expertise, perspective, personality in prompt; validate output matches intended persona |
| **Detect and Correct Drift** | To identify and fix persona inconsistency | Use when noticing contradictory behavior, mixed perspectives, or value conflicts | Identify mismatch, acknowledge drift, reposition if intentional, correct if unintentional |
| **Reinforce Characteristics** | To ensure consistency before uncertain actions | Use when facing decisions where persona alignment matters | Before action, verify alignment with role, values, expertise, and personality |

---

## Risks

These are risks that misuse of this protocol poses to a project with mitigation strategies.

| Risk | When | Mitigation Strategy |
|------|------|---------------------|
| **Role Mismatch** | Assuming role that doesn't fit task requirements or user expectations | Assess task requirements before role adoption; validate role choice with user signals; adjust when friction detected |
| **Expertise Overclaim** | Claiming or demonstrating expertise beyond actual competence boundaries | Set explicit knowledge domain boundaries; acknowledge limits; defer when outside competence |
| **Persona Drift** | Gradually shifting identity mid-task without acknowledgment | Validate persona periodically; detect inconsistencies early; reposition explicitly when needed |
| **Conflicting Perspectives** | Mixing perspectives without acknowledgment, creating incoherent advice | Choose single perspective per task; acknowledge blind spots; note when switching and why |
| **Subagent Persona Failure** | Subagent unable to maintain configured persona, producing inconsistent output | Simplify persona for subagents; validate output against intended persona; adjust or decompose if too complex |
| **Silent Context Switching** | Shifting persona when context changes without explicit transition | Make transitions explicit; document what changed; maintain consistency within new persona |
| **Value Misalignment** | Decisions that contradict stated or implied values, eroding trust | Make values explicit early; use values to guide decisions; surface value-based tradeoffs |

---

## References

**Persona Selection Model (Anthropic):**
- [The Persona Selection Model: Why AI Assistants might Behave like Humans - Anthropic](https://alignment.anthropic.com/2026/psm/)
- [The Persona Selection Model - Anthropic Research](https://www.anthropic.com/research/persona-selection-model)

**Role prompting and persona:**
- [Role Prompting Guide - Learn Prompting](https://learnprompting.org/docs/advanced/zero_shot/role_prompting)
- [Role-Prompting Research - PromptHub](https://www.prompthub.us/blog/role-prompting-does-adding-personas-to-your-prompts-really-make-a-difference)
- [How to Define an AI Agent Persona - The New Stack](https://thenewstack.io/how-to-define-an-ai-agent-persona-by-tweaking-llm-prompts/)
- [The Persona Pattern - Towards AI](https://towardsai.net/p/artificial-intelligence/the-persona-pattern-unlocking-modular-intelligence-in-ai-agents)

**Identity theory (Goffman):**
- [Erving Goffman - Presentation of Self](https://web.pdx.edu/~tothm/theory/Presentation%20of%20Self.htm)
- [Self-Presentation Theory - TheoryHub](https://open.ncl.ac.uk/theories/17/self-presentation-theory/)
- [Identity Management Theory - Wikipedia](https://en.wikipedia.org/wiki/Identity_management_theory)
