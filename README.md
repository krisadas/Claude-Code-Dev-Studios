<p align="center">
  <h1 align="center">Claude Code Software Studios</h1>
  <p align="center">
    Turn a single Claude Code session into a full software development studio.
    <br />
    13 agents. Structured workflows. One coordinated AI team.
  </p>
</p>

<p align="center">
  <a href="LICENSE"><img src="https://img.shields.io/badge/license-MIT-blue.svg" alt="MIT License"></a>
  <a href=".claude/agents"><img src="https://img.shields.io/badge/agents-13-blueviolet" alt="13 Agents"></a>
  <a href=".claude/skills"><img src="https://img.shields.io/badge/skills-28-green" alt="28 Skills"></a>
  <a href=".claude/hooks"><img src="https://img.shields.io/badge/hooks-8-orange" alt="8 Hooks"></a>
  <a href=".claude/rules"><img src="https://img.shields.io/badge/rules-9-red" alt="9 Rules"></a>
  <a href="https://docs.anthropic.com/en/docs/claude-code"><img src="https://img.shields.io/badge/built%20for-Claude%20Code-f5f5f5?logo=anthropic" alt="Built for Claude Code"></a>
  <a href="https://ko-fi.com/donchitos"><img src="https://img.shields.io/badge/Ko--fi-Support%20this%20project-ff5e5b?logo=ko-fi&logoColor=white" alt="Ko-fi"></a>
</p>

---

## Why This Exists

Building software solo with AI is powerful — but a single chat session has no structure. No one stops you from hardcoding secrets, skipping API specs, or writing spaghetti code. There's no QA pass, no design review, no one asking "does this actually fit the product's vision?"

**Claude Code Software Studios** solves this by giving your AI session the structure of a real studio. Instead of one general-purpose assistant, you get 13 specialized agents organized into a studio hierarchy — directors who guard the vision, department leads who own their domains, and specialists who do the hands-on work. Each agent has defined responsibilities, escalation paths, and quality gates.

The result: you still make every decision, but now you have a team that asks the right questions, catches mistakes early, and keeps your project organized from first brainstorm to launch.

---

## Table of Contents

