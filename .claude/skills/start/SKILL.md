---
name: start
description: "First-time onboarding — asks where you are, then guides you to the right workflow. No assumptions."
argument-hint: "[no arguments]"
user-invocable: true
---

# Guided Onboarding

This skill is the entry point for new users. It does NOT assume you have a product
idea, a stack preference, or any prior experience. It asks first, then routes
you to the right workflow.

---

## Workflow

### 1. Detect Project State (Silent)

Before asking anything, silently gather context to tailor your guidance.

Check:
- **Stack configured?** Read `.claude/docs/technical-preferences.md`. If the
  Stack section contains `[TO BE CONFIGURED]`, the stack is not set.
- **Product concept exists?** Check for `design/product-concept.md`.
- **Feature specs exist?** Count markdown files in `design/features/`.
- **Source code exists?** Glob for source files in `src/`.
- **Prototypes exist?** Check for subdirectories in `prototypes/`.
- **Production artifacts?** Check for files in `production/sprints/` or
  `production/milestones/`.

Store these findings internally. Use them to validate the user's
self-assessment and tailor follow-up recommendations.

---

### 2. Ask Where the User Is

> **Welcome to Claude Code Software Studios!**
>
> Before I suggest anything, I'd like to understand where you're starting from.
> Where are you at with your product idea right now?
>
> **A) No idea yet** — I don't have a product or feature concept at all.
> I want to explore and figure out what to build.
>
> **B) Vague idea** — I have a rough problem or domain in mind
> (e.g., "something with task management" or "a developer tool") but nothing concrete.
>
> **C) Clear concept** — I know what I want to build — the core idea and target users —
> but haven't formalized it into documents yet.
>
> **D) Existing work** — I already have specs, prototypes, code, or significant
> planning done. I want to organize or continue the work.

Wait for the user's answer before proceeding.

---

### 3. Route Based on Answer

#### If A: No idea yet

1. Acknowledge that starting from zero is completely fine
2. Briefly explain what `/brainstorm` does (guided product ideation using
   problem discovery, user needs, and MVP definition)
3. Recommend `/brainstorm open` as the next step
4. Show the recommended path:
   - `/brainstorm` — discover your product concept
   - `/design-system <feature>` — spec out individual features
   - `/prototype` — validate the core concept
   - `/sprint-plan` — plan the first sprint

#### If B: Vague idea

1. Ask them to share their vague idea — even a few words is enough
2. Validate it as a starting point
3. Recommend `/brainstorm [their hint]` to develop it
4. Show the recommended path:
   - `/brainstorm [hint]` — develop into a full product concept
   - `/design-system <feature>` — spec out features
   - `/prototype` — validate the core flow
   - `/sprint-plan` — plan the first sprint

#### If C: Clear concept

1. Ask 2-3 follow-up questions:
   - What's the core problem you're solving? (one sentence)
   - Who is the primary user?
   - What tech stack do you have in mind, or need help choosing?
2. Offer two paths:
   - **Formalize first**: Run `/brainstorm` to structure into a product concept doc
   - **Jump to specs**: Go straight to `/design-system <feature>` for each feature
3. Show the recommended path:
   - `/brainstorm` or `/design-system` (their pick)
   - `/design-review` — validate the spec
   - `/architecture-decision` — make first technical decisions
   - `/sprint-plan` — plan the first sprint

#### If D: Existing work

1. Share what you found in Step 1:
   - "I can see you have [X source files / Y feature specs / Z prototypes]..."
   - "Your stack is [configured as X / not yet configured]..."
2. Recommend `/project-stage-detect` for a full analysis
3. If the stack isn't configured, note that should come first
4. Show the recommended path:
   - `/project-stage-detect` — full gap analysis
   - `/design-system <feature>` — if specs are incomplete
   - `/gate-check` — validate readiness for next phase
   - `/sprint-plan` — organize the work

---

### 4. Confirm Before Proceeding

After presenting the recommended path, ask which step they'd like first.
Never auto-run the next skill.

> "Would you like to start with [recommended first step], or would you prefer
> to do something else first?"

---

### 5. Hand Off

When the user chooses their next step, the `/start` skill's job is done
once they have a clear next action.

---

## Edge Cases

- **User picks D but project is empty**: Gently redirect — "It looks like the
  project is a fresh template with no artifacts yet. Would Path A or B be a better fit?"
- **User picks A but project has code**: Mention what you found — "I noticed
  there's already code in `src/`. Did you mean to pick D (existing work)?"
- **User is returning (stack configured, concept exists)**: Skip onboarding —
  "It looks like you're already set up! Your stack is [X] and you have a product concept.
  Want to pick up where you left off? Try `/sprint-plan` or just tell me what you'd like to work on."
