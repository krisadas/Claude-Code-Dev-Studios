# Path-Specific Rules

Rules in `.claude/rules/` are automatically enforced when editing files in matching paths:

| Rule File | Path Pattern | Enforces |
| ---- | ---- | ---- |
| `core-code.md` | `src/core/**` | Dependency direction, graceful degradation, thread safety, no global mutable state |
| `ai-code.md` | `src/ai/**` | Model parameter safety, PII protection, rate limiting, audit trail |
| `network-code.md` | `src/networking/**` | TLS required, rate limiting, auth tokens never logged, API versioning |
| `ui-code.md` | `src/ui/**` | No app state ownership, i18n strings, WCAG 2.1 AA accessibility |
| `design-docs.md` | `design/features/**` | Required 8 sections, data models, acceptance criteria |
| `data-files.md` | `assets/data/**`, `config/**` | JSON validity, naming conventions, schema rules, no secrets |
| `test-standards.md` | `tests/**` | Test naming, coverage requirements, fixture patterns |
| `prototype-code.md` | `prototypes/**` | Relaxed standards, README required, hypothesis documented |