- [What's Included](#whats-included)
- [Studio Hierarchy](#studio-hierarchy)
- [Slash Commands](#slash-commands)
- [Getting Started](#getting-started)
- [Upgrading](#upgrading)
- [Project Structure](#project-structure)
- [How It Works](#how-it-works)
- [Design Philosophy](#design-philosophy)
- [Customization](#customization)
- [Platform Support](#platform-support)
- [Community](#community)
- [License](#license)

---

## What's Included

| Category | Count | Description |
|----------|-------|-------------|
| **Agents** | 13 | Specialized subagents across product, engineering, design, QA, and ops |
| **Skills** | 28 | Slash commands for common workflows (`/start`, `/sprint-plan`, `/code-review`, `/brainstorm`, etc.) |
| **Hooks** | 8 | Automated validation on commits, pushes, asset changes, session lifecycle, agent audit, and gap detection |
| **Rules** | 9 | Path-scoped coding standards enforced when editing core, AI, UI, network code, feature specs, and more |
| **Templates** | 29 | Document templates for feature specs, ADRs, sprint plans, release notes, and more |

## Studio Hierarchy

Agents are organized into three tiers, matching how real studios operate:

```
Tier 1 — Leadership (Opus)
  technical-director    product-manager

Tier 2 — Department Leads (Sonnet)
  lead-programmer       qa-lead       release-manager

Tier 3 — Specialists (Sonnet/Haiku)
  backend-engineer      frontend-engineer     network-programmer
  devops-engineer       security-engineer     ux-designer
  performance-analyst   qa-tester
```

## Slash Commands

Type `/` in Claude Code to access all 28 skills:

**Reviews & Analysis**
`/design-review` `/code-review` `/scope-check` `/perf-profile` `/tech-debt`

**Production**
`/sprint-plan` `/milestone-review` `/estimate` `/retrospective` `/bug-report`

**Project Management**
`/start` `/project-stage-detect` `/reverse-document` `/gate-check` `/design-system`

**Release**
`/release-checklist` `/launch-checklist` `/changelog` `/patch-notes` `/hotfix`

**Creative**
`/brainstorm` `/prototype` `/onboard` `/localize`

**Team Orchestration** (coordinate multiple agents on a single feature)
`/team-ui` `/team-release` `/team-polish`

## Getting Started

### Prerequisites

- [Git](https://git-scm.com/)
- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) (`npm install -g @anthropic-ai/claude-code`)
- **Recommended**: [jq](https://jqlang.github.io/jq/) (for hook validation) and Python 3 (for JSON validation)

All hooks fail gracefully if optional tools are missing — nothing breaks, you just lose validation.

### Setup

1. **Clone or use as template**:
   ```bash
   git clone https://github.com/krisadas/Claude-Code-Software-Studios.git my-project
   cd my-project
   ```

2. **Open Claude Code** and start a session:
   ```bash
   claude
   ```

3. **Run `/start`** — the system asks where you are (no idea, vague concept,
   clear design, existing work) and guides you to the right workflow. No assumptions.

   Or jump directly to a specific skill if you already know what you need:
   - `/brainstorm` — explore product ideas from scratch
   - `/design-system <feature>` — spec out a feature if you already know what to build
   - `/project-stage-detect` — analyze an existing project

## Upgrading

Already using an older version of this template? See [UPGRADING.md](UPGRADING.md)
for step-by-step migration instructions, a breakdown of what changed between
versions, and which files are safe to overwrite vs. which need a manual merge.

## Project Structure

```
CLAUDE.md                           # Master configuration
.claude/
  settings.json                     # Hooks, permissions, safety rules
  agents/                           # 13 agent definitions (markdown + YAML frontmatter)
  skills/                           # 28 slash commands (subdirectory per skill)
  hooks/                            # 8 hook scripts (bash, cross-platform)
  rules/                            # 9 path-scoped coding standards
  docs/
    quick-start.md                  # Detailed usage guide
    agent-roster.md                 # Full agent table with domains
    agent-coordination-map.md       # Delegation and escalation paths
    setup-requirements.md           # Prerequisites and platform notes
    templates/                      # Document templates
src/                                # Application source code
assets/                             # Static assets and data files
design/                             # Feature specs and product documentation
docs/                               # Technical documentation and ADRs
tests/                              # Test suites
tools/                              # Build and pipeline tools
prototypes/                         # Throwaway prototypes (isolated from src/)
production/                         # Sprint plans, milestones, release tracking
```

## How It Works

### Agent Coordination

Agents follow a structured delegation model:

1. **Vertical delegation** — leadership delegates to leads, leads delegate to specialists
2. **Horizontal consultation** — same-tier agents can consult each other but can't make binding cross-domain decisions
3. **Conflict resolution** — disagreements escalate up to the shared parent (`product-manager` for scope/design, `technical-director` for technical)
4. **Change propagation** — cross-department changes are coordinated by `product-manager`
5. **Domain boundaries** — agents don't modify files outside their domain without explicit delegation

### Collaborative, Not Autonomous

This is **not** an auto-pilot system. Every agent follows a strict collaboration protocol:

1. **Ask** — agents ask questions before proposing solutions
2. **Present options** — agents show 2-4 options with pros/cons
3. **You decide** — the user always makes the call
4. **Draft** — agents show work before finalizing
5. **Approve** — nothing gets written without your sign-off

You stay in control. The agents provide structure and expertise, not autonomy.

### Automated Safety

**Hooks** run automatically on every session:

| Hook | Trigger | What It Does |
|------|---------|--------------|
| `validate-commit.sh` | `git commit` | Checks for hardcoded values, TODO format, JSON validity, design doc sections |
| `validate-push.sh` | `git push` | Warns on pushes to protected branches |
| `validate-assets.sh` | File writes in `assets/` | Validates naming conventions and JSON structure |
| `session-start.sh` | Session open | Loads sprint context and recent git activity |
| `detect-gaps.sh` | Session open | Detects fresh projects (suggests `/start`) and missing documentation when code/prototypes exist |
| `pre-compact.sh` | Context compression | Preserves session progress notes |
| `session-stop.sh` | Session close | Logs accomplishments |
| `log-agent.sh` | Agent spawned | Audit trail of all subagent invocations |

**Permission rules** in `settings.json` auto-allow safe operations (git status, test runs) and block dangerous ones (force push, `rm -rf`, reading `.env` files).

### Path-Scoped Rules

Coding standards are automatically enforced based on file location:

| Path | Enforces |
|------|----------|
| `src/core/**` | Dependency direction, graceful degradation, thread safety, no global mutable state |
| `src/ai/**` | Model parameter safety, PII protection, rate limiting, audit trail |
| `src/networking/**` | TLS, rate limiting, auth tokens never logged, API versioning |
| `src/ui/**` | No app state ownership, i18n strings, accessibility (WCAG 2.1 AA) |
| `design/features/**` | Required 8 sections, data models, acceptance criteria |
| `tests/**` | Test naming, coverage requirements, fixture patterns |
| `prototypes/**` | Relaxed standards, README required, hypothesis documented |
| `assets/data/**`, `config/**` | Schema validation, no secrets, change tracking |

## Design Philosophy

This template is grounded in professional software development practices:

- **Jobs-to-be-Done** — Problem-first thinking before solution design
- **API-First Design** — Contract-first development with OpenAPI specs
- **Verification-Driven Development** — Tests first, then implementation
- **Defense in Depth** — Security at every layer (input validation, auth, secrets management)
- **Collaborative Protocol** — Question → Options → Decision → Draft → Approval

## Customization

This is a **template**, not a locked framework. Everything is meant to be customized:

- **Add/remove agents** — delete agent files you don't need, add new ones for your domains
- **Edit agent prompts** — tune agent behavior, add project-specific knowledge
- **Modify skills** — adjust workflows to match your team's process
- **Add rules** — create new path-scoped rules for your project's directory structure
- **Tune hooks** — adjust validation strictness, add new checks
- **Configure your stack** — update `.claude/docs/technical-preferences.md` with your chosen frameworks

## Platform Support

Tested on **Windows 10** with Git Bash. All hooks use POSIX-compatible patterns (`grep -E`, not `grep -P`) and include fallbacks for missing tools. Works on macOS and Linux without modification.

## Community

- **Discussions** — [GitHub Discussions](https://github.com/krisadas/Claude-Code-Software-Studios/discussions) for questions, ideas, and showcasing what you've built
- **Issues** — [Bug reports and feature requests](https://github.com/krisadas/Claude-Code-Software-Studios/issues)

---

*This project is under active development. The agent architecture, skills, and coordination system are solid and usable today — but there's more coming.*

## License

MIT License. See [LICENSE](LICENSE) for details.
