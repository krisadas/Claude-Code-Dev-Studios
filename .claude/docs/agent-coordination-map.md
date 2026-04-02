# Agent Coordination and Delegation Map

## Organizational Hierarchy

```
                           [Human Developer]
                                 |
                   +-------------+-------------+
                   |                           |
           technical-director           product-manager
                   |                           |
           lead-programmer          (coordinates all)
                   |
        +-----+----+----+-----+
        |     |         |     |
      be    fe         net  devops
                         |
                   +-----+-----+
                   |           |
               perf-a     security


  Additional Leads (report to technical-director / product-manager):
    qa-lead              -- Test sign-off, regression suite, quality gate
    release-manager      -- Release pipeline, versioning, deployment

  Specialists:
    backend-engineer     -- APIs, business logic, databases, auth
    frontend-engineer    -- UI components, state, API integration
    network-programmer   -- HTTP, WebSocket, API clients, rate limiting
    devops-engineer      -- CI/CD, infrastructure, deployment
    security-engineer    -- Vulnerability review, auth, data privacy
    ux-designer          -- User flows, wireframes, accessibility design
    performance-analyst  -- Profiling, bottlenecks, optimization
    qa-tester            -- Test case execution, edge cases, regression
```

### Legend
```
be   = backend-engineer       fe   = frontend-engineer
net  = network-programmer     devops = devops-engineer
perf-a = performance-analyst  security = security-engineer
```

## Delegation Rules

### Who Can Delegate to Whom

| From | Can Delegate To |
|------|----------------|
| technical-director | lead-programmer, devops-engineer, security-engineer, performance-analyst |
| product-manager | Any agent (task assignment within their domain only) |
| lead-programmer | backend-engineer, frontend-engineer, network-programmer |
| qa-lead | qa-tester |
| release-manager | devops-engineer (release builds), qa-lead (release testing) |
| security-engineer | network-programmer (security review), lead-programmer (secure patterns) |

### Escalation Paths

| Situation | Escalate To |
|-----------|------------|
| Two engineers disagree on implementation | lead-programmer |
| Design vs technical feasibility conflict | product-manager + technical-director |
| Code architecture disagreement | technical-director |
| Cross-system code conflict | lead-programmer, then technical-director |
| Schedule conflict between workstreams | product-manager |
| Scope exceeds capacity | product-manager |
| Quality gate disagreement | qa-lead, then technical-director |
| Performance budget violation | performance-analyst flags, technical-director decides |

## Common Workflow Patterns

### Pattern 1: New Feature (Full Pipeline)

```
1. product-manager     -- Confirms feature aligns with roadmap and priorities
2. ux-designer         -- Designs user flow and wireframes
3. lead-programmer     -- Designs API contract and architecture
4. backend-engineer    -- Implements API endpoints and business logic
5. frontend-engineer   -- Implements UI components and data binding
6. qa-tester           -- Writes test cases
7. qa-lead             -- Reviews and approves test coverage
8. lead-programmer     -- Code review
9. qa-tester           -- Executes tests
10. product-manager    -- Marks feature complete
```

### Pattern 2: Bug Fix

```
1. qa-tester           -- Files bug report with /bug-report
2. qa-lead             -- Triages severity and priority
3. product-manager     -- Assigns to sprint (if not P0)
4. lead-programmer     -- Identifies root cause, assigns to engineer
5. [backend/frontend-engineer] -- Fixes the bug
6. lead-programmer     -- Code review
7. qa-tester           -- Verifies fix and runs regression
8. qa-lead             -- Closes bug
```

### Pattern 3: Sprint Cycle

```
1. product-manager     -- Plans sprint with /sprint-plan new
2. [All agents]        -- Execute assigned tasks
3. product-manager     -- Daily status with /sprint-plan status
4. qa-lead             -- Continuous testing during sprint
5. lead-programmer     -- Continuous code review during sprint
6. product-manager     -- Sprint retrospective with post-sprint hook
7. product-manager     -- Plans next sprint incorporating learnings
```

### Pattern 4: Milestone Checkpoint

```
1. product-manager     -- Runs /milestone-review
2. technical-director  -- Reviews technical health
3. qa-lead             -- Reviews quality metrics
4. product-manager     -- Facilitates go/no-go discussion
5. [All leads]         -- Agree on scope adjustments if needed
6. product-manager     -- Documents decisions and updates plans
```

### Pattern 5: Release Pipeline

```
1. product-manager     -- Declares release candidate, confirms milestone criteria met
2. release-manager     -- Cuts release branch, generates /release-checklist
3. qa-lead             -- Runs full regression, signs off on quality
4. security-engineer   -- Final security scan
5. performance-analyst -- Confirms performance benchmarks within targets
6. devops-engineer     -- Builds release artifacts, runs deployment pipeline
7. release-manager     -- Generates /changelog, tags release, creates release notes
8. technical-director  -- Final sign-off on major releases
9. release-manager     -- Deploys and monitors for 48 hours
10. product-manager    -- Marks release complete
```

### Pattern 6: Rapid Prototype

```
1. product-manager     -- Defines the hypothesis and success criteria
2. lead-programmer     -- Scaffolds prototype with /prototype
3. [backend/frontend-engineer] -- Builds minimal implementation (hours, not days)
4. product-manager     -- Evaluates prototype against criteria
5. lead-programmer     -- Documents findings report
6. technical-director  -- Go/no-go decision on proceeding to production
7. product-manager     -- Schedules production work if approved
```

## Cross-Domain Communication Protocols

### Feature Spec Change Notification

When a feature spec changes, the product-manager must notify:
- lead-programmer (implementation impact)
- qa-lead (test plan update needed)
- Relevant specialist agents depending on the change

### Architecture Change Notification

When an ADR is created or modified, the technical-director must notify:
- lead-programmer (code changes needed)
- All affected specialist engineers
- qa-lead (testing strategy may change)
- product-manager (schedule impact)

## Anti-Patterns to Avoid

1. **Bypassing the hierarchy**: A specialist agent should never make decisions
   that belong to their lead without consultation.
2. **Cross-domain implementation**: An agent should never modify files outside
   their designated area without explicit delegation from the relevant owner.
3. **Shadow decisions**: All decisions must be documented. Verbal agreements
   without written records lead to contradictions.
4. **Monolithic tasks**: Every task assigned to an agent should be completable
   in 1-3 days. If it is larger, it must be broken down first.
5. **Assumption-based implementation**: If a spec is ambiguous, the implementer
   must ask the specifier rather than guessing. Wrong guesses are more expensive
   than a question.
