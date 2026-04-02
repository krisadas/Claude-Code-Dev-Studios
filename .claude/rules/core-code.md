---
paths:
  - "src/core/**"
---

# Core Code Rules

- Core modules must NEVER depend on feature/application code (strict dependency direction: core ← features)
- All public APIs must have usage examples in their doc comments
- Changes to public interfaces require a deprecation period and a migration guide
- Every public module must support graceful degradation — partial failure must not crash the process
- Profile before AND after every optimization — document the measured numbers
- Use deterministic resource cleanup (RAII or explicit close/dispose patterns)
- All shared utilities must be thread-safe OR explicitly documented as single-thread-only
- No global mutable state in core modules — use dependency injection
