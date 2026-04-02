---
name: release-checklist
description: "Generates a comprehensive pre-release validation checklist covering build verification, test results, security, and deployment readiness."
argument-hint: "[target: web|api|mobile|all]"
user-invocable: true
---

When this skill is invoked:

> **Explicit invocation only**: This skill should only run when the user explicitly requests it with `/release-checklist`. Do not auto-invoke based on context matching.

1. **Read the argument** for the deployment target (`web`, `api`, `mobile`, or `all`).
   Default to `all` if not specified.

2. **Read CLAUDE.md** for project context, version, and deployment targets.

3. **Read the current milestone** from `production/milestones/`.

4. **Scan the codebase** for outstanding issues: TODO, FIXME, HACK counts.

5. **Generate the release checklist**:

```markdown
## Release Checklist: [Version] — [Target]
Generated: [Date]

### Codebase Health
- TODO count: [N]
- FIXME count: [N] (list all — potential blockers)
- HACK count: [N] (list all — need review)

### Build Verification
- [ ] Clean build succeeds
- [ ] No compiler warnings
- [ ] Build version number correctly set ([version])
- [ ] Build is reproducible from tagged commit
- [ ] All environment variables documented

### Quality Gates
- [ ] Zero critical bugs open
- [ ] Zero major bugs open (or documented exceptions)
- [ ] All critical user paths tested and signed off by QA
- [ ] API response times within target (p95)
- [ ] No regression from previous build
- [ ] Integration tests passing in staging

### Content Complete
- [ ] All placeholder content replaced with final versions
- [ ] All user-facing text proofread
- [ ] All strings localization-ready (no hardcoded text)
- [ ] Release notes drafted
```

6. **Add target-specific sections**:

For `web`:
```markdown
### Web-Specific
- [ ] Tested in all supported browsers
- [ ] Mobile viewport tested at common breakpoints
- [ ] Core Web Vitals within thresholds (LCP, FID, CLS)
- [ ] HTTPS enforced, no mixed content
- [ ] 404 and error pages correct
```

For `api`:
```markdown
### API-Specific
- [ ] OpenAPI/Swagger docs updated
- [ ] Breaking changes documented with migration guide
- [ ] Rate limiting configured and tested
- [ ] Auth and authorization tested for all endpoints
- [ ] Input validation covers all endpoints
```

For `mobile`:
```markdown
### Mobile-Specific
- [ ] App store guidelines compliance verified
- [ ] Privacy policy linked and accurate
- [ ] Tested on minimum and target device specs
- [ ] Background/foreground lifecycle handled correctly
- [ ] App size within store limits
```

7. **Add deployment section**:

```markdown
### Deployment Readiness
- [ ] Staging smoke test passed
- [ ] Database migrations tested on staging
- [ ] Environment variables set in production
- [ ] Rollback procedure documented and tested
- [ ] Monitoring alerts configured

### Go / No-Go: [READY / NOT READY]

**Sign-offs Required:**
- [ ] QA Lead
- [ ] Technical Director
- [ ] Product Manager
```

8. **Save** to `production/releases/release-checklist-[version].md`.

9. **Output a summary**: total items, known blockers, file path.
