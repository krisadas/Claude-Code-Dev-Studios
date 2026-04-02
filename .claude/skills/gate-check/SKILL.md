---
name: gate-check
description: "Validate readiness to advance between development phases. Produces a PASS/CONCERNS/FAIL verdict with specific blockers and required artifacts."
argument-hint: "[target-phase: design | technical-setup | development | staging | release]"
user-invocable: true
---

# Phase Gate Validation

This skill validates whether the project is ready to advance to the next development
phase. It checks for required artifacts, quality standards, and blockers.

**Distinct from `/project-stage-detect`**: That skill is diagnostic ("where are we?").
This skill is prescriptive ("are we ready to advance?" with a formal verdict).

## Development Stages (5)

The project progresses through these stages:

1. **Concept** — Product ideation, concept document
2. **Design** — Feature specs, architecture decisions
3. **Technical Setup** — Stack configured, ADRs written
4. **Development** — Active feature implementation
5. **Staging** — Testing, QA, hardening
6. **Release** — Launch prep and deployment

**When a gate passes**, write the new stage name to `production/stage.txt`
(single line, e.g. `Development`). This updates the status line immediately.

---

## 1. Parse Arguments

- **With argument**: `/gate-check development` — validate readiness for that phase
- **No argument**: Auto-detect current stage using heuristics, then validate the NEXT transition

---

## 2. Phase Gate Definitions

### Gate: Concept → Design

**Required Artifacts:**
- [ ] `design/product-concept.md` exists and has real content
- [ ] Product pillars defined (in concept doc or `design/product-pillars.md`)

**Quality Checks:**
- [ ] Product concept has been reviewed (`/design-review` verdict not MAJOR REVISION NEEDED)
- [ ] Core user flow is described
- [ ] Target user segment is identified

---

### Gate: Design → Technical Setup

**Required Artifacts:**
- [ ] At least 1 feature spec in `design/features/`
- [ ] Feature dependencies mapped

**Quality Checks:**
- [ ] Feature spec(s) pass design review (8 required sections present)
- [ ] MVP feature set is defined and scoped

---

### Gate: Technical Setup → Development

**Required Artifacts:**
- [ ] Tech stack chosen (CLAUDE.md Technology Stack is not `[CHOOSE]`)
- [ ] Technical preferences configured (`.claude/docs/technical-preferences.md` populated)
- [ ] At least 1 Architecture Decision Record in `docs/architecture/`

**Quality Checks:**
- [ ] Architecture decisions cover core systems (API layer, data layer, auth, state management)
- [ ] Naming conventions and performance budgets set

---

### Gate: Development → Staging

**Required Artifacts:**
- [ ] `src/` has active code organized into modules/services
- [ ] All MVP features from specs are implemented (cross-reference `design/features/` with `src/`)
- [ ] Test files exist in `tests/`
- [ ] At least 1 prototype or spike in `prototypes/` (if applicable)

**Quality Checks:**
- [ ] Tests are passing (run test suite via Bash)
- [ ] No critical/blocker bugs open
- [ ] Core user flows work end-to-end
- [ ] Performance is within budget (check technical-preferences.md targets)

---

### Gate: Staging → Release

**Required Artifacts:**
- [ ] All features from milestone plan are implemented
- [ ] Localization strings are externalized (no hardcoded user-facing text in `src/`)
- [ ] QA test plan exists and has been executed
- [ ] Release checklist completed (`/release-checklist` run)
- [ ] Changelog drafted

**Quality Checks:**
- [ ] Full QA pass signed off by `qa-lead`
- [ ] All tests passing
- [ ] Performance targets met
- [ ] No known critical or high-severity bugs
- [ ] Security review completed
- [ ] Legal requirements met (privacy policy, terms of service, GDPR if applicable)
- [ ] Build compiles and packages cleanly

---

## 3. Run the Gate Check

For each item in the target gate:

### Artifact Checks
- Use `Glob` and `Read` to verify files exist and have meaningful content
- For code checks, verify directory structure and file counts

### Quality Checks
- For test checks: Run the test suite via `Bash` if a test runner is configured
- For spec review checks: `Read` the feature spec and check for the 8 required sections
- For performance checks: `Read` technical-preferences.md and compare against profiling data
- For localization checks: `Grep` for hardcoded strings in `src/`

---

## 4. Collaborative Assessment

For items that can't be automatically verified, **ask the user**:

- "I can't automatically verify that the core flows work end-to-end. Has manual testing been done?"
- "Performance profiling data isn't available. Would you like to run `/perf-profile`?"
- "No security review found. Has a security audit been completed?"

**Never assume PASS for unverifiable items.** Mark them as MANUAL CHECK NEEDED.

---

## 5. Output the Verdict

```
## Gate Check: [Current Phase] → [Target Phase]

**Date**: [date]
**Checked by**: gate-check skill

### Required Artifacts: [X/Y present]
- [x] design/product-concept.md — exists, 3.1KB
- [ ] docs/architecture/ — MISSING (no ADRs found)

### Quality Checks: [X/Y passing]
- [x] Feature spec has 8/8 required sections
- [ ] Tests — FAILED (3 failures in tests/unit/)
- [?] End-to-end flows tested — MANUAL CHECK NEEDED

### Blockers
1. **No Architecture Decision Records** — Run `/architecture-decision` before entering Development.
2. **3 test failures** — Fix failing tests before advancing.

### Recommendations
- [Priority actions to resolve blockers]
- [Optional improvements that aren't blocking]

### Verdict: [PASS / CONCERNS / FAIL]
- **PASS**: All required artifacts present, all quality checks passing
- **CONCERNS**: Minor gaps exist but can be addressed during the next phase
- **FAIL**: Critical blockers must be resolved before advancing
```

---

## 6. Update Stage on PASS

When the verdict is **PASS** and the user confirms they want to advance:

1. Write the new stage name to `production/stage.txt` (single line, no trailing newline)
2. Always ask before writing: "Gate passed. May I update `production/stage.txt` to '[Stage]'?"

---

## 7. Follow-Up Actions

Based on the verdict, suggest specific next steps:

- **No product concept?** → `/brainstorm`
- **Missing feature specs?** → `/design-system <feature-name>`
- **Missing ADRs?** → `/architecture-decision`
- **Tests failing?** → delegate to `lead-programmer` or `qa-tester`
- **Performance unknown?** → `/perf-profile`
- **Not localized?** → `/localize`
- **Ready for release?** → `/launch-checklist`

---

## Collaborative Protocol

1. **Scan first**: Check all artifacts and quality gates
2. **Ask about unknowns**: Don't assume PASS for things you can't verify
3. **Present findings**: Show the full checklist with status
4. **User decides**: The verdict is a recommendation — the user makes the final call
5. **Get approval**: "May I write this gate check report to production/gate-checks/?"

**Never** block a user from advancing — the verdict is advisory.
