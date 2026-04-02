---
name: project-stage-detect
description: "Automatically analyze project state, detect stage, identify gaps, and recommend next steps based on existing artifacts."
argument-hint: "[optional: role filter like 'engineer' or 'designer']"
user-invocable: true
---

# Project Stage Detection

This skill scans your project to determine its current development stage, completeness
of artifacts, and gaps that need attention. Useful when:
- Starting with an existing project
- Onboarding a new contributor
- Checking what's missing before a milestone
- Understanding "where are we?"

---

## Workflow

### 1. Scan Key Directories

**Design Documentation** (`design/`):
- Check for `design/product-concept.md`, `design/product-pillars.md`
- Count feature specs in `design/features/*.md`
- Analyze completeness (Overview, User Story, Requirements, etc.)

**Source Code** (`src/`):
- Count source files (language-agnostic)
- Identify major modules/services (directories with 5+ files)
- Estimate lines of code (rough scale)

**Production Artifacts** (`production/`):
- Check for active sprint plans
- Look for milestone definitions

**Prototypes** (`prototypes/`):
- Count prototype directories
- Check for READMEs

**Architecture Docs** (`docs/architecture/`):
- Count ADRs (Architecture Decision Records)

**Tests** (`tests/`):
- Count test files
- Estimate test coverage (rough heuristic)

### 2. Classify Project Stage

Check `production/stage.txt` first — if it exists, use its value (explicit override
from `/gate-check`). Otherwise, auto-detect using these heuristics:

| Stage | Indicators |
|-------|-----------|
| **Concept** | No product concept doc, ideation phase |
| **Design** | Product concept exists, feature specs missing or incomplete |
| **Technical Setup** | Feature specs exist, tech stack not configured |
| **Development** | Stack configured, `src/` has active code |
| **Staging** | Explicit only (set by `/gate-check` Development → Staging gate) |
| **Release** | Explicit only (set by `/gate-check` Staging → Release gate) |

### 3. Collaborative Gap Identification

**DO NOT** just list missing files. Instead, **ask clarifying questions**:

- "I see API code (`src/api/`) but no `design/features/` spec for it. Was this prototyped first, or should we reverse-document?"
- "You have 10 ADRs but no architecture overview. Should I create one to help new contributors?"
- "No sprint plans in `production/`. Are you tracking work elsewhere (Jira, Linear, etc.)?"
- "I found a product concept but no feature specs. Have you broken down the product into individual features yet, or should we run `/design-system`?"

### 4. Generate Stage Report

**Report structure**:
```markdown
# Project Stage Analysis

**Date**: [date]
**Stage**: [Concept/Design/Technical Setup/Development/Staging/Release]

## Completeness Overview
- Design: [X%] ([N] specs, [gaps])
- Code: [X%] ([N] files, [modules])
- Architecture: [X%] ([N] ADRs, [gaps])
- Production: [X%] ([status])
- Tests: [X%] ([coverage estimate])

## Gaps Identified
1. [Gap description + clarifying question]
2. [Gap description + clarifying question]

## Recommended Next Steps
[Priority-ordered list based on stage and role]
```

### 5. Role-Filtered Recommendations (Optional)

If user provided a role argument (e.g., `/project-stage-detect engineer`):

**Engineer**:
- Focus on architecture docs, test coverage, missing ADRs
- Code-to-spec gaps

**Designer / PM**:
- Focus on feature spec completeness, missing design sections
- Sprint planning artifacts

**General** (no role):
- Holistic view of all gaps
- Highest-priority items across domains

### 6. Request Approval Before Writing

```
I've analyzed your project. Here's what I found:

[Show summary]

Gaps identified:
1. [Gap 1 + question]
2. [Gap 2 + question]

Recommended next steps:
- [Priority 1]
- [Priority 2]

May I write the full stage analysis to production/project-stage-report.md?
```

Wait for user approval before creating the file.

---

## Follow-Up Actions

- **No product concept?** → `/brainstorm`
- **Missing feature specs?** → `/design-system <feature-name>`
- **Missing architecture docs?** → `/architecture-decision` or `/reverse-document architecture`
- **No sprint plan?** → `/sprint-plan`
- **Approaching milestone?** → `/milestone-review`
- **Ready to advance stage?** → `/gate-check`
