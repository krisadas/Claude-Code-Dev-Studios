---
name: design-review
description: "Reviews a feature specification document for completeness, internal consistency, implementability, and adherence to project design standards. Run this before handing a spec to engineers."
argument-hint: "[path-to-feature-spec]"
user-invocable: true
---

When this skill is invoked:

1. **Read the target feature specification** in full.

2. **Read the master CLAUDE.md** to understand project context and standards.

3. **Read related feature documents** referenced or implied by the target spec
   (check `design/features/` for related features).

4. **Evaluate against the Feature Spec checklist**:
   - [ ] Has Overview section (one-paragraph summary)
   - [ ] Has User Story section (who uses this and why)
   - [ ] Has Detailed Requirements section (unambiguous specifications)
   - [ ] Has Data Models section (key entities and relationships)
   - [ ] Has Edge Cases section (unusual situations handled)
   - [ ] Has Dependencies section (other systems/features listed)
   - [ ] Has Configuration section (tunable values identified)
   - [ ] Has Acceptance Criteria section (testable success conditions)

5. **Check for internal consistency**:
   - Do the requirements produce the behavior described in the user story?
   - Do edge cases contradict the main requirements?
   - Are dependencies bidirectional (does the other feature know about this one)?

6. **Check for implementability**:
   - Are the requirements precise enough for an engineer to implement without guessing?
   - Are there any "hand-wave" sections where details are missing?
   - Are API contracts defined?
   - Are performance implications considered?

7. **Check for cross-feature consistency**:
   - Does this conflict with any existing feature?
   - Does this create unintended interactions with other features?
   - Is this consistent with the product's established patterns and pillars?

8. **Output the review** in this format:

```
## Design Review: [Document Title]

### Completeness: [X/8 sections present]
[List missing sections]

### Consistency Issues
[List any internal or cross-feature contradictions]

### Implementability Concerns
[List any vague or unimplementable sections]

### API / Integration Concerns
[List any undefined contracts or integration gaps]

### Recommendations
[Prioritized list of improvements]

### Verdict: [APPROVED / NEEDS REVISION / MAJOR REVISION NEEDED]
```

9. **Contextual next step recommendations**:
   - If verdict is APPROVED: suggest "This spec is ready for implementation. Assign to an engineer or use `/sprint-plan` to schedule the work."
   - If verdict is NEEDS REVISION: suggest specific sections to improve, then re-run `/design-review`.
   - If verdict is MAJOR REVISION NEEDED: suggest returning to `/brainstorm` or `/design-system` for the affected sections.
