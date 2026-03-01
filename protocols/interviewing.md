# Interviewing

Systematic techniques for gathering requirements and clarifying user needs through effective questioning.

Ask the right questions to get actionable information without overwhelming the user.

---

## Goal

Gather sufficient information to proceed confidently while minimizing user burden.

---

## Intent

Without effective questioning, work proceeds on assumptions that may be wrong. Too many questions exhaust users; too few lead to rework. The cost of clarifying is always less than fixing bugs from wrong assumptions. This protocol ensures questions are well-designed, appropriately paced, and yield actionable information.

---

## Scope

Techniques for designing effective questions, gathering information through asking or inference, managing conversation flow, and resolving ambiguities. Applies to requirements gathering, scope clarification, preference elicitation, and constraint discovery.

---

## Core Approaches

### Question Design

Use question design techniques to craft questions that yield useful information.

| Technique | When | Why | How | Positive Validation | Negative Validation |
|-----------|------|-----|-----|---------------------|---------------------|
| **Open Questions** | Exploring unknown territory or understanding context | To encourage detailed responses revealing attitudes and needs | Ask what/how/why questions, avoid yes/no framing, leave room for unexpected information, use "Tell me about..." or "What would..." | Does question invite elaboration? Can it reveal unexpected information? | Can question be answered with single word? Does it lead the answer? |
| **Closed Questions** | Confirming specific details or making choices | To get definitive answers for decision points | Frame as yes/no or choice between options, use after open questions to confirm, limit options to 2-4 choices | Is answer unambiguous? Does it resolve a specific decision? | Is question used too early before exploration? Are there too many options? |
| **Probing Questions** | Initial answer is incomplete or unclear | To dig deeper into reasons, constraints, or implications | Ask follow-up "why" or "what if", explore edge cases mentioned, clarify vague terms used in response | Does probe reveal underlying reasons? Does it clarify ambiguity? | Is probing excessive or feeling like interrogation? |
| **Funnel Sequencing** | Complex topic needs structured exploration | To move from broad understanding to specific details | Start broad (open), narrow with probes, confirm with closed, allow return to broad if new topics emerge | Does sequence move from general to specific? Are transitions natural? | Is sequence rigid? Are you closing too quickly? |

### Information Gathering

Use information gathering techniques to obtain what you need efficiently.

| Technique | When | Why | How | Positive Validation | Negative Validation |
|-----------|------|-----|-----|---------------------|---------------------|
| **Inference First** | Information may be discoverable without asking | To reduce question burden and show competence | Analyze codebase for patterns, check existing documentation, examine similar implementations, infer from context before asking | Did you check discoverable sources first? Is inference well-grounded? | Are you asking what you could have discovered? Is inference a guess? |
| **Assumption Documentation** | Must proceed without confirmation | To make implicit assumptions explicit and reviewable | State assumption clearly, explain basis for assumption, flag for user review, design for easy correction if wrong | Are assumptions explicit? Is basis stated? Can user easily correct? | Are assumptions hidden? Are you acting on unverified assumptions? |
| **Success Definition** | Scope or completion criteria unclear | To establish clear end state before starting | Ask "What would success look like?", probe for must-haves vs nice-to-haves, identify how they'll evaluate completion | Is success concrete? Are priorities clear? | Is success vague ("it should work well")? Are all outcomes equal priority? |
| **Constraint Discovery** | Requirements may have unstated limits | To surface hidden constraints before they cause rejection | Ask about performance, compatibility, style, timeline, probe for "obvious" expectations, check for existing patterns to follow | Are constraints explicit? Have you checked for "obvious" ones? | Will user reject solution due to unstated constraint? |

### Conversation Management

Use conversation management techniques to maintain productive dialogue.

