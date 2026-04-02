# Directory Structure

```text
/
├── CLAUDE.md                    # Master configuration
├── .claude/                     # Agent definitions, skills, hooks, rules, docs
├── src/                         # Application source code (core, api, ai, networking, ui, tools)
├── assets/                      # Static assets and data files (images, fonts, data, config)
├── design/                      # Feature specs and product documentation (features, product-concept)
├── docs/                        # Technical documentation (architecture, api, postmortems)
├── tests/                       # Test suites (unit, integration, performance, e2e)
├── tools/                       # Build and pipeline tools (ci, build, scripts)
├── prototypes/                  # Throwaway prototypes (isolated from src/)
└── production/                  # Production management (sprints, milestones, releases)
    ├── session-state/           # Ephemeral session state (active.md — gitignored)
    └── session-logs/            # Session audit trail (gitignored)
```
