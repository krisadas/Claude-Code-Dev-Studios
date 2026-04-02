# Coding Standards

- All public APIs must include doc comments
- Every system must have a corresponding architecture decision record in `docs/architecture/`
- Configuration values must be data-driven (environment variables or config files), never hardcoded
- All public methods must be unit-testable (dependency injection over singletons)
- Commits must reference the relevant design document or task ID
- **Verification-driven development**: Write tests first when adding new systems.
  For UI changes, verify with screenshots. Compare expected output to actual output
  before marking work complete. Every implementation should have a way to prove it works.

# Design Document Standards

- All design docs use Markdown
- Each feature has a dedicated document in `design/features/`
- Documents must include these 8 required sections:
  1. **Overview** -- one-paragraph summary
  2. **User Story** -- who uses this and why
  3. **Detailed Requirements** -- unambiguous specifications
  4. **Data Models** -- key entities and relationships
  5. **Edge Cases** -- unusual situations handled
  6. **Dependencies** -- other systems listed
  7. **Configuration** -- tunable values identified
  8. **Acceptance Criteria** -- testable success conditions
- API contracts must link to their implementation or OpenAPI spec