| Technique | When | Why | How | Positive Validation | Negative Validation |
|-----------|------|-----|-----|---------------------|---------------------|
| **Question Pacing** | Multiple questions needed | To avoid overwhelming user with too many questions | Ask 1-2 questions at a time, batch related questions, prioritize most critical questions first, defer less important questions | Are questions appropriately batched? Are critical questions first? | Are you asking too many questions at once? Are low-priority questions asked early? |
| **Contradiction Resolution** | User gives conflicting information | To surface and resolve inconsistencies before proceeding | Note contradiction without blame, present both statements neutrally, ask which takes priority, document resolution | Is contradiction surfaced respectfully? Is resolution documented? | Are contradictions ignored? Is user made to feel wrong? |
| **Understanding Validation** | Complex requirements have been gathered | To confirm understanding before acting | Paraphrase understanding back, summarize key points, ask for confirmation or correction, highlight any assumptions | Is understanding confirmed? Were assumptions flagged? | Are you proceeding without confirming? Is paraphrase accurate? |
| **Sufficient Information** | Deciding whether to ask more or proceed | To avoid both analysis paralysis and premature action | Assess: can you start meaningful work? Are unknowns low-risk? Can you course-correct if wrong? | Can you proceed confidently? Are remaining unknowns manageable? | Are you paralyzed seeking certainty? Are you proceeding with critical unknowns? |

---

## Example Patterns

Structured example sequences for interviewing in different contexts. Far more are possible.

### Requirements Clarification

Use when user request is ambiguous or incomplete.

1. Check if information is discoverable (Inference First)
2. If not, ask open question about goal or context (Open Questions)
3. Probe for constraints and preferences (Constraint Discovery)
4. Ask what success looks like (Success Definition)
5. Confirm understanding with paraphrase (Understanding Validation)
6. Document any remaining assumptions (Assumption Documentation)
7. Assess if ready to proceed (Sufficient Information)

**Exit Conditions:**
- Can infer everything needed → stop asking, proceed with documented assumptions
- User expresses fatigue → stop, document assumptions, offer to clarify later
- Critical information missing and can't proceed → must continue questioning

### Resolving Conflicting Statements

Use when user has given contradictory requirements.

1. Note both statements without judgment
2. Present contradiction neutrally ("Earlier you mentioned X, and now Y...")
3. Ask which takes priority or if both are needed (Closed Questions)
4. If both needed, probe for how to reconcile (Probing Questions)
5. Document resolution explicitly
6. Confirm resolved understanding (Understanding Validation)

**Exit Conditions:**
- User clarifies priority → proceed with documented priority
- Both requirements essential but conflicting → escalate or propose tradeoff options
- User unsure → document uncertainty, propose reversible approach

---

## References

**Anthropic/Claude documentation:**
- [Common workflows - Claude Code Docs](https://docs.anthropic.com/en/docs/claude-code/common-workflows)
- [Prompting best practices - Claude API Docs](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/claude-4-best-practices)
- [Be clear and direct - Claude API Docs](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/be-clear-and-direct)

**Requirements elicitation:**
- [Requirements Elicitation Questions - Bridging the Gap](https://www.bridging-the-gap.com/what-questions-do-i-ask-during-requirements-elicitation/)
- [Eliciting Requirements Techniques - Requiment](https://www.requiment.com/eliciting-requirements-techniques/)
- [Six Effective Elicitation Questions - LinkedIn](https://www.linkedin.com/pulse/six-effective-elicitation-questions-ask-your-angela)

**Question design:**
- [The Funnel Technique - NN/g](https://www.nngroup.com/articles/the-funnel-technique-in-qualitative-user-research/)
- [Effective Questioning - CPD Online](https://cpdonline.co.uk/knowledge-base/business/effective-questioning/)
- [How to Ask Probing Questions - Envato Tuts+](https://business.tutsplus.com/tutorials/how-to-ask-probing-questions--cms-41116)

**Asking vs assuming:**
- [Art of Asking Clarifying Questions - AlgoCademy](https://algocademy.com/blog/the-art-of-asking-clarifying-questions-a-key-skill-for-programmers/)
- [Perils of Presumption - Dev.to](https://dev.to/trev_the_dev/the-perils-of-presumption-why-making-assumptions-in-development-is-bad-34p4)

**Conflicting requirements:**
- [Managing Conflicting Expectations - PMI](https://www.pmi.org/learning/library/managing-conflicting-expectations-6893)
- [Requirements Conflict Management - Business Analysis Excellence](https://business-analysis-excellence.com/requirements-conflict-8-ways-manage-sticky-stakeholder/)
