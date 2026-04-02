---
name: design-system
description: "Guided, section-by-section feature specification authoring. Gathers context from existing docs, walks through each required section collaboratively, cross-references dependencies, and writes incrementally to file."
argument-hint: "<feature-name> (e.g., 'user-auth', 'dashboard', 'notifications')"
user-invocable: true
---

When this skill is invoked:

## 1. Parse Arguments & Validate

A feature name argument is **required**. If missing, fail with:
> "Usage: `/design-system <feature-name>` — e.g., `/design-system user-auth`
> Run `/brainstorm` first to establish the product concept, then use this skill
> to write individual feature specs."

Normalize the feature name to kebab-case for the filename.

---

## 2. Gather Context (Read Phase)

Read all relevant context **before** asking the user anything.

### 2a: Required Reads

- **Product concept**: Read `design/product-concept.md` if it exists. If missing, warn:
  > "No product concept found. Consider running `/brainstorm` first."
- **Existing feature spec**: Read `design/features/[feature-name].md` if it exists
  (resume, don't restart from scratch).

### 2b: Dependency Reads

Glob `design/features/*.md` and read any specs that:
- Are explicitly depended on by this feature
- Depend on this feature's output
- Share significant data models or APIs with this feature

For each related spec, extract:
- Key API contracts and interfaces
- Shared data models
- Edge cases that assume this feature's behavior

### 2c: Present Context Summary

Before starting, present a brief summary:

> **Designing: [Feature Name]**
> - Related features: [list, noting which have specs vs. unspecified]
> - Existing decisions to respect: [key constraints from related specs]

Use `AskUserQuestion`:
- "Ready to start designing [feature-name]?"
  - Options: "Yes, let's go", "Show me more context first"

---

## 3. Create File Skeleton

Once the user confirms, **immediately** create the spec file with empty section
headers. This ensures incremental writes have a target.

```markdown
# [Feature Name]

> **Status**: In Design
> **Author**: [user + agents]
> **Last Updated**: [today's date]

## Overview

[To be designed]

## User Story

[To be designed]

## Detailed Requirements

### Core Requirements

[To be designed]

### States and Transitions

[To be designed]

### Interactions with Other Features

[To be designed]

## Data Models

[To be designed]

## API Contract

[To be designed]

## Edge Cases

[To be designed]

## Dependencies

[To be designed]

## Configuration

[To be designed]

## Acceptance Criteria

[To be designed]

## Open Questions

[To be designed]
```

Ask: "May I create the skeleton file at `design/features/[feature-name].md`?"

After writing, update `production/session-state/active.md` with:
- Task: Designing [feature-name] spec
- Current section: Starting (skeleton created)
- File: design/features/[feature-name].md

---

## 4. Section-by-Section Design

Walk through each section in order. For **each section**, follow this cycle:

```
Context  →  Questions  →  Options  →  Decision  →  Draft  →  Approval  →  Write
```

1. **Context**: State what this section needs and surface constraints from related specs.
2. **Questions**: Ask clarifying questions. Use `AskUserQuestion` for constrained choices.
3. **Options**: Present 2-4 approaches with pros/cons where design choices exist.
4. **Decision**: User picks an approach or provides custom direction.
5. **Draft**: Write the section content in conversation for review.
6. **Approval**: Ask "Approve this section, or would you like changes?"
7. **Write**: Use Edit to replace `[To be designed]` with approved content. Confirm.

After writing each section, update `production/session-state/active.md`.

### Section Guidance

---

**Section A: Overview**
- One paragraph a newcomer could read and understand.
- What is this feature? Why does it exist? What does the user get from it?

---

**Section B: User Story**
- Format: "As a [user type], I want to [action], so that [benefit]."
- Include primary and secondary user stories if applicable.

---

**Section C: Detailed Requirements**
- Precise enough for an engineer to implement without ambiguity.
- Sub-sections: Core Requirements, States and Transitions, Interactions.
- Use numbered rules for sequential processes, bullets for properties.
- For state machines, map every state and every valid transition in a table.

---

**Section D: Data Models**
- Key entities, their fields, and relationships.
- Use a simple schema or table format.
- Note which fields are required, optional, or system-generated.

---

**Section E: API Contract**
- For each endpoint or interface:
  - Method, path, request schema, response schema, error codes.
- Can be deferred if backend/frontend are designed separately, but flag it.

---

**Section F: Edge Cases**
- Explicit handling of unusual situations.
- What happens at boundary values? With missing data? With concurrent requests?
- What should NOT be possible? Enforce constraints.

---

**Section G: Dependencies**
- Other features, services, or third-party APIs this feature requires.
- For each: what specifically is needed, and who owns it?

---

**Section H: Configuration**
- Designer/operator-adjustable values (feature flags, rate limits, timeouts).
- For each: safe range, default value, behavior at extremes.

---

**Section I: Acceptance Criteria**
- Testable conditions that prove the feature works.
- Format: "Given [context], when [action], then [result]."
- Include performance criteria (e.g., "API responds within 200ms p95").

---

## 5. Post-Design Validation

After all sections are written:

### 5a: Self-Check
Read back the complete spec from file. Verify:
- All required sections have real content
- API contracts are defined
- Edge cases have resolutions
- Acceptance criteria are testable

### 5b: Offer Design Review
Use `AskUserQuestion`:
- "Run `/design-review` now to validate the spec?"
  - Options: "Yes, run review now", "I'll review it myself first", "Skip review"

### 5c: Update Session State
Update `production/session-state/active.md` with:
- Task: [feature-name] spec
- Status: Complete (or In Review)
- File: design/features/[feature-name].md

### 5d: Suggest Next Steps
Use `AskUserQuestion`:
- "What's next?"
  - Options:
    - "Design another feature"
    - "Assign to sprint (`/sprint-plan`)"
    - "Run `/gate-check`"
    - "Stop here for this session"

---

## 6. Specialist Agent Routing

| Feature Category | Primary Agent | Supporting Agent(s) |
|-----------------|---------------|---------------------|
| API / backend features | `backend-engineer` | `security-engineer` (auth), `lead-programmer` (arch) |
| UI / frontend features | `frontend-engineer` | `ux-designer` (flows), `lead-programmer` (arch) |
| Auth / security | `security-engineer` | `backend-engineer` (impl) |
| Performance-critical | `performance-analyst` | `backend-engineer` or `frontend-engineer` |
| Networking / real-time | `network-programmer` | `backend-engineer` |

When delegating via Task tool:
- Provide: feature name, product concept summary, related spec excerpts, the specific section being worked on
- Agents return analysis/proposals; main session presents to user; user decides; main session writes to file

---

## 7. Recovery & Resume

If the session is interrupted:
1. Read `production/session-state/active.md`
2. Read `design/features/[feature-name].md` — sections with `[To be designed]` still need work
3. Resume from the next incomplete section

---

## Collaborative Protocol

1. **Question → Options → Decision → Draft → Approval** for every section
2. **AskUserQuestion** at every decision point
3. **"May I write to [filepath]?"** before skeleton and each section write
4. **Incremental writing**: Each section written to file immediately after approval
5. **Never** auto-generate the full spec as a fait accompli
6. **Never** write a section without user approval
