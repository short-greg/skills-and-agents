# Mode-to-Protocol Consolidation Guideline

This guideline explains how to consolidate actions from modes into protocols, merging the mode layer into the protocol layer.

---

## Purpose

The original framework had three layers:
- **Protocols** — Techniques (how to think/do)
- **Modes** — Actions (what to do)
- **Workflows** — Orchestration (when to do)

This consolidation merges modes into protocols by adding a **Possible Actions** section to each protocol. The result is a simpler two-layer framework:
- **Protocols** — Techniques + Actions
- **Workflows** — Orchestration

---

## Steps

Follow these steps when consolidating.

### 1. Identify Protocol to Consolidate

Choose a protocol that has mode actions referencing it and has not been consolidated.

### 2. Find All Mode Actions Referencing the Protocol

Search modes for references to the protocol:

```
grep -r "protocols/[protocol_name].md" modes/
```

This gives you a list of actions that currently delegate to this protocol.

### 3. Analyze Each Action for Fit

For each action found, evaluate whether it actually belongs to this protocol:

| Category | Criteria | Action |
|----------|----------|--------|
| **Strong fit** | Action's purpose is core to the protocol's scope | Include in Possible Actions |
| **Partial fit** | Only part of the action relates to this protocol | Extract the relevant part only |
| **Misattributed** | Action doesn't actually fit the protocol's scope | Note for reassignment to correct protocol |
| **Weak fit** | Tangentially related but not core | Consider excluding or noting as edge case |

**Key question:** Does the action's purpose match the protocol's scope statement?

### 4. Consolidate Overlapping Actions

Multiple modes may have similar actions. Consolidate them:

- **Confirm Plan** (planning) + **Output and Confirm** (defining) + **Output and Confirm** (designing) → **Request Confirmation**

Look for:
- Same purpose with different names
- Same name with slight variations
- Actions that are subsets of broader actions

### 5. Identify Missing Actions

Find actions the protocol should have but no mode currently defines:

1. Check if protocol's scope statement lists activities not covered by existing actions
2. Check if Core Approaches have techniques without corresponding high-level actions
3. Search web for "[protocol topic] actions" or "[protocol topic] activities" to find common activities in this domain

### 6. Draft the Possible Actions Table

Format:

```markdown
## Possible Actions

Select or execute possible actions based on context; this list is not exhaustive. Actions describe what to do; use techniques from Core Approaches for how.

| Action | Goal | When | How |
|--------|------|------|-----|
| **[Action Name]** | To [what this achieves] | Use when [observable condition] | [Brief description] |
```

**Guidelines:**
- Goal: "To X" format
- When: "Use when X" with observable triggers
- How: Brief, not exhaustive (techniques provide details)
- Actions should map to Core Approaches (not duplicate them)

### 7. Filter and Refine the Possible Actions Table

Iterate until you have a solid list of actions:

1. Remove actions that duplicate Core Approaches techniques (actions use techniques, not restate them)
2. Remove actions that don't match the protocol's scope
3. Merge actions that serve the same purpose
4. Verify each action has a distinct goal and clear trigger
5. Confirm 5-12 actions total (fewer than 5 suggests incomplete coverage; more than 12 suggests over-granularity)

### 8. Identify Project-Level Risks

For each action, ask: "What goes wrong at the project level when this is misused?"

1. List misuse scenarios for each action (over-application, under-application, wrong context)
2. Trace each misuse to its project-level consequence (not just the immediate communication failure)
3. Categorize consequences:
   - Trust and relationship damage
   - Decision quality degradation
   - Scope and timeline impacts
   - Compounding errors over time
   - Engagement and communication breakdown
4. Draft mitigation strategy for each risk (actionable, not vague advice)

### 9. Filter and Refine the Risks Table

Iterate until you have a solid list of risks:

1. Merge risks with the same root cause
2. Remove risks that are too generic ("communication fails") — be specific to this protocol
3. Verify each risk has an observable trigger in the "When" column
4. Verify each mitigation is actionable (not "be careful" but "do X instead of Y")
5. Confirm 5-10 risks total (fewer than 5 suggests incomplete analysis; more than 10 suggests overlap). If it exceeds it and there is no overlap. Consider to make a recommendation to divide into two protocols.

**Format:**

```markdown
## Risks

These are risks that misuse of this protocol poses to a project with mitigation strategies.

| Risk | When | Mitigation Strategy |
|------|------|---------------------|
| **[Risk Name]** | [Misuse condition] | [How to prevent/mitigate] |
```

### 10. Get approval

Share the proposed update with the user and get confirmation.

### 11. Update the Protocol

Add the Possible Actions and Risks sections to the protocol, before References.

### 12. Update Protocol References in Modes (Optional)

If modes will continue to exist, update them to reference the protocol's actions rather than defining their own.

---

## Example: Pragmatics Consolidation

**Step 2 - Found 14 actions referencing pragmatics.md**

**Step 3 - Analysis:**
- Strong fit: Calibrate Style, Request Confirmation, Apply Position (4 actions)
- Partial fit: Recommend Actions, Report, Organize Ideas (6 actions)
- Misattributed: Confirm and Document, Gather Context (2 actions — should be elicitation)
- Weak fit: Describe Each Idea, Summarize State (2 actions)

**Step 4 - Consolidations:**
- Confirm Plan + Output and Confirm → Request Confirmation
- Adopt Style and Calibrate + style parts of other actions → Calibrate Style

**Step 5 - Missing actions identified:**
- Persuade (from scope: "suggesting, recommending")
- Negotiate Agreement (from research on pragmatics)
- Acknowledge and Redirect (common AI assistant need)
- Set Expectations (common AI assistant need)

**Step 6/7 - Final Possible Actions (10 after filtering):**
Calibrate Style, Present Options, Make Recommendation, Signal Confidence, Frame Message, Request Confirmation, Negotiate Agreement, Acknowledge and Redirect, Set Expectations, Persuade

**Step 8/9 - Risks identified (8 after filtering):**
Trust erosion, Decision delay, Wrong direction pursued, Scope drift, Compounding misalignment, User disengagement, False agreement, Overcommitment

---

## Validation

After consolidation, verify:

- [ ] All strong-fit actions from modes are represented
- [ ] Partial-fit actions have their relevant parts extracted
- [ ] Misattributed actions are noted for reassignment
- [ ] Missing actions from scope/research are included
- [ ] Risks focus on project-level consequences
- [ ] Actions map to (not duplicate) Core Approaches
- [ ] Protocol stays within line limits or has justification

---

## Notes

- This is a temporary guideline for the consolidation effort
- Once all protocols are consolidated, this guideline can be archived
- The mode files may be retained, deprecated, or deleted based on framework direction
