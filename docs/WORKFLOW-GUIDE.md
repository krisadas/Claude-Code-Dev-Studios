# Claude Code Software Studios -- Complete Workflow Guide

> **How to go from zero to a shipped product using the Agent Architecture.**
>
> This guide walks you through every phase of software development using the
> 13-agent system and slash commands. It assumes you have Claude Code installed
> and are working from the project root.

---

## Table of Contents

1. [Phase 0: Setup & Configuration](#phase-0-setup--configuration)
2. [Phase 1: Ideation & Concept](#phase-1-ideation--concept)
3. [Phase 2: Design & Architecture](#phase-2-design--architecture)
4. [Phase 3: Prototyping & Validation](#phase-3-prototyping--validation)
5. [Phase 4: Sprint Workflow](#phase-4-sprint-workflow)
6. [Phase 5: Implementation Deep-Dive](#phase-5-implementation-deep-dive)
7. [Phase 6: Testing & Quality Assurance](#phase-6-testing--quality-assurance)
8. [Phase 7: Polish & Hardening](#phase-7-polish--hardening)
9. [Phase 8: Localization & Accessibility](#phase-8-localization--accessibility)
10. [Phase 9: Release & Launch](#phase-9-release--launch)
11. [Phase 10: Post-Launch](#phase-10-post-launch)
12. [Appendix A: Agent Quick-Reference](#appendix-a-agent-quick-reference)
13. [Appendix B: Slash Command Quick-Reference](#appendix-b-slash-command-quick-reference)
14. [Appendix C: Common Workflows](#appendix-c-common-workflows)

---

## Phase 0: Setup & Configuration

### What You Need

Before you start, make sure you have:

- **Claude Code** installed and working
- **Git** with standard terminal (Mac/Linux) or Git Bash (Windows)
- **jq** (optional but recommended -- hooks fall back to `grep` if missing)
- **Node.js / Python / Go** depending on your chosen stack

### Step 0.1: Clone and Configure

Clone the repository and open it in your editor:

```bash
git clone <repo-url> my-project
cd my-project
```

### Step 0.2: Run /start (Recommended for New Users)

If you're new to the project or don't yet have a product concept:

```
/start
```

This guided onboarding asks where you are (no idea, vague idea, clear concept,
existing work) and routes you to the right phase.

### Step 0.3: Configure Your Stack

Edit the Technology Stack section in `CLAUDE.md` directly, or ask the
`technical-director` agent to help you choose:

```
Ask the technical-director to recommend a stack for a web app
with a React frontend, RESTful API, and PostgreSQL database.
```

**What to configure:**
- Frontend framework (React, Next.js, or Angular)
- Backend language/framework (Node.js, Golang, or Python)
- Database
- Deployment target
- Populate `.claude/docs/technical-preferences.md` with naming conventions
  and performance budgets

> **Manual alternative:** Edit the Technology Stack section in `CLAUDE.md`
> directly, then fill in `.claude/docs/technical-preferences.md`.

### Step 0.4: Verify Hooks Are Working

Start a new Claude Code session. You should see output from the
`session-start.sh` hook:

```
=== Claude Code Software Studios -- Session Context ===
Branch: main
Recent commits:
  abc1234 Initial commit
===================================
```

If you see this, hooks are working. If not, check `.claude/settings.json` to
make sure the hook paths are correct for your OS.

### Step 0.5: Create Your Directory Structure

Create directories as needed -- the system expects this layout:

```
src/                  # Application source code
  core/               # Shared utilities and core logic
  api/                # Backend API / services
  frontend/           # Frontend application
  workers/            # Background jobs / async workers
assets/               # Static assets
  data/               # JSON config / seed data
design/               # Design documents
  features/           # Feature specification documents
  architecture/       # Architecture Decision Records
docs/                 # Technical documentation
  api/                # API documentation (OpenAPI specs)
  postmortems/        # Post-mortems
tests/                # Test suites
  unit/
  integration/
  performance/
prototypes/           # Throwaway prototypes
production/           # Sprint plans, milestones, releases
  sprints/
  milestones/
  releases/
```

> **Tip:** You don't need all of these on day one. Create directories as you
> reach the phase that needs them. The **rules system** enforces standards
> based on file paths — code in `src/api/` gets network code rules,
> code in `src/core/` gets core code rules, and so on.

---

## Phase 1: Ideation & Concept

### What Happens in This Phase

You go from "no idea" or "vague idea" to a structured product concept document.
This is where you figure out **what** you're building and **for whom**.

> **Tip:** If you ran `/start` in Phase 0 and chose Path A or B, you're already
> here. `/start` routes you to `/brainstorm` automatically.

### Step 1.1: Brainstorm With `/brainstorm`

```
/brainstorm
```

Or with a domain hint:

```
/brainstorm developer tooling
```

**What happens:** The skill guides you through collaborative product discovery:

Agent: "Let's explore product concepts. What problem space interests you? (Optional)"

User: "Developer tooling — something that helps teams ship faster"

Agent: "I've generated 3 product directions in the developer tooling space:

       **Direction A: Automated Code Review** — AI-powered review that understands context
       **Direction B: Deployment Intelligence** — Predictive deployment risk analysis
       **Direction C: Onboarding Accelerator** — Reduces time-to-productivity for new hires

       Which direction resonates? Or shall I explore more?"

User: "Direction B sounds differentiated"

Agent: [Performs Jobs-to-be-Done analysis, user profiling, MVP definition, stack
       recommendation for Direction B, asking for feedback at each stage]

       "Here's the complete analysis. Should I formalize this into a
       product concept document?"

User: "Yes"

Agent: [Creates structured concept doc with value proposition, pillars, user segments,
       MVP scope, and stack recommendation]

### Step 1.2: Document the Product Concept

The brainstorm output becomes `design/product-concept.md`. This document includes:
- One-sentence value proposition
- Primary user segment
- Core user flow (onboarding moment, daily use, retention hook)
- Product pillars (3-5 design principles that guide decisions)
- Anti-features (what this product explicitly does NOT do)
- MVP definition
- Stack recommendation

### Step 1.3: Define Your Product Pillars

Lock in **3-5 pillars** before going further. Example pillars:
- **Zero Config**: Works out of the box without setup friction
- **Signal over Noise**: Every alert is actionable, never spammy
- **Team-Aware**: Understands team context, not just individual activity

Also define **anti-features**:
- We do NOT pursue: gamification, social feeds, manual configuration

> **When you're happy with your concept and pillars, you're ready for Design.**

---

## Phase 2: Design & Architecture

### What Happens in This Phase

You create all the feature specifications and architecture decisions that define
how your product works. This is design and architecture — not yet implementation.

### Step 2.1: Spec Each Feature With `/design-system`

For each feature in your product, create a specification:

```
/design-system user-authentication
```

This guides you through creating a complete feature spec in `design/features/`
section by section. Each spec requires these 8 sections:

| # | Section | What Goes Here |
|---|---------|---------------|
| 1 | **Overview** | One-paragraph summary |
| 2 | **User Story** | Who uses this and why |
| 3 | **Detailed Requirements** | Unambiguous specs — if two engineers read this, they'd build the same thing |
| 4 | **Data Models** | Key entities and relationships |
| 5 | **Edge Cases** | What happens in unusual situations |
| 6 | **Dependencies** | Other features/services this relies on (bidirectional) |
| 7 | **Configuration** | Values operators can adjust, with safe ranges |
| 8 | **Acceptance Criteria** | Testable success conditions |

**Example feature specs you might create:**
- `design/features/user-auth.md` — Authentication and authorization
- `design/features/dashboard.md` — Main user dashboard
- `design/features/notifications.md` — Alert and notification system
- `design/features/api-keys.md` — API key management

### Step 2.2: Design Review

Before any spec goes to engineers, run a review:

```
/design-review design/features/user-auth.md
```

The skill checks the document against the 8-section standard:
- Are all sections present and complete?
- Are requirements unambiguous?
- Are API contracts defined?
- Are edge cases explicitly resolved (not "TBD")?
- Are acceptance criteria testable?

Fix any issues flagged before proceeding.

### Step 2.3: Technical Architecture

Before coding starts, make key technical decisions:

```
/architecture-decision "REST vs GraphQL for our API layer"
```

**What happens:** The skill guides you through creating an Architecture Decision Record:

Agent: "I'll help document this decision. What's driving the choice right now?"

User: "We need to decide between REST and GraphQL for our API. We have 3 client
      types: web app, mobile, and third-party integrations."

Agent: "Given 3 client types with different data needs, here's my analysis:

       **Option A: REST**
       Pros: Simple, well-understood, easy to cache, great tooling
       Cons: Over-fetching for mobile, multiple endpoints for complex views

       **Option B: GraphQL**
       Pros: Flexible queries, single endpoint, great for varied clients
       Cons: Complexity, caching harder, learning curve, N+1 query risk

       **Option C: REST with view-specific endpoints**
       Pros: Performance of REST, solves over-fetching, simple to cache
       Cons: More endpoints to maintain

       For 3 different client types, I recommend Option A (REST) initially —
       the complexity cost of GraphQL isn't justified until you have proven
       over-fetching is a real pain point. You can migrate later.

       Should I create an ADR documenting this decision?"

User: "Yes, go with REST"

Agent: [Creates ADR in docs/architecture/ with context, options, rationale, consequences]

**For complex architectural decisions, use the `technical-director` agent:**

```
Ask the technical-director to evaluate whether we should use a monolith
or microservices architecture for our initial launch, given we're a team
of 2 with a 3-month timeline.
```

### Step 2.4: Milestone Planning

Define your project milestones:

1. **MVP** — Core value proposition working end-to-end, no polish
2. **Private Beta** — All planned features functional, invited users only
3. **Public Beta** — Content-complete, accepting all users, gathering feedback
4. **v1.0** — Production-ready, performance targets met, fully supported

> **When all your feature specs are reviewed and your architecture is
> decided, you're ready to validate your riskiest assumptions.**

---

## Phase 3: Prototyping & Validation

### What Happens in This Phase

Before committing to full production, validate risky or uncertain technical
approaches with throwaway prototypes. This saves weeks of wasted effort.

### Step 3.1: Identify What Needs Prototyping

Prototype when:
- A technical approach is risky and you're not sure it's feasible
- A feature concept is novel and you're not sure the UX will work
- Two implementation approaches both seem viable and you need to compare

### Step 3.2: Run the Prototype Skill

```
/prototype "real-time deployment risk scoring using git metadata"
```

**What happens:** The skill collaborates with you to set up the prototype:

Agent: "Let's prototype the deployment risk scoring. First, let me clarify the
       hypothesis we're testing:

       - Are we testing if the scoring algorithm produces meaningful signal?
       - Or testing the technical feasibility of processing git metadata in real-time?
       - Or validating whether users find the output actionable?

       What's the core question this prototype must answer?"

User: "Testing if git metadata alone (file churn, commit frequency, author count)
      produces meaningful risk signal without needing code analysis"

Agent: "Got it. Here's my proposed setup:

       **Hypothesis**: Git metadata features (churn rate, author count, time-since-last-change)
       can predict deployment risk with >70% accuracy vs historical incidents

       **Prototype Scope** (1-2 days max):
       - Script to extract features from a sample git repo
       - Simple scoring formula (weighted sum of features)
       - Output: risk score per commit with comparison to known incidents
       - No UI, no API — pure data analysis

       Does this match what you want to validate?"

User: "Yes, use our own repo as the sample"

Agent: "Updated to use this repo. Should I create prototypes/risk-scoring/ and implement?"

User: "Yes"

Agent: [Creates directory, README with hypothesis and criteria, implements minimal prototype]

**Key rule:** The `prototype-code` rule (`.claude/rules/prototype-code.md`)
intentionally relaxes standards for prototypes — hardcoded values OK, no tests
required, but README with hypothesis and findings is mandatory.

### Step 3.3: Evaluate the Prototype

Run the prototype and answer:
- Does the hypothesis hold? Is it feasible / accurate / useful?
- What surprised you?
- What would change for production?

Update the prototype README with findings and a **verdict**: Proceed, Pivot, or Kill.

> **When your prototypes have validated (or invalidated) your riskiest
> assumptions, you're ready for production sprints.**

---

## Phase 4: Sprint Workflow

### What Happens in This Phase

This is the core development loop. You work in sprints (typically 1-2 weeks),
implementing features, fixing bugs, and tracking progress.

### Step 4.1: Create Your First Sprint

```
/sprint-plan new
```

**What happens:** The skill delegates to `product-manager` to collaborate on planning:

Agent: "Let's plan your sprint. What's your primary goal? (One sentence)"

User: "Implement user authentication with email/password and JWT"

Agent: "How much time do you have available this sprint?"

User: "30 hours over 2 weeks"

Agent: "Based on the user-auth spec in design/features/user-auth.md, here's my
       proposed breakdown:

       **Must Have** (sprint fails without these):
       - User registration endpoint with validation
       - Password hashing (bcrypt)
       - Login endpoint returning JWT
       - JWT middleware for protected routes
       - Integration tests for auth flows

       **Should Have** (important but can slip):
       - Refresh token mechanism
       - Rate limiting on auth endpoints
       - Password strength validation

       **Nice to Have** (if time permits):
       - Email verification flow
       - 'Remember me' functionality

       **Risks I see**:
       - Email delivery service not chosen yet (blocks email verification)
       - JWT secret rotation strategy not decided (need ADR)

       Does this breakdown match your vision?"

User: "Move email verification to Should Have — it's important for production"

Agent: "Updated. Should I create production/sprints/sprint-01.md with this plan?"

User: "Yes"

### Step 4.2: The Daily Work Loop

Each session follows this pattern:

```
1. Start a Claude Code session
   (session-start hook shows branch, recent commits)

2. Pick a task from the sprint plan
   ("I'm working on the JWT middleware")

3. Claude identifies the right agent(s) to help
   - backend-engineer for API / server-side work
   - frontend-engineer for client-side work
   - lead-programmer for architecture questions

4. Implement the feature (see Phase 5 for details)

5. Write tests (verification-driven development)

6. Commit your work

7. Repeat until session context is ~65-70% full

8. /clear to start fresh, or compact to continue
```

### Step 4.3: Sprint Tracking and Scope

Check on sprint progress:

```
/sprint-plan status
```

If scope is growing:

```
/scope-check production/sprints/sprint-03.md
```

This compares current scope against the original plan and flags:
- Tasks added since sprint start
- Scope increase percentage
- Recommendations for what to cut

### Step 4.4: Sprint Review & Retrospective

At the end of a sprint:

```
/retrospective
```

Then plan the next sprint:

```
/sprint-plan new
```

### Step 4.5: Milestone Reviews

At milestone checkpoints:

```
/milestone-review "mvp"
```

This produces a comprehensive report:
- Feature completeness percentage
- Quality metrics (bug counts, test coverage)
- Risk assessment
- Go/no-go recommendation
- Action items to reach the milestone

---

## Phase 5: Implementation Deep-Dive

### What Happens in This Phase

This is where code gets written. The agent system ensures code quality through
path-specific rules, automated hooks, and structured delegation.

### Step 5.1: Backend Feature Implementation

For a backend feature (e.g., "implement the user registration endpoint"):

```
Ask the backend-engineer agent to implement the user registration endpoint
as specified in design/features/user-auth.md. The endpoint should be at
POST /api/v1/auth/register.
```

**What happens:** The `backend-engineer` agent collaborates on implementation:

Agent: "I've read design/features/user-auth.md. Before implementing, I need to
       clarify a few details:

       The spec says 'validate email format and uniqueness' — questions:
       - Should I return a generic 'invalid credentials' error for duplicate emails,
         or a specific 'email already registered' error? (security vs UX tradeoff)
       - What's the password minimum length? The spec says 'strong password'
         but doesn't define it.
       - Should I send a verification email immediately, or is that out of scope
         for this sprint?

       Also, what's our preferred HTTP error response format?"

User: "Generic error for duplicates (security). Minimum 8 chars, 1 uppercase, 1 number.
      Skip email verification for now. Use { error: { code, message } } format."

Agent: "Got it. Here's my implementation approach:

       **Files to create**:
       - src/api/auth/register.ts — handler logic
       - src/api/auth/validators.ts — input validation
       - src/core/users/user-repository.ts — database access
       - tests/integration/auth/register.test.ts — integration tests

       **Architecture**:
       - Handler validates input, calls repository, returns JWT
       - Repository handles DB queries (testable via interface)
       - All config values from environment variables

       May I proceed?"

User: "Yes"

Agent: [Implements with rule enforcement: no hardcoded values, input validation,
       parameterized queries, proper error handling]

       "Implementation complete. Ready for testing. Should I write the integration tests?"

### Step 5.2: Frontend Feature Implementation

For a frontend feature:

```
Ask the frontend-engineer agent to implement the login form component
as specified in design/features/user-auth.md. Use React with TypeScript.
```

**What happens:** The `frontend-engineer` collaborates on implementation:

Agent: "I've read the spec. A few questions before building:

       - Should this be a standalone page or a modal/drawer?
       - Is there an existing form component to build on, or starting from scratch?
       - What should happen after successful login? (redirect, refresh, etc.)
       - Do we have a design system or should I use a component library?"

User: "Standalone page at /login. No existing form component. Redirect to /dashboard.
      Use Tailwind CSS."

Agent: "Here's my approach:

       **Files to create**:
       - src/frontend/pages/Login.tsx — page component
       - src/frontend/components/LoginForm.tsx — form component
       - src/frontend/hooks/useAuth.ts — auth state hook

       **Architecture**:
       - Controlled form with React state
       - API call via useAuth hook (keeps component clean)
       - Loading, error, and success states handled explicitly
       - Keyboard accessible (tab order, Enter to submit)

       Approve this structure?"

User: "Approved"

Agent: [Implements with rule enforcement: accessible HTML, no hardcoded text,
       loading/error states, no direct state mutation]

### Step 5.3: UI Team Orchestration

For features that span UX design and frontend implementation:

```
/team-ui "notifications settings page"
```

**What happens:** The team skill orchestrates a structured pipeline:

**Phase 1 - UX Design** (ux-designer):
Agent: "I'll design the notifications settings page. Questions:
       - What notification types does the user control?
       - Should settings be global or per-workspace?
       - How granular should the controls be? (on/off vs frequency vs channels)"

User: [Answers questions]

Agent: [Creates UX spec with wireframes and interaction map]
       "UX spec ready. Approve to proceed to implementation?"

**Phase 2 - Implementation** (frontend-engineer):
Agent: "Implementing the settings page per the UX spec...
       - All text through i18n system
       - Keyboard accessible
       - Loading and error states for each save action"

**Phase 3 - Review**:
Agent (ux-designer): "Verifying against wireframes and accessibility standards..."
Agent (frontend-engineer): "Checking bundle impact and browser compatibility..."

### Step 5.4: Architecture Decision Records

When you face a significant technical choice during implementation:

```
/architecture-decision "Database connection pooling strategy"
```

This documents the decision so future contributors understand why things
were built this way.

### Step 5.5: Code Review

After implementing a feature:

```
/code-review src/api/auth/register.ts
```

**What happens:**

Agent: "Reviewing src/api/auth/register.ts...

       **Architecture Compliance**:
       ✓ Matches ADR-002 (repository pattern)
       ✓ Input validation at API boundary
       ✗ Issue: Direct database call in handler — breaks repository pattern

       **Security**:
       ✓ Parameterized queries (no SQL injection risk)
       ✓ Password hashed before storage
       ⚠ Warning: Error message may leak user existence — consider generic message

       **Testability**:
       ✗ Handler directly imports UserRepository — hard to mock in tests

       **Recommendations**:
       1. HIGH: Inject repository via parameter to enable mocking
       2. HIGH: Make error message generic for duplicate email
       3. LOW: Add JSDoc to exported function

       Should I show you how to fix issue #1?"

---

## Phase 6: Testing & Quality Assurance

### What Happens in This Phase

Testing runs throughout development. The system supports verification-driven
development with automated enforcement.

### Step 6.1: Write Tests Alongside Implementation

The `test-standards` rule (`.claude/rules/test-standards.md`) enforces:
- Test naming: `test_[module]_[scenario]_[expected_result]`
- Structure: Arrange / Act / Assert
- No external state dependencies (mock external services)
- Tests clean up after themselves

```
Ask the qa-tester agent to write test cases for the user registration
endpoint based on the acceptance criteria in design/features/user-auth.md.
```

**What happens:** The `qa-tester` agent collaborates on test creation:

Agent: "From the acceptance criteria, I see:
       - Registration with valid data returns 201 and JWT
       - Duplicate email returns 400 with generic error
       - Invalid password format returns 400 with validation errors
       - Missing required fields return 400

       Questions:
       - Should I include integration tests (hit real DB) or just unit tests?
       - Do you want load tests? (e.g., 100 concurrent registrations)
       - Any specific edge cases beyond the spec?"

User: "Both unit and integration tests. Add a test for SQL injection attempt.
      Skip load tests for now."

Agent: [Writes tests following test-standards: proper naming, AAA structure,
       database cleanup after each integration test]

### Step 6.2: Bug Reporting

```
/bug-report "Login returns 500 when email contains special characters"
```

This creates a structured bug report with:
- Severity (Critical / Major / Minor)
- Steps to reproduce
- Expected vs actual behavior
- Component affected
- Related feature spec

### Step 6.3: Test Planning for Milestones

```
Ask the qa-lead agent to create a test plan for the MVP milestone,
covering authentication, dashboard, and API key management.
```

**What happens:** The `qa-lead` collaborates on comprehensive test planning:

Agent: "I'll create a test plan for MVP. From the milestone definition:
       - Authentication system
       - Dashboard
       - API key management

       Questions:
       - What browsers/devices are in scope?
       - What's our performance target (API p95, page load)?
       - Any known risky areas that need extra coverage?
       - Timeline for testing completion?"

User: "Chrome/Firefox/Safari. API p95 < 200ms. Auth and API key management are risky.
      Testing must complete by end of next week."

Agent: [Creates detailed test plan with functional, edge case, regression,
       performance, and security sections]

---

## Phase 7: Polish & Hardening

### What Happens in This Phase

Your product is feature-complete. Now you make it production-ready: performance,
security hardening, and edge case coverage.

### Step 7.1: Performance Profiling

```
/perf-profile
```

**What happens:**

Agent: "Starting performance profiling. Let's establish targets:
       - API response time target (p95)?
       - Page load target?
       - Throughput target (requests/second)?"

User: "API p95 < 200ms. LCP < 2.5s. Handle 100 req/s."

Agent: "Running profiler on current build...

       **Current Performance**:
       - API p95: 480ms (target: 200ms)
       - LCP: 3.8s (target: 2.5s)
       - Throughput: 40 req/s (target: 100 req/s)

       **Bottlenecks Identified**:
       1. Database queries: N+1 on dashboard endpoint — 3 queries per item
       2. No response caching on static data endpoints
       3. Frontend bundle: 2.1MB uncompressed, no code splitting

       **Recommendations**:
       1. Fix N+1 with JOIN or dataloader
       2. Add Redis cache for user session data (TTL: 5min)
       3. Implement route-based code splitting

       Should I generate detailed optimization tasks?"

**For deeper analysis, use the `performance-analyst` agent directly.**

### Step 7.2: Team Polish

Use the team polish skill for comprehensive hardening:

```
/team-polish "authentication system"
```

**What happens:** The skill coordinates three specialists in parallel:

**Phase 1 - Performance** (performance-analyst):
Agent: "Profiling auth system...
       Login endpoint p95: 380ms (above 200ms target)
       Root cause: bcrypt cost factor 14 — try cost factor 12
       Expected gain: 380ms → 180ms"

**Phase 2 - Security** (security-engineer):
Agent: "Security review of auth system:
       ✓ Passwords hashed with bcrypt
       ✓ JWT signed with RS256
       ⚠ No rate limiting on login endpoint — brute force risk
       ⚠ JWT expiry is 7 days — recommend 1 hour + refresh token
       ✗ Refresh tokens not invalidated on logout — token revocation needed"

**Phase 3 - Hardening** (qa-tester):
Agent: "Edge case and stress testing...
       - Concurrent login attempts: handles correctly
       - Invalid JWT formats: all rejected correctly
       - Session after password change: ✗ old JWTs still valid — needs fix"

**Sign-off**: All specialists report → coordinator generates readiness report.

### Step 7.3: Technical Debt

Before release, assess your tech debt:

```
/tech-debt
```

This scans the codebase for:
- TODO/FIXME/HACK comments
- Code duplication
- Overly complex functions
- Missing tests
- Outdated dependencies

Each debt item gets categorized and prioritized. You decide what to fix
before release vs. what to defer.

---

## Phase 8: Localization & Accessibility

### Step 8.1: Localization Scan

```
/localize src/
```

This scans for:
- Hardcoded strings that should be externalized
- String concatenation that breaks translation
- Text that doesn't account for expansion
- Missing locale files

### Step 8.2: Accessibility Review

```
Ask the ux-designer agent to audit our UI for WCAG 2.1 AA compliance,
keyboard navigation, and screen reader compatibility.
```

The `ui-code` rule already enforces some accessibility:
- i18n-ready strings (no hardcoded text)
- Keyboard accessible elements
- Loading and error states

The `ux-designer` goes deeper:
- Focus management and keyboard traps
- ARIA labeling completeness
- Color contrast ratios
- Motion preferences (`prefers-reduced-motion`)
- Screen reader announcements for dynamic content

---

## Phase 9: Release & Launch

### What Happens in This Phase

Your product is polished, tested, and ready. Now you ship it.

### Step 9.1: Release Checklist

```
/release-checklist v1.0.0
```

This generates a comprehensive pre-release checklist covering:
- Build verification (clean build, no warnings)
- Quality gates (zero critical bugs, tests passing)
- Security (no exposed secrets, TLS configured, auth tested)
- Content complete (no placeholders, all text proofread)
- Deployment readiness (staging smoke test, rollback plan)

### Step 9.2: Launch Readiness (Full Validation)

```
/launch-checklist
```

A complete cross-department validation:

| Area | What's Checked |
|------|---------------|
| **Code** | Build stability, test suite, code quality |
| **Infrastructure** | Deployment, scaling, backups, monitoring |
| **Security** | Secrets management, TLS, auth, GDPR compliance |
| **Performance** | API p95, page load, throughput under load |
| **Accessibility** | WCAG compliance, keyboard nav, screen readers |
| **Localization** | All strings externalized, translations verified |
| **Legal** | Terms of service, privacy policy, license attributions |
| **Community** | Documentation, FAQ, support channels ready |
| **Operations** | On-call schedule, incident playbook, rollback plan |

Each item gets a **Go / No-Go** status. All must be Go to ship.

### Step 9.3: Generate Release Communications

```
/patch-notes v1.0.0
```

Generates user-friendly release notes from git history and sprint data.
Translates developer language ("refactored auth middleware") into user
language ("improved login reliability").

```
/changelog v1.0.0
```

Generates an internal changelog (more technical, for the team).

### Step 9.4: Coordinate the Release

```
/team-release v1.0.0
```

**What happens:** The team release skill coordinates all roles for launch:

**Phase 1 - Release Planning** (product-manager):
Agent: "Starting release coordination for v1.0.0. Reviewing milestone status...

       ✓ All MVP features complete
       ✓ QA sign-off received
       ⚠ Infrastructure: load test pending
       ✗ Legal: privacy policy not yet published

       Two items need resolution before go/no-go."

User: "Load test running now. Privacy policy publishes today."

**Phase 2 - Release Candidate** (release-manager):
Agent: "Cutting release branch v1.0.0...
       - Version bumped in all relevant files
       - Release checklist generated
       - Branch frozen for feature changes"

**Phase 3 - Quality Gate** (qa-lead + devops-engineer in parallel):
Agent (qa-lead): "Running full regression suite on release build...
       All critical and high tests: ✓ Pass. QA signs off."
Agent (devops-engineer): "Build artifacts generated. Staging smoke test: ✓ Pass.
       Deployment pipeline ready."

**Phase 4 - Go/No-Go** (product-manager):
Agent: "All sign-offs received:
       ✓ QA, Technical Director, Release Manager, DevOps
       GO for launch."

### Step 9.5: Ship

```
git tag v1.0.0
git push origin main --tags
```

The deployment pipeline takes over from here.

---

## Phase 10: Post-Launch

### Step 10.1: Hotfix Workflow

When a critical bug appears in production:

```
/hotfix "API returns 500 on accounts created before v0.9.0 due to missing field"
```

This bypasses normal sprint processes with a full audit trail:
1. Creates a hotfix branch
2. Tracks approvals
3. Implements the fix with the `lead-programmer`
4. Ensures the fix is backported to the development branch
5. Documents the incident

### Step 10.2: Post-Mortem

After launch dust settles:

```
Ask Claude to create a post-mortem using the template at
.claude/docs/templates/post-mortem.md
```

This covers:
- What went well
- What went poorly
- Key metrics (signups, error rates, performance)
- Lessons for the next milestone

### Step 10.3: Retrospective

```
/retrospective
```

Document what the team learned from the release and incorporate it into
the next sprint plan.

---

## Appendix A: Agent Quick-Reference

### "I need to do X -- which agent do I use?"

| I need to... | Agent | Tier |
|-------------|-------|------|
| Brainstorm a product idea | `/brainstorm` skill | — |
| Make a product/scope decision | `product-manager` | 1 |
| Make a technical architecture decision | `technical-director` | 1 |
| Review code architecture | `lead-programmer` | 2 |
| Plan a test strategy | `qa-lead` | 2 |
| Manage a release | `release-manager` | 2 |
| Implement backend features | `backend-engineer` | 3 |
| Implement frontend features | `frontend-engineer` | 3 |
| Set up CI/CD or infrastructure | `devops-engineer` | 3 |
| Audit for security vulnerabilities | `security-engineer` | 3 |
| Design UX flows and wireframes | `ux-designer` | 3 |
| Write test cases and bug reports | `qa-tester` | 3 |
| Profile and optimize performance | `performance-analyst` | 3 |
| Design API networking or protocols | `network-programmer` | 3 |

### Agent Hierarchy

```
                    technical-director / product-manager
                                   |
               ------------------------------------------------
               |              |           |          |        |
         lead-programmer   qa-lead   release-mgr  ux-designer  devops
               |
    -------------------------
    |           |            |
 backend    frontend    network-prog
 engineer   engineer
```

**Escalation rule:** Scope/product conflicts go to `product-manager`.
Technical conflicts go to `technical-director`.

---

## Appendix B: Slash Command Quick-Reference

### By Workflow Stage

| Stage | Commands |
|-------|----------|
| **Onboarding** | `/start` |
| **Ideation** | `/brainstorm` |
| **Design** | `/design-system`, `/design-review`, `/architecture-decision` |
| **Sprint** | `/sprint-plan`, `/estimate`, `/scope-check`, `/retrospective` |
| **Implementation** | `/code-review`, `/prototype`, `/tech-debt` |
| **Testing** | `/perf-profile`, `/bug-report` |
| **Quality** | `/localize` |
| **Release** | `/release-checklist`, `/launch-checklist`, `/changelog`, `/patch-notes`, `/hotfix` |
| **Production** | `/milestone-review`, `/gate-check`, `/project-stage-detect`, `/onboard` |
| **Teams** | `/team-ui`, `/team-release`, `/team-polish` |

---

## Appendix C: Common Workflows

### Workflow 1: "I just started and have no product idea"

```
1. /start (asks where you are, routes you to the right workflow)
   — or /brainstorm if you prefer to jump straight to ideation
2. Pick the best direction from the brainstorm output
3. Create a product concept doc (design/product-concept.md)
4. Define product pillars
5. /design-review on your concept doc
6. /design-system for each feature (guided, section-by-section)
```

### Workflow 2: "I have a design and want to start coding"

```
1. /design-review on each feature spec to make sure they're solid
2. /architecture-decision for your first major tech choice
3. /sprint-plan new to plan your first sprint
4. Start implementing with backend-engineer / frontend-engineer
5. /code-review after each major feature
6. Write tests alongside code
7. Commit frequently (hooks validate automatically)
```

### Workflow 3: "I need to add a complex feature"

```
1. Create/update the feature spec in design/features/
2. /design-review to validate the design
3. /estimate to understand effort and risk
4. Use the appropriate team skill:
   - /team-ui for UI features
   - /team-polish for hardening existing features
5. /code-review the implementation
```

### Workflow 4: "Something broke in production"

```
1. /hotfix "description of the issue"
2. Fix is implemented on hotfix branch
3. /code-review the fix
4. Run tests
5. /release-checklist for hotfix build
6. Deploy and backport
```

### Workflow 5: "I'm approaching a milestone"

```
1. /milestone-review to check progress
2. /scope-check to see if scope has crept
3. /tech-debt to assess debt before milestone
4. /perf-profile to check performance targets
5. /team-polish for final hardening pass
6. /release-checklist when ready to ship
```

### Workflow 6: "Starting a new sprint"

```
1. /retrospective to review the last sprint
2. /sprint-plan new to create the next sprint
3. /scope-check to ensure scope is manageable
4. Start working through sprint tasks
5. /sprint-plan status to check progress mid-sprint
```

### Workflow 7: "Shipping the product"

```
1. /milestone-review for final milestone
2. /tech-debt to decide what's acceptable at launch
3. /localize for final localization pass
4. /team-polish for security and performance hardening
5. /launch-checklist for full cross-department validation
6. /team-release to coordinate the release
7. /patch-notes and /changelog for communications
8. Ship!
9. /hotfix if anything breaks post-launch
10. Post-mortem after launch stabilizes
```

---

## Tips for Getting the Most Out of the System

1. **Always start with design, then implement.** The agent system is built
   around the assumption that a feature spec exists before code is written.
   Agents reference specs constantly.

2. **Use team skills for cross-cutting features.** Don't manually coordinate
   multiple agents — let `/team-ui`, `/team-polish`, `/team-release` handle
   the orchestration.

3. **Trust the rules system.** When a rule flags something in your code, fix
   it. The rules encode best practices (no hardcoded config, input validation,
   accessibility, security).

4. **Compact proactively.** At ~65-70% context usage, compact or `/clear`.
   Don't wait until you're at the limit.

5. **Use the right tier of agent.** Don't ask `technical-director` to write
   a component. Don't ask `qa-tester` to make architecture decisions.

6. **Run `/design-review` before handing specs to engineers.** This catches
   incomplete or ambiguous requirements early, saving rework.

7. **Run `/code-review` after every major feature.** Catch architectural
   issues before they propagate.

8. **Prototype risky technical approaches first.** A day of prototyping can
   save a week of production work on an approach that doesn't work.

9. **Keep your sprint plans honest.** Use `/scope-check` regularly. Scope
   creep is the #1 killer of software projects.

10. **Document decisions with ADRs.** Future-you will thank present-you for
    recording *why* things were built the way they were.
