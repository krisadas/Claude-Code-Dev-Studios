# Software Studio Agent Architecture -- Quick Start Guide

## What Is This?

This is a complete Claude Code agent architecture for software development. It
organizes 13 specialized AI agents into a studio hierarchy that mirrors real
software teams, with defined responsibilities, delegation rules, and coordination
protocols. The stack supports React, Next.js, Angular (frontend), Node.js,
Golang, and Python (backend).

## How to Use

### 1. Understand the Hierarchy

There are three tiers of agents:

- **Tier 1 (Opus)**: Leadership who make high-level decisions
  - `technical-director` -- architecture and technology decisions
  - `product-manager` -- roadmap, sprint planning, coordination, and risk

- **Tier 2 (Sonnet)**: Department leads who own their domain
  - `lead-programmer`, `qa-lead`, `release-manager`

- **Tier 3 (Sonnet/Haiku)**: Specialists who execute within their domain
  - `backend-engineer`, `frontend-engineer`, `network-programmer`,
    `devops-engineer`, `security-engineer`, `ux-designer`,
    `performance-analyst`, `qa-tester`

### 2. Pick the Right Agent for the Job

Ask yourself: "What role would handle this in a real software team?"

| I need to... | Use this agent |
|-------------|---------------|
| Design a new feature | `product-manager` |
| Write API endpoints | `backend-engineer` |
| Build UI components | `frontend-engineer` |
| Design user flows / wireframes | `ux-designer` |
| Plan the next sprint | `product-manager` |
| Review code quality | `lead-programmer` |
| Write test cases | `qa-tester` |
| Fix a performance problem | `performance-analyst` |
| Set up CI/CD | `devops-engineer` |
| Review code for security issues | `security-engineer` |
| Resolve an architecture conflict | `technical-director` |
| Manage a release | `release-manager` |
| Implement networking / API clients | `network-programmer` |

### 3. Use Slash Commands for Common Tasks

| Command | What it does |
|---------|-------------|
| `/start` | First-time onboarding — asks where you are, guides you to the right workflow |
| `/design-review` | Reviews a feature spec document |
| `/code-review` | Reviews code for quality and architecture |
| `/sprint-plan` | Creates or updates sprint plans |
| `/architecture-decision` | Creates an ADR |
| `/milestone-review` | Reviews milestone progress |
| `/onboard` | Generates onboarding docs for a role |
| `/prototype` | Scaffolds a throwaway prototype |
| `/release-checklist` | Validates pre-release checklist |
| `/changelog` | Generates changelog from git history |
| `/retrospective` | Runs sprint/milestone retrospective |
| `/estimate` | Produces structured effort estimates |
| `/hotfix` | Emergency fix with audit trail |
| `/tech-debt` | Scan, track, and prioritize tech debt |
| `/scope-check` | Detect scope creep against plan |
| `/localize` | Localization scan, extract, validate |
| `/perf-profile` | Performance profiling and bottleneck ID |
| `/gate-check` | Validate phase readiness (PASS/CONCERNS/FAIL) |
| `/project-stage-detect` | Analyze project state, detect stage, identify gaps |
| `/reverse-document` | Generate design/architecture docs from existing code |
| `/design-system` | Guided, section-by-section feature spec authoring |
| `/team-ui` | Orchestrate full UI team pipeline |
| `/team-release` | Orchestrate full release team pipeline |
| `/team-polish` | Orchestrate full polish team pipeline |
| `/launch-checklist` | Complete launch readiness validation |
| `/patch-notes` | Generate user-facing release notes |
| `/brainstorm` | Guided product/feature ideation from scratch |
| `/bug-report` | Structured bug report creation |

### 4. Use Templates for New Documents

Templates are in `.claude/docs/templates/`:

- `game-design-document.md` -- use as reference for feature specs
- `architecture-decision-record.md` -- for technical decisions
- `risk-register-entry.md` -- for new risks
- `test-plan.md` -- for feature test plans
- `sprint-plan.md` -- for sprint planning
- `milestone-definition.md` -- for new milestones
- `technical-design-document.md` -- for per-system technical designs
- `post-mortem.md` -- for project/milestone retrospectives
- `release-checklist-template.md` -- for release checklists
- `changelog-template.md` -- for release changelogs
- `release-notes.md` -- for user-facing release notes
- `incident-response.md` -- for live incident response playbooks
- `project-stage-report.md` -- for project stage detection output
- `design-doc-from-implementation.md` -- for reverse-documenting existing code
- `architecture-doc-from-code.md` -- for reverse-documenting code into architecture docs

### 5. Follow the Coordination Rules

1. Work flows down the hierarchy: Leadership -> Leads -> Specialists
2. Conflicts escalate up the hierarchy
3. Cross-department work is coordinated by the `product-manager`
4. Agents do not modify files outside their domain without delegation
5. All decisions are documented

## First Steps for a New Project

**Don't know where to begin?** Run `/start`. It asks where you are and routes
you to the right workflow. No assumptions about your product, stack, or experience level.

If you already know what you need, jump directly to the relevant path:

### Path A: "I have no idea what to build"

1. **Run `/start`** (or `/brainstorm open`) — guided product ideation:
   what problem to solve, who the users are, what the MVP looks like
   - Generates 3 concepts, helps you pick one, defines the core loop
   - Produces a product concept document
2. **Configure your stack** — update `.claude/docs/technical-preferences.md`
   with your chosen frameworks and performance budgets
3. **Validate the concept** — Run `/design-review design/product-concept.md`
4. **Spec out features** — Run `/design-system [feature-name]` for each key feature
5. **Test the core flow** — Run `/prototype [core-feature]`
6. **Plan the first sprint** — Run `/sprint-plan new`
7. Start building

### Path B: "I know what I want to build"

If you already have a product concept and stack choice:

1. **Configure your stack** — update `.claude/docs/technical-preferences.md`
2. **Spec out features** — Run `/design-system [feature-name]` in priority order
3. **Create the initial ADR** — Run `/architecture-decision`
4. **Create the first milestone** in `production/milestones/`
5. **Plan the first sprint** — Run `/sprint-plan new`
6. Start building

### Path C: "I have an existing project"

If you have specs, prototypes, or code already:

1. **Run `/start`** (or `/project-stage-detect`) — analyzes what exists,
   identifies gaps, and recommends next steps
2. **Validate phase readiness** — Run `/gate-check` to see where you stand
3. **Plan the next sprint** — Run `/sprint-plan new`

## File Structure Reference

```
CLAUDE.md                          -- Master config (read this first)
.claude/
  settings.json                    -- Claude Code hooks and project settings
  agents/                          -- 13 agent definitions (YAML frontmatter)
  skills/                          -- 28 slash command definitions (YAML frontmatter)
  hooks/                           -- 8 hook scripts (.sh) wired by settings.json
  rules/                           -- 9 path-specific rule files
  docs/
    quick-start.md                 -- This file
    technical-preferences.md       -- Project-specific standards (configure this first)
    coding-standards.md            -- Coding and feature spec standards
    coordination-rules.md          -- Agent coordination rules
    context-management.md          -- Context budgets and compaction instructions
    review-workflow.md             -- Review and sign-off process
    directory-structure.md         -- Project directory layout
    agent-roster.md                -- Full agent list with tiers
    skills-reference.md            -- All slash commands
    rules-reference.md             -- Path-specific rules
    hooks-reference.md             -- Active hooks
    agent-coordination-map.md      -- Full delegation and workflow map
    setup-requirements.md          -- System prerequisites
    settings-local-template.md     -- Personal settings.local.json guide
    hooks-reference/               -- Hook documentation
    templates/                     -- Document templates
```
