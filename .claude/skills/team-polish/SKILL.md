---
name: team-polish
description: "Orchestrate the quality team: coordinates performance-analyst, security-engineer, and qa-tester to optimize, harden, and validate a feature or area for release quality."
argument-hint: "[feature or area to polish]"
user-invocable: true
---

When this skill is invoked, orchestrate the quality team through a structured pipeline.

**Decision Points:** At each phase transition, use `AskUserQuestion` to present
the user with the subagent's proposals as selectable options. Write the agent's
full analysis in conversation, then capture the decision with concise labels.
The user must approve before moving to the next phase.

## Team Composition
- **performance-analyst** — Profiling, optimization, memory analysis, latency
- **security-engineer** — Security review, vulnerability assessment, hardening
- **qa-tester** — Edge case testing, regression testing, soak testing

## How to Delegate

Use the Task tool to spawn each team member as a subagent:
- `subagent_type: performance-analyst` — Profiling, latency, memory, throughput
- `subagent_type: security-engineer` — Vulnerability review, auth checks, data exposure
- `subagent_type: qa-tester` — Edge case testing, regression, stress testing

Always provide full context in each agent's prompt (target feature/area, performance budgets, known issues). Launch independent agents in parallel where the pipeline allows.

## Pipeline

### Phase 1: Assessment
Delegate to **performance-analyst**:
- Profile the target feature/area
- Identify performance bottlenecks and latency outliers
- Measure memory usage and check for leaks
- Benchmark against targets (API p95, page load, throughput)
- Output: performance report with prioritized optimization list

### Phase 2: Performance Optimization
Delegate to **performance-analyst**:
- Fix performance hotspots from Phase 1
- Optimize database queries, caching strategy, network calls
- Verify optimizations don't change functional behavior
- Output: optimized code with before/after metrics

### Phase 3: Security Review (parallel with Phase 2)
Delegate to **security-engineer**:
- Review for common vulnerabilities (OWASP Top 10)
- Verify authentication and authorization are correct
- Check for sensitive data exposure in logs, responses, errors
- Validate input sanitization and output encoding
- Output: security findings with severity ratings

### Phase 4: Hardening
Delegate to **qa-tester**:
- Test all edge cases: boundary conditions, invalid inputs, concurrent operations
- Soak test: run the feature under sustained load checking for degradation
- Stress test: maximum load, worst-case scenarios
- Regression test: verify changes haven't broken existing functionality
- Output: test results with any remaining issues

### Phase 5: Sign-off
- Collect results from all team members
- Compare metrics against budgets
- Report: READY FOR RELEASE / NEEDS MORE WORK
- List remaining issues with severity and recommendations

## Output
A summary report covering: performance before/after metrics, security findings, test results, and release readiness assessment.
