# Frame of Mind

Techniques for establishing identity, interaction frame, and stance before working.

Position yourself and define the interaction to guide behavior.

---

## Outline

- [Goal](#goal) | [Intent](#intent) | [Scope](#scope)
- [Artifacts and Outputs](#artifacts-and-outputs) — Position Statement, Interaction Frame, Stance Specification
- [Core Approaches](#core-approaches) — Identity Positioning, Interaction Framing, Stance Setting
- [Example Patterns](#example-patterns) — Establishing Frame for Task, Setting Frame for Subagent

---

## Goal

Establish who you are, what kind of interaction this is, and how you will engage before starting work.

---

## Intent

Interactions require implicit social understanding. Without explicit framing, defaults may not fit. "Who am I?" determines what knowledge and values apply. "What are we doing?" determines appropriate behaviors. "How will I engage?" determines tone and approach. When spawning subagents, you must specify their frame explicitly.

---

## Scope

Techniques for identity positioning (who am I), interaction framing (what are we doing), and stance setting (how will I engage). Based on Goffman's frame analysis and positioning theory. Applies before work, when switching tasks, when spawning subagents.

---

## Artifacts and Outputs

| Artifact/Output | Purpose | When to Use | Artifact? |
|-----------------|---------|-------------|-----------|
| **Position Statement** | Explicit identity: role, expertise, perspective | When task requires specific viewpoint or spawning subagent | Output |
| **Interaction Frame** | Definition of what kind of activity this is | When interaction type affects appropriate behavior | Output |
| **Stance Specification** | Orientation and engagement mode | When default stance doesn't fit the situation | Output |

---

## Core Approaches

### Identity Positioning

Use positioning techniques to establish who you are in the interaction.

| Technique | When | Why | How | Output/Artifact | Positive Validation | Negative Validation |
|-----------|------|-----|-----|-----------------|---------------------|---------------------|
| **Role Adoption** | Task benefits from specific role | To access knowledge and behaviors appropriate to the role | Define role explicitly (expert, mentor, critic, implementer), adopt role's vocabulary and priorities, maintain consistency | Position Statement | Is role explicit and consistent? | Are you switching roles without acknowledgment? |
| **Expertise Level** | Audience requires calibrated depth | To match complexity to audience | Determine required level (novice, practitioner, expert), adjust vocabulary and assumptions, include/exclude background accordingly | Position Statement | Is expertise level explicit? Does output match? | Are you over/under-explaining? |
| **Perspective Taking** | Problem benefits from specific viewpoint | To see situation through particular eyes | Identify whose perspective (user, developer, business, security), consider what matters to that perspective, prioritize accordingly | Position Statement | Is perspective explicit? Are priorities aligned? | Are you mixing perspectives without acknowledgment? |
| **Domain Grounding** | Task requires specialized knowledge | To activate relevant domain knowledge | Identify domain (security, performance, UX, architecture), adopt domain-specific concerns and vocabulary, apply domain heuristics | Position Statement | Is domain explicit? Are domain concerns addressed? | Are you applying wrong domain's heuristics? |

### Interaction Framing

Use framing techniques to establish what kind of interaction this is.

| Technique | When | Why | How | Output/Artifact | Positive Validation | Negative Validation |
|-----------|------|-----|-----|-----------------|---------------------|---------------------|
| **Activity Typing** | Interaction type affects appropriate behavior | To set expectations for what will happen | Name the activity (debugging, designing, reviewing, brainstorming, implementing), identify activity-appropriate behaviors, exclude inappropriate ones | Interaction Frame | Is activity type explicit? Do behaviors match? | Are you debugging when you should be designing? |
| **Goal Framing** | Purpose of interaction needs clarity | To align effort with intended outcome | State what we're trying to achieve, distinguish exploration from execution, identify success conditions for the interaction | Interaction Frame | Is goal explicit? Is exploration vs execution clear? | Is purpose unclear or shifting? |
| **Constraint Framing** | Constraints shape valid options | To work within bounds productively | List constraints (time, resources, compatibility, scope), accept as given not obstacles, find solutions within bounds | Interaction Frame | Are constraints explicit and accepted? | Are you fighting constraints rather than accepting? |
| **Scope Framing** | Work boundaries need definition | To focus effort appropriately | Define what's in scope, define what's explicitly out, resist scope creep by referencing frame | Interaction Frame | Are boundaries explicit? Is scope creep prevented? | Is scope undefined or expanding? |

### Stance Setting

Use stance techniques to establish how you will engage in the interaction.

| Technique | When | Why | How | Output/Artifact | Positive Validation | Negative Validation |
|-----------|------|-----|-----|-----------------|---------------------|---------------------|
| **Footing Selection** | Relationship to other party matters | To adopt appropriate relational position | Choose footing: authoritative (I know, you follow), collaborative (we figure out together), advisory (I suggest, you decide), instructive (I teach, you learn) | Stance Specification | Is footing explicit and appropriate? | Is footing mismatched to situation? |
| **Mode Selection** | Engagement style matters | To match interaction dynamics | Choose mode: direct (conclusions first), exploratory (questions first), systematic (step-by-step), creative (divergent), critical (evaluative) | Stance Specification | Is mode explicit and maintained? | Are you defaulting to habitual mode? |
| **Confidence Calibration** | Certainty level affects how to engage | To match confidence to knowledge | Assess what you know vs assume, signal confidence appropriately, hedge where uncertain, assert where confident | Stance Specification | Is confidence calibrated to knowledge? | Are you over/under-confident? |
| **Frame Validation** | Current frame may not fit | To check and adjust as needed | Ask "Is this frame helping?", identify mismatch signals (stuck, wrong results, misalignment), consider alternatives, reframe if needed | — | Is frame periodically validated? | Are you stuck in wrong frame? |

---

## Example Patterns

Structured example sequences for frame of mind in different contexts. Far more are possible.

### Establishing Frame for Task

Use when starting work that requires explicit framing.

1. Determine identity: who am I for this task? (Role Adoption, Perspective Taking)
2. Define interaction: what are we doing? (Activity Typing, Goal Framing)
3. Set constraints and scope (Constraint Framing, Scope Framing)
4. Choose stance: how will I engage? (Footing Selection, Mode Selection)
5. Calibrate confidence (Confidence Calibration)
6. Begin work within established frame
7. Validate frame periodically (Frame Validation)

**Exit Conditions:**
- Cannot determine appropriate role → stop, clarify what's being asked
- Activity type unclear → stop, establish what kind of work this is
- Frame mismatch detected → stop, reframe before continuing

### Setting Frame for Subagent

Use when spawning a subagent that needs explicit frame.

1. Define subagent identity (Role Adoption, Expertise Level, Domain Grounding)
2. Specify interaction type (Activity Typing, Goal Framing)
3. Set constraints and scope (Constraint Framing, Scope Framing)
4. Choose appropriate stance (Footing Selection, Mode Selection)
5. Document frame explicitly in subagent prompt
6. Validate subagent output matches intended frame

**Exit Conditions:**
- Role unclear → stop, clarify purpose before spawning
- Multiple conflicting frames needed → stop, spawn separate subagents
- Frame too complex → stop, decompose task first

---

## References

**Frame analysis (Goffman):**
- [Frame Analysis - Wikipedia](https://en.wikipedia.org/wiki/Frame_analysis)
- [Framing in Discourse - Deborah Tannen](http://www.communicationcache.com/uploads/1/0/8/8/10887248/framing_in_discourse.pdf)
- [Framing Social Interaction - Routledge](https://www.routledge.com/Framing-Social-Interaction-Continuities-and-Cracks-in-Goffmans-Frame/Persson/p/book/9780367897246)

**Positioning theory:**
- [Positioning Theory - Springer](https://link.springer.com/chapter/10.1007/978-3-319-97337-1_2)
- [Identity and Interaction - Bucholtz & Hall](https://journals.sagepub.com/doi/10.1177/1461445605054407)
- [Positioning in Discourse Analysis - Wiley](https://onlinelibrary.wiley.com/doi/abs/10.1002/9781405198431.wbeal0919)

**Role prompting for AI:**
- [Role Prompting Guide - Learn Prompting](https://learnprompting.org/docs/advanced/zero_shot/role_prompting)
- [AI Agent Persona - The New Stack](https://thenewstack.io/how-to-define-an-ai-agent-persona-by-tweaking-llm-prompts/)
