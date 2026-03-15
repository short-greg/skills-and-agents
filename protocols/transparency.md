# Transparency

Systematic techniques for making work visible, understandable, and traceable.

Make the implicit explicit through documentation, status communication, and decision rationale.

---

## Outline

- [Goal](#goal) | [Intent](#intent) | [Scope](#scope)
- [Artifacts and Outputs](#artifacts-and-outputs) — Package Documentation, Architecture Decision Record, Status Report, Completion Summary
- [Core Approaches](#core-approaches) — Artifact Transparency, Progress Transparency, Decision Transparency
- [Example Patterns](#example-patterns) — Documenting a Software Package, Completing a Task

---

## Goal

Ensure work is visible, decisions are explainable, and progress is trackable for both humans and AI.

---

## Intent

Without transparency, knowledge stays implicit, decisions seem arbitrary, and progress is invisible. When team members change, context is lost. When AI assists, it lacks necessary background. Transparency makes the implicit explicit—enabling understanding, trust, and effective collaboration.

---

## Scope

Techniques for documenting artifacts at all levels, communicating progress and status, and recording decision rationale. Applies to any work that others need to understand, continue, or review.

---

## Artifacts and Outputs

| Artifact/Output | Purpose | When to Use | Artifact? |
|-----------------|---------|-------------|-----------|
| **Package Documentation** | CLAUDE.md/README documenting purpose and boundaries | When creating or modifying a package/module | Either |
| **Architecture Decision Record** | Formal decision documentation with context and rationale | When making significant architectural choices | Yes |
| **Status Report** | Current progress with done/in-progress/blocked | When work is ongoing and stakeholders need visibility | Output |
| **Completion Summary** | What was delivered with changes and caveats | When finishing work for handoff or review | Output |

---

## Core Approaches

### Artifact Transparency

Use artifact transparency techniques to make created things understandable.

| Technique | When | Why | How | Output/Artifact | Positive Validation | Negative Validation |
|-----------|------|-----|-----|-----------------|---------------------|---------------------|
| **Hierarchical Documentation** | Artifact has multiple levels of detail | To enable understanding at appropriate depth | Document at each level: system (architecture), package (CLAUDE.md/README), module, class, function; link between levels | Package Documentation | Can reader navigate from overview to detail? Is each level documented? | Must reader understand everything to understand anything? |
| **Inline Documentation** | Code or artifact needs explanation | To explain intent, not just what | Add docstrings to public APIs (parameters, returns, behavior), comment complex logic and edge cases, explain "why" not "what" | — | Are public interfaces documented? Is complex logic explained? | Are obvious things commented? Is "why" missing? |
| **Package Documentation** | Creating or modifying a package/module | To establish boundaries and usage | Create CLAUDE.md/README with: purpose, what it does/doesn't do, dependencies, contents, entry points, examples | Package Documentation | Can someone understand the package without reading all code? | Is structure only discoverable by reading implementation? |
| **Synchronization** | Code or artifact changes | To prevent documentation drift | Update docs in same commit as changes, treat docs as code (versioned, reviewed), delete outdated docs rather than leave stale | — | Are docs updated with changes? Is there one source of truth? | Do docs and reality diverge? Are facts duplicated? |

### Progress Transparency

Use progress transparency techniques to make current state visible.

| Technique | When | Why | How | Output/Artifact | Positive Validation | Negative Validation |
|-----------|------|-----|-----|-----------------|---------------------|---------------------|
| **Status Reporting** | Work is ongoing | To keep stakeholders informed | Report what's done, what's in progress, what's blocked; use consistent format; include evidence not just claims | Status Report | Is current status clear? Are blockers visible? | Is progress invisible until completion? |
| **Completion Summary** | Work is finished | To enable handoff and review | Summarize what was delivered, list changes made, note any caveats or known issues, provide before/after if applicable | Completion Summary | Is completion unambiguous? Are caveats documented? | Is it unclear what was done? Are issues hidden? |
| **Change Notification** | Modifications affect others | To prevent surprises | Announce what changed, why it changed, who's affected, what action is needed; notify before or immediately after | — | Are affected parties informed? Is impact clear? | Do changes surprise people? |
| **Bug/Issue Reporting** | Problems are discovered | To enable diagnosis and tracking | Document symptoms, reproduction steps, expected vs actual behavior, context (environment, versions), severity assessment | — | Can someone reproduce the issue? Is context sufficient? | Is report vague ("it's broken")? Is context missing? |

### Decision Transparency

Use decision transparency techniques to make choices explainable and traceable.

| Technique | When | Why | How | Output/Artifact | Positive Validation | Negative Validation |
|-----------|------|-----|-----|-----------------|---------------------|---------------------|
| **Decision Recording** | Making significant choices | To preserve context for future | Document context/problem, options considered, decision made, rationale for choice; store accessibly (ADR, wiki, code comment) | Architecture Decision Record | Is decision documented? Is rationale clear? | Are decisions implicit? Will rationale be lost? |
| **Tradeoff Explanation** | Decision involves competing concerns | To make sacrifices explicit | State what you're optimizing, what you're sacrificing, why this balance is appropriate for context | — | Are tradeoffs explicit? Is the balance justified? | Are tradeoffs hidden? Will someone ask "why not X"? |
| **Assumption Documentation** | Proceeding based on assumptions | To enable validation and revision | State assumptions explicitly, document basis for each, flag for review when conditions change | — | Are assumptions explicit? Is basis documented? | Are assumptions implicit? Will they be forgotten? |
| **Constraint Acknowledgment** | External factors limit options | To explain why alternatives weren't chosen | Document constraints (time, resources, compatibility, requirements), explain how they shaped the decision | — | Are constraints documented? Is their impact clear? | Do decisions seem arbitrary because constraints are hidden? |

---

## Example Patterns

Structured example sequences for transparency in different contexts. Far more are possible.

### Documenting a Software Package

Use when creating or significantly modifying a package/module.

1. Write one-sentence purpose statement
2. Document what it does AND doesn't do (boundaries)
3. List dependencies (internal and external) with rationale
4. Describe contents (files, subpackages, key classes)
5. Identify entry points and public API
6. Add usage examples for common cases
7. Document any conventions or patterns used
8. Update in same commit as code changes

**Exit Conditions:**
- Purpose unclear → stop, clarify with stakeholders
- Boundaries overlap with other packages → stop, resolve ownership
- Can't explain without reading all code → stop, simplify or restructure

### Completing a Task

Use when finishing work that others need to review or continue.

1. Summarize what was done (Completion Summary)
2. List specific changes made (files, functions, behaviors)
3. Document any decisions made and rationale (Decision Recording)
4. Note assumptions made (Assumption Documentation)
5. Report any known issues or caveats
6. Update affected documentation (Synchronization)
7. Notify affected parties (Change Notification)

**Exit Conditions:**
- Can't summarize clearly → stop, work may be incomplete or unfocused
- Decisions undocumented → stop, record rationale before it's forgotten
- Documentation not updated → stop, sync before declaring complete

---

## Possible Actions

Select or execute possible actions based on context; this list is not exhaustive. Actions describe what to do; use techniques from Core Approaches for how.

| Action | Goal | When | How |
|--------|------|------|-----|
| **Document Work Product** | To make created artifacts understandable | Use when producing artifacts others need to understand | Apply Hierarchical, Inline, or Package Documentation based on artifact type |
| **Synchronize Documentation** | To prevent documentation drift | Use when code or artifacts change | Update docs in same change as implementation, delete outdated content |
| **Report Status** | To keep stakeholders informed of progress | Use when work is ongoing and visibility needed | Report done/in-progress/blocked with evidence, use consistent format |
| **Summarize Completion** | To enable handoff and review | Use when finishing work | Summarize deliverables, list changes, note caveats and known issues |
| **Notify Stakeholders** | To prevent surprises from changes | Use when modifications affect others | Announce what changed, why, who's affected, what action needed |
| **Report Issue** | To enable diagnosis and tracking | Use when problems discovered | Document symptoms, reproduction steps, expected vs actual, context |
| **Record Decision** | To preserve context for future | Use when making significant choices | Document context, options, decision, rationale; store accessibly |
| **Explain Tradeoffs** | To make sacrifices explicit | Use when decision involves competing concerns | State what's optimized, what's sacrificed, why balance is appropriate |
| **Document Assumptions** | To enable validation and revision | Use when proceeding based on assumptions | State assumptions explicitly, document basis, flag for review |
| **Acknowledge Constraints** | To explain why alternatives weren't chosen | Use when external factors limit options | Document constraints and how they shaped the decision |

---

## Risks

These are risks that misuse of this protocol poses to a project with mitigation strategies.

| Risk | When | Mitigation Strategy |
|------|------|---------------------|
| **Documentation Drift** | Docs updated separately from code changes | Update docs in same commit/PR as code changes; treat stale docs as bugs |
| **Knowledge Loss** | Critical decisions/context not captured before team changes | Record decisions at decision time, not retrospectively; document "why" not just "what" |
| **Surprise Discoveries** | Changes made without stakeholder notification | Identify affected parties before making changes; notify before or immediately after |
| **Decision Amnesia** | Rationale discussed but not recorded | Record decisions when made, not when convenient; use lightweight ADR format |
| **Hidden Assumptions** | Proceeding without stating assumptions explicitly | State assumptions in writing; flag for review when context changes |
| **False Progress Signals** | Status reports inaccurate or omit blockers | Report based on evidence, not optimism; make blockers visible immediately |
| **Information Overload** | Every detail documented with equal weight | Document at appropriate level; summarize for stakeholders, detail for implementers |
| **Incomplete Handoffs** | Work declared complete without summary | Always produce completion summary with changes, caveats, and known issues |

---

## References

- [Documentation Best Practices - Google](https://google.github.io/styleguide/docguide/best_practices.html)
- [Documenting Python Code - Real Python](https://realpython.com/documenting-python-code/)
- [Code Documentation Best Practices - AltexSoft](https://www.altexsoft.com/blog/how-to-write-code-documentation/)
- [Architecture Decision Records - GitHub](https://github.com/joelparkerhenderson/architecture-decision-record)
- [Project Status Reports - Atlassian](https://www.atlassian.com/agile/project-management/status-report)
- [Diátaxis Documentation Framework](https://diataxis.fr/)
