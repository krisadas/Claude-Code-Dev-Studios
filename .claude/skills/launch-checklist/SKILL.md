---
name: launch-checklist
description: "Complete launch readiness validation covering every department: code, infrastructure, security, marketing, community, legal, and go/no-go sign-offs."
argument-hint: "[launch-date or 'dry-run']"
user-invocable: true
---

When this skill is invoked:

> **Explicit invocation only**: This skill should only run when the user explicitly requests it with `/launch-checklist`. Do not auto-invoke based on context matching.

1. **Read the argument** for the launch date or `dry-run` mode.

2. **Gather project context**:
   - Read `CLAUDE.md` for tech stack, deployment targets, and team structure
   - Read the latest milestone in `production/milestones/`
   - Read any existing release checklist in `production/releases/`

3. **Scan codebase health**:
   - Count `TODO`, `FIXME`, `HACK` comments and their locations
   - Check for debug output in production code
   - Check for hardcoded dev values (localhost, test credentials, debug flags)
   - Check for hardcoded user-facing strings

4. **Generate the launch checklist**:

```markdown
# Launch Checklist: [Product Name]
Target Launch: [Date or DRY RUN]
Generated: [Date]

---

## 1. Code Readiness

### Build Health
- [ ] Clean build succeeds
- [ ] Zero compiler warnings
- [ ] All unit tests passing
- [ ] All integration tests passing
- [ ] Performance benchmarks within targets
- [ ] Build version correctly set and tagged in source control

### Code Quality
- [ ] TODO count: [N] (zero required for launch, or documented exceptions)
- [ ] FIXME count: [N] (zero required)
- [ ] HACK count: [N] (each must have documented justification)
- [ ] No debug output in production code
- [ ] No hardcoded dev/test values
- [ ] All feature flags set to production values
- [ ] Error handling covers all critical paths
- [ ] Error/crash reporting integrated and verified

### Security
- [ ] No exposed API keys or credentials in source
- [ ] All secrets in environment variables or secrets manager
- [ ] Network communication secured (TLS everywhere)
- [ ] Input validation on all API endpoints
- [ ] Authentication and authorization tested
- [ ] Privacy policy compliance verified (GDPR/CCPA if applicable)
- [ ] Security review completed

---

## 2. Infrastructure

### Deployment
- [ ] Production environment provisioned and smoke-tested
- [ ] Database migrations applied and verified
- [ ] Environment variables and secrets configured
- [ ] SSL/TLS certificates valid and auto-renewing

### Reliability
- [ ] Auto-scaling configured and tested
- [ ] Database backups configured and tested
- [ ] Monitoring and alerting configured
- [ ] Health check endpoints responding
- [ ] Rollback procedure documented and tested

### Observability
- [ ] Error tracking active
- [ ] Logging pipeline verified
- [ ] Key metrics tracked: error rate, latency, uptime
- [ ] Alerts configured for critical thresholds
- [ ] On-call runbook documented

---

## 3. Quality Assurance

### Testing
- [ ] Full regression test suite passed
- [ ] Zero critical bugs open
- [ ] Zero major bugs open (or documented exceptions)
- [ ] All critical user flows tested end-to-end
- [ ] Edge cases tested (empty state, error state, load)
- [ ] Cross-browser / cross-device testing complete (if applicable)

### Performance
- [ ] API response times within targets (p95)
- [ ] Page load times within targets (LCP, FCP)
- [ ] Load test passed at expected peak traffic

### Accessibility & Localization
- [ ] WCAG 2.1 AA minimum compliance verified
- [ ] All user-facing strings externalized (no hardcoded text)
- [ ] All supported languages translated and verified

---

## 4. Legal & Compliance

- [ ] Terms of Service published and linked
- [ ] Privacy Policy published and linked
- [ ] Third-party license attributions complete
- [ ] GDPR/CCPA compliance verified
- [ ] Cookie consent implemented (if applicable)

---

## 5. Community & Marketing

- [ ] Launch announcement drafted and scheduled
- [ ] Documentation / help center complete
- [ ] Support channels set up
- [ ] FAQ and known issues page prepared
- [ ] Support team briefed

---

## 6. Operations

- [ ] On-call schedule set for first 72 hours post-launch
- [ ] Incident response playbook reviewed
- [ ] Rollback plan documented and tested
- [ ] Hotfix pipeline tested (can ship emergency fix within 4 hours)
- [ ] Launch monitoring dashboard ready
- [ ] Go-live procedure documented step by step

---

## Go / No-Go Decision

**Overall Status**: [READY / NOT READY / CONDITIONAL]

### Blocking Items
[List any items that must be resolved before launch]

### Sign-Offs Required
- [ ] Technical Director — Technical health and stability
- [ ] QA Lead — Quality and test coverage
- [ ] Product Manager — Scope and overall readiness
- [ ] Release Manager — Build and deployment readiness
```

5. **Save the checklist** to
   `production/releases/launch-checklist-[date].md`, creating directories as needed.

6. **Output a summary** to the user: total items, blocking items count,
   departments with incomplete sections, and the file path.
