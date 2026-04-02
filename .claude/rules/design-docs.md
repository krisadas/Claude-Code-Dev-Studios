---
paths:
  - "design/features/**"
---

# Design Document Rules

- Every design document MUST contain these 8 sections: Overview, User Story, Detailed Requirements, Data Models, Edge Cases, Dependencies, Configuration, Acceptance Criteria
- Requirements must be unambiguous — "it should feel fast" is not a valid specification; define measurable criteria
- Edge cases must explicitly state what happens, not just "handle gracefully"
- Dependencies must be bidirectional — if feature A depends on B, B's doc must mention A
- Configuration values must specify safe ranges and what behavior they affect
- Acceptance criteria must be testable — a QA engineer must be able to verify pass/fail
- API contracts must be defined before implementation begins
- Design documents MUST be written incrementally: create skeleton first, then fill
  each section one at a time with user approval between sections. Write each
  approved section to the file immediately to persist decisions and manage context
