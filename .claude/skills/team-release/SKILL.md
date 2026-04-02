---
name: team-release
description: "Orchestrate the release team: coordinates release-manager, qa-lead, devops-engineer, and product-manager to execute a release from candidate to deployment."
argument-hint: "[version number or 'next']"
user-invocable: true
---

When this skill is invoked, orchestrate the release team through a structured pipeline.

**Decision Points:** At each phase transition, use `AskUserQuestion` to present
the user with the subagent's proposals as selectable options. Write the agent's
full analysis in conversation, then capture the decision with concise labels.
The user must approve before moving to the next phase.

## Team Composition
- **release-manager** — Release branch, versioning, changelog, deployment
- **qa-lead** — Test sign-off, regression suite, release quality gate
- **devops-engineer** — Build pipeline, artifacts, deployment automation
- **product-manager** — Go/no-go decision, stakeholder communication, scheduling

## How to Delegate

Use the Task tool to spawn each team member as a subagent:
- `subagent_type: release-manager` — Release branch, versioning, changelog, deployment
- `subagent_type: qa-lead` — Test sign-off, regression suite, quality gate
- `subagent_type: devops-engineer` — Build pipeline, artifacts, deployment automation
- `subagent_type: product-manager` — Go/no-go decision, stakeholder communication

Always provide full context in each agent's prompt (version number, milestone status, known issues).

## Pipeline

### Phase 1: Release Planning
Delegate to **product-manager**:
- Confirm all milestone acceptance criteria are met
- Identify any scope items deferred from this release
- Set the target release date and communicate to team
- Output: release authorization with scope confirmation

### Phase 2: Release Candidate
Delegate to **release-manager**:
- Cut release branch from the agreed commit
- Bump version numbers in all relevant files
- Generate the release checklist using `/release-checklist`
- Freeze the branch — no feature changes, bug fixes only
- Output: release branch name and checklist

### Phase 3: Quality Gate (parallel)
Delegate in parallel:
- **qa-lead**: Execute full regression test suite. Test all critical paths. Verify no critical/major bugs. Sign off on quality.
- **devops-engineer**: Build release artifacts for all targets. Verify builds are clean and reproducible. Run automated tests in CI.

### Phase 4: Performance & Security
Delegate (can run in parallel with Phase 3):
- Run performance benchmarks against targets (delegate to performance-analyst if available)
- Verify no new security issues (delegate to security-engineer if available)
- Output: performance and security sign-off

### Phase 5: Go/No-Go
Delegate to **product-manager**:
- Collect sign-off from: qa-lead, release-manager, devops-engineer, technical-director
- Evaluate any open issues — blocking or can they ship?
- Make the go/no-go call
- Output: release decision with rationale

### Phase 6: Deployment (if GO)
Delegate to **release-manager** + **devops-engineer**:
- Tag the release in version control
- Generate release notes using `/patch-notes`
- Deploy to staging for final smoke test
- Deploy to production
- Monitor for 48 hours post-release

### Phase 7: Post-Release
- **release-manager**: Generate release report (what shipped, what was deferred)
- **product-manager**: Update milestone tracking, communicate to stakeholders
- **qa-lead**: Monitor incoming bug reports for regressions
- Schedule post-release retrospective if issues occurred

## Output
A summary report covering: release version, scope, quality gate results, go/no-go decision, deployment status, and monitoring plan.
