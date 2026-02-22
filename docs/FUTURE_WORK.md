# Future Work

Ideas and plans for future development. Currently testing the framework for personal use.

---

## Foundational Problems

Three fundamental hurdles limit what LLMs can achieve in producing maintainable code. These are largely unsolved and represent the deeper challenges this framework attempts to address.

### 1. Constraints on Flow

**Problem:** LLMs don't reliably follow multi-step processes.

**Impact:** Skipped steps, incomplete implementations, inconsistent patterns.

**Current approach:** Phases, checklists, gates, progress tracking.

**Status:** Relatively strong - but still relies on self-enforcement.

### 2. Assessment on Output

**Problem:** LLMs can't rigorously evaluate their own work.

**Impact:** Accepts bad code as "good enough", misses issues, false confidence.

**Current approach:** OKRs, tool-based metrics (radon, pylint), checklists.

**Status:** Weak - AI self-reports, no external enforcement.

**The deeper issue:** Many qualities (modularity, naming, abstraction) are partially measurable but ultimately subjective. Metrics tell you *how much* coupling exists, not *whether it's the right* coupling. What should be coupled and what level is acceptable depends on context, domain, and intent.

**Open questions:**
- How to set up rigorous evaluation of subjective qualities?
- How to determine acceptable thresholds per context?
- How to avoid "vibes-based" self-assessment?

### 3. Representation

**Problem:** LLMs don't maintain coherent models of the system they're working on.

**Impact:** Forgets context, duplicates functionality, violates existing patterns, creates drift.

**Current approach:** Documentation maintenance pattern ("read before, update after").

**Status:** Weak - no verification that representation is accurate or complete.

**Open questions:**
- How to maintain effective representations of the system?
- How to verify the representation matches reality?
- How to prevent drift over time?

---

## Testing Approach

**Current:** Use as submodule or symbolic link in real projects to validate the framework.

**Goal:** Identify friction points, gaps, and modularity issues through actual usage.

---

## Open Questions

### Modularity

1. **Should primitives be invokable standalone?**
   - Currently primitives (orient, define, design, implement, validate, etc.) are composed by workflows
   - Could someone invoke just `define` without a full workflow?
   - Primitives are generic; workflows provide context

2. **Should there be additional "building block" primitives?**
   - Current set: orient, define, design, implement, validate, investigate, brainstorm, critique
   - Potential additions: `interview`, `assess`, `synthesize`
   - Composed into workflows as needed

3. **Should workflows be customizable at runtime?**
   - Let users skip steps they don't need
   - Provide sensible defaults but allow step omission
   - Current: repo-type customization (Prototype/Production/Library)

4. **What level of modularity matters most?**
   - Unknown - will discover through testing

### Naming

- Primitives are generic and reusable (`brainstorm`, not `bugfix-brainstorm`)
- Workflows are domain-specific (`bugfix_workflow`, `feature_workflow`)
- This separation is intentional but may need refinement

---

## Distribution (Later)

### Package Name Ideas
- `diviner` (taken but fits conceptually - "divining" what user needs)
- `@org/diviner`, `skill-diviner`, `divinerkit`
- Other ideas TBD

### Slash Command Prefix
- Current favorite: `divine-` → `/divine-skill`, `/divine-agent`
- Alternatives: `forge-`, `mint-`, `spark-`

### Plugin Structure (Planned)

```
plugin/
├── skills/
│   ├── <prefix>-setup/SKILL.md
│   └── <prefix>-workflow/SKILL.md
│
├── primitives/                # Bundled - core cognitive actions
├── workflows/                 # Bundled - multi-step processes
├── protocols/                 # Bundled - reusable patterns
└── templates/                 # Bundled - skill templates
```

### Commands (Planned)

| Command | Purpose |
|---------|---------|
| `/<prefix>-setup` | Initial project setup |
| `/<prefix>-workflow` | Create workflow from template |

### Distribution Channels (Research)

- **Claude Code Plugin** - native integration
- **OpenSkills** - universal installer (`npx openskills install`)
- **skild.sh** - marketplace
- **SkillKit** - cross-platform format translation

---

## Existing Ecosystem (Reference)

| Tool | Focus | Gap |
|------|-------|-----|
| skild.sh | Marketplace - find/install skills | No creation |
| SkillKit | Format translation between tools | No creation |
| openskills | Universal installer | No creation |
| Vercel Skills | Pre-built React/Next.js skills | No custom creation |

**Our focus:** Guided skill/agent authoring with proven patterns.

---

## Notes

- Framework is tool-agnostic (`${TOOL_CONFIG}` placeholders)
- Guidelines = pre-answered interview questions for common workflows
- Creating from guideline = shorter interview
- Creating from scratch = longer interview, uses templates

---

## Framework Perspective

Skills and agents can be viewed as a **programming paradigm**:
- Declarative: You specify what you want, not how to do it
- Pass in instructions + data, get outputs
- AI determines the "how"

This differs from most existing tools which focus on distribution/installation of pre-made skills rather than principled authoring.

The three foundational problems (flow, assessment, representation) are what prevent this paradigm from producing truly maintainable code at scale.
