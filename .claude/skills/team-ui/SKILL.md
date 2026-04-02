---
name: team-ui
description: "Orchestrate the UI team: coordinates ux-designer and frontend-engineer to design, implement, and polish a user interface feature from wireframe to final."
argument-hint: "[UI feature description]"
user-invocable: true
---

When this skill is invoked, orchestrate the UI team through a structured pipeline.

**Decision Points:** At each phase transition, use `AskUserQuestion` to present
the user with the subagent's proposals as selectable options. Write the agent's
full analysis in conversation, then capture the decision with concise labels.
The user must approve before moving to the next phase.

## Team Composition
- **ux-designer** — User flows, wireframes, accessibility, interaction design
- **frontend-engineer** — Component implementation, state management, API integration

## How to Delegate

Use the Task tool to spawn each team member as a subagent:
- `subagent_type: ux-designer` — User flows, wireframes, accessibility, interaction design
- `subagent_type: frontend-engineer` — Component implementation, state management, API integration

Always provide full context in each agent's prompt (feature requirements, existing UI patterns, platform targets).

## Pipeline

### Phase 1: UX Design
Delegate to **ux-designer**:
- Define the user flow (entry points, states, exit points)
- Create wireframes for each screen/state
- Specify interaction patterns: how does keyboard and mouse navigate this?
- Define accessibility requirements: text sizes, contrast, keyboard focus
- Identify data the UI needs to display (what state does it read?)
- Output: UX spec with wireframes and interaction map

### Phase 2: Implementation
Delegate to **frontend-engineer**:
- Implement the UI following the UX spec
- UI must NEVER own or modify application state — display only, events for actions
- All text through i18n system — no hardcoded strings
- Support keyboard accessibility for all interactive elements
- Handle loading, error, and empty states explicitly
- Wire up data binding to application state / API
- Output: implemented UI feature

### Phase 3: Review (parallel)
Delegate in parallel:
- **ux-designer**: Verify implementation matches wireframes and interaction spec. Test keyboard-only navigation. Check accessibility (contrast, focus indicators, ARIA).
- **frontend-engineer** (self-review): Verify performance (no unnecessary re-renders), bundle impact, cross-browser behavior.

### Phase 4: Polish
- Address review feedback
- Verify animations respect `prefers-reduced-motion`
- Test at all supported viewport sizes and breakpoints
- Run Lighthouse accessibility audit

## Output
A summary report covering: UX spec status, implementation status, accessibility compliance, and any outstanding issues.
