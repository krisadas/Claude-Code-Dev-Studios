# Agent Roster

The following agents are available. Each has a dedicated definition file in
`.claude/agents/`. Use the agent best suited to the task at hand. When a task
spans multiple domains, the coordinating agent (usually `product-manager` or the
domain lead) should delegate to specialists.

## Tier 1 -- Leadership Agents (Opus)
| Agent | Domain | When to Use |
|-------|--------|-------------|
| `technical-director` | Technical vision | Architecture decisions, tech stack choices, performance strategy |
| `product-manager` | Product management | Sprint planning, milestone tracking, risk management, coordination |

## Tier 2 -- Department Lead Agents (Sonnet)
| Agent | Domain | When to Use |
|-------|--------|-------------|
| `lead-programmer` | Code architecture | System design, code review, API design, refactoring |
| `qa-lead` | Quality assurance | Test strategy, bug triage, release readiness, regression planning |
| `release-manager` | Release pipeline | Build management, versioning, changelogs, deployment, rollbacks |

## Tier 3 -- Specialist Agents (Sonnet or Haiku)
| Agent | Domain | Model | When to Use |
|-------|--------|-------|-------------|
| `backend-engineer` | Server-side | Sonnet | APIs, business logic, databases, auth, background services |
| `frontend-engineer` | Client-side | Sonnet | UI components, state management, API integration, client performance |
| `devops-engineer` | Build/deploy | Sonnet | CI/CD, build scripts, infrastructure, branching strategy |
| `security-engineer` | Security | Sonnet | Vulnerability review, auth design, data protection, compliance |
| `ux-designer` | UX flows | Sonnet | User flows, wireframes, accessibility, interaction design |
| `qa-tester` | Test execution | Haiku | Writing test cases, bug reports, test checklists |
| `performance-analyst` | Performance | Sonnet | Profiling, optimization recommendations, memory analysis |
| `network-programmer` | Networking | Sonnet | API networking, protocol design, real-time communication |
