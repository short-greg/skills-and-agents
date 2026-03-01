# Instruction Giving

Systematic techniques for writing instructions that enable successful task completion.

Write instructions that succeed on first attempt by managing clarity, structure, and cognitive load.

---

## Outline

- [Goal](#goal) | [Intent](#intent) | [Scope](#scope)
- [Artifacts and Outputs](#artifacts-and-outputs) — Procedural Instructions, Prerequisites List, Verification Checklist, Warning/Notice
- [Core Approaches](#core-approaches) — Clarity Techniques, Structure Techniques, Load Management
- [Example Patterns](#example-patterns) — Writing Procedural Instructions, Simplifying Complex Instructions

---

## Goal

Produce instructions that enable successful task completion on first attempt.

---

## Intent

Poorly designed instructions fail for predictable reasons: ambiguous language, missing prerequisites, poor sequencing, or cognitive overload. Cognitive Load Theory shows that working memory holds 5-9 items; exceeding this causes failure. Instructions succeed when they are unambiguous, well-structured, and matched to task complexity.

---

## Scope

Techniques for writing clear instructions, structuring procedural content, managing cognitive load, and verifying instruction effectiveness. Applies to skill definitions, workflow steps, setup procedures, and any task requiring step-by-step guidance.

---

## Artifacts and Outputs

| Artifact/Output | Purpose | When to Use | Artifact? |
|-----------------|---------|-------------|-----------|
| **Procedural Instructions** | Complete step-by-step guide for task completion | When documenting repeatable processes | Either |
| **Prerequisites List** | Requirements that must be met before starting | When task has dependencies or setup requirements | Output |
| **Verification Checklist** | Expected outcomes to confirm at checkpoints | When complex steps need confirmation | Output |
| **Warning/Notice** | Formatted caution before risky or destructive steps | When steps have potential for error or harm | Output |

---

## Core Approaches

### Clarity Techniques

Use clarity techniques to eliminate ambiguity and ensure single interpretation.

| Technique | When | Why | How | Output/Artifact | Positive Validation | Negative Validation |
|-----------|------|-----|-----|-----------------|---------------------|---------------------|
| **Explicit Language** | Instructions use vague, hedging, or passive words | To eliminate interpretation variance | Use imperative mood (commands not suggestions), replace "should/could/might" with "must/will", use definite articles ("the file" not "a file"), specify exact values not ranges | — | Are all statements imperative and definite? Is there only one interpretation? | Do instructions contain "should", "could", "might", passive voice, or vague quantifiers? |
| **Assumption Surfacing** | Reader may lack context you assume | To prevent failure from unstated prerequisites | List what reader must know, state environment requirements, make implicit knowledge explicit, document "obvious" steps | Prerequisites List | Are all assumptions stated? Would a newcomer succeed? | Are you assuming knowledge the reader may lack? |
| **Concrete Examples** | Abstract instructions are hard to follow | To ground instructions in specific instances | Provide input/output examples, show exact commands with real values, include sample data, demonstrate edge cases | — | Are examples concrete and complete? Do they match instructions? | Are instructions abstract without grounding examples? |
| **Action Verbs** | Steps describe states rather than actions | To make each step executable | Start each step with imperative verb (Run, Create, Open, Configure), specify the object of action, include parameters | — | Does each step start with a verb? Is the action clear? | Do steps describe states ("The file should be...") rather than actions? |
| **Conciseness** | Instructions contain unnecessary words or details | To reduce reading time and cognitive load | Remove filler words (actually, basically, simply), eliminate redundant phrases, use single words over phrases ("use" not "make use of"), cut to minimum words needed | — | Is every word necessary? Can anything be cut without losing meaning? | Are there filler words, redundant phrases, or verbose constructions? |
| **Accuracy** | Instructions may contain errors or outdated information | To prevent failure from incorrect instructions | Verify commands work as written, test with actual values, check version compatibility, confirm paths and names exist | — | Have instructions been tested? Are all commands and values correct? | Are commands assumed correct without testing? Are versions or paths outdated? |

### Structure Techniques

Use structure techniques to organize instructions for successful execution.

| Technique | When | Why | How | Output/Artifact | Positive Validation | Negative Validation |
|-----------|------|-----|-----|-----------------|---------------------|---------------------|
| **Prerequisite Gating** | Task has requirements that must be met first | To prevent failure from unmet dependencies | State prerequisites before instructions, use "Stop if X is not true" gates, group by type (tools, access, state) | Prerequisites List | Are prerequisites at the top? Are gates explicit? | Are prerequisites buried in later steps? |
| **Step Sequencing** | Multiple steps must occur in order | To ensure correct execution order | Number steps explicitly, use transition words (first, then, next, finally), one action per step, start new line for each step | Procedural Instructions | Is order unambiguous? Is each step atomic? | Could steps be executed out of order? Are multiple actions in one step? |
| **Conditional Branching** | Different paths exist based on conditions | To handle variations without confusion | Branch at decision point ("If X: do A. If not X: do B"), keep branches parallel in structure, rejoin after divergence | Procedural Instructions | Are conditions checked before branching? Are all paths covered? | Are conditions checked mid-step? Are some paths missing? |
| **Cognitive Chunking** | Many steps overwhelm working memory | To group related steps into manageable units | Group by cognitive boundary (where one mental model ends), use 5-9 items per chunk, add headers for each chunk, allow pause between chunks | Procedural Instructions | Are chunks coherent units? Is each chunk 5-9 items? | Are unrelated steps grouped? Are chunks too large? |

### Load Management

Use load management techniques to match instruction complexity to task difficulty.

| Technique | When | Why | How | Output/Artifact | Positive Validation | Negative Validation |
|-----------|------|-----|-----|-----------------|---------------------|---------------------|
| **Load-Based Adaptation** | Task complexity varies | To prevent overload on complex tasks | Assess intrinsic load (count interdependencies), minimize presentation complexity for high-load tasks, add context/rationale only for low-load tasks | — | Is presentation complexity inverse to task complexity? | Are complex tasks presented with complex formatting? |
| **Verification Points** | Complex steps need confirmation | To enable self-correction without re-reading | State expected outcome after complex steps ("Output should show X"), include visual confirmation where possible, specify what success looks like | Verification Checklist | Are verification points after complex steps? Is expected outcome specific? | Are complex steps missing verification? Is expected outcome vague? |
| **Notice Placement** | Warnings or cautions are needed | To prevent errors before they occur | Place notices before relevant step (never after), use hierarchy: Note < Warning < Caution < Danger, make consequence explicit | Warning/Notice | Are notices before the step? Is severity appropriate? | Are warnings after the step? Is severity unclear? |
| **Redundancy Reduction** | Information is repeated unnecessarily | To reduce extraneous cognitive load | State information once at point of use, reference rather than repeat, remove decorative content, eliminate synonymous terms | — | Is each piece of information stated once? Are references clear? | Is same information repeated? Are there unnecessary decorations? |

---

## Example Patterns

Structured example sequences for writing instructions in different contexts. Far more are possible.

### Writing Procedural Instructions

Use when creating step-by-step procedures.

1. Identify prerequisites (Assumption Surfacing)
2. List prerequisites with explicit gates at top (Prerequisite Gating)
3. Break procedure into 5-9 step chunks (Cognitive Chunking)
4. Write each step with action verb and single action (Action Verbs, Step Sequencing)
5. Add verification points after complex steps (Verification Points)
6. Place warnings before relevant steps (Notice Placement)
7. Test with someone unfamiliar with task

**Exit Conditions:**
- Cannot identify prerequisites → stop, trace back from first step to find dependencies
- Steps exceed 9 per chunk → stop, find cognitive boundaries to split
- Test reader fails → stop, identify failure point and revise

### Simplifying Complex Instructions

Use when existing instructions are failing or confusing.

1. Assess intrinsic load (count interdependencies, specialized knowledge)
2. If high load: strip to essentials, remove rationale, minimize formatting
3. If low load: add context, explain why, include alternatives
4. Replace vague terms with explicit language (Explicit Language)
5. Add concrete examples for abstract steps (Concrete Examples)
6. Insert verification points (Verification Points)
7. Test with target audience

**Exit Conditions:**
- Cannot reduce complexity → stop, task may need prerequisite training
- Still failing after simplification → stop, check if prerequisites are actually met
- Conflicting feedback → stop, segment audience and write separate versions

---

## References

**Anthropic/Claude documentation:**
- [Be clear and direct - Claude API Docs](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/be-clear-and-direct)
- [Use examples - Claude API Docs](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/multishot-prompting)
- [Chain complex prompts - Claude API Docs](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/chain-prompts)

**Cognitive load theory:**
- [Cognitive Load Theory and Instructional Design - eLearning Industry](https://elearningindustry.com/cognitive-load-theory-and-instructional-design)
- [Cognitive Load Theory - MCW](https://www.mcw.edu/-/media/MCW/Education/Academic-Affairs/OEI/Faculty-Quick-Guides/Cognitive-Load-Theory.pdf)

**Technical writing:**
- [Chunking for Microcontent - Precision Content](https://www.precisioncontent.com/blog/top-3-simple-technical-writing-tips-for-microcontent-chunking-titling-and-lists/)
- [Technical Writing Best Practices - Documind](https://www.documind.chat/blog/technical-writing-best-practices)

**Procedural writing:**
- [How to Write Procedures - Instructional Solutions](https://www.instructionalsolutions.com/blog/procedure-writing)
- [Procedural Writing Elements - Literacy Ideas](https://literacyideas.com/procedural-texts/)

**Ambiguity reduction:**
- [Avoiding Ambiguity in Requirements - JAF Consulting](https://jafconsulting.com/avoiding-ambiguity-in-requirements-tips-and-tricks-for-precision-and-clarity/)
- [Making Assumptions Explicit - Enterprise Craftsmanship](https://enterprisecraftsmanship.com/posts/making-implicit-assumptions-explicit)
