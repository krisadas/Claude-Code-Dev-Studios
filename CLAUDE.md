# Claude Code Software Studios -- Software Studio Agent Architecture

Software development managed through 13 coordinated Claude Code subagents.
Each agent owns a specific domain, enforcing separation of concerns and quality.

## Technology Stack

### Frontend
- **Frameworks**: React, Next.js, Angular
- **Language**: TypeScript (preferred), JavaScript

### Backend
- **Runtimes / Frameworks**: Node.js, Golang, Python
- **Common patterns**: REST APIs, GraphQL, gRPC

### Shared
- **Version Control**: Git with trunk-based development
- **Database**: [SPECIFY per project: PostgreSQL / MySQL / MongoDB / Redis]
- **Deployment**: [SPECIFY per project: Docker / Kubernetes / serverless]

## Project Structure

@.claude/docs/directory-structure.md

## Technical Preferences

@.claude/docs/technical-preferences.md

## Coordination Rules

@.claude/docs/coordination-rules.md

## Collaboration Protocol

**User-driven collaboration, not autonomous execution.**
Every task follows: **Question -> Options -> Decision -> Draft -> Approval**

- Agents MUST ask "May I write this to [filepath]?" before using Write/Edit tools
- Agents MUST show drafts or summaries before requesting approval
- Multi-file changes require explicit approval for the full changeset
- No commits without user instruction

See `docs/COLLABORATIVE-DESIGN-PRINCIPLE.md` for full protocol and examples.

> **First session?** If the project has no stack configured and no product concept,
> run `/start` to begin the guided onboarding flow.

## Coding Standards

@.claude/docs/coding-standards.md

## Context Management

@.claude/docs/context-management.md
