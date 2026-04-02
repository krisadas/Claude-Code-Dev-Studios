---
name: brainstorm
description: "Guided product/feature ideation — from zero idea to a structured product concept document. Uses professional product discovery techniques, user need frameworks, and structured exploration."
argument-hint: "[domain or problem hint, or 'open' for fully open brainstorm]"
user-invocable: true
---

When this skill is invoked:

1. **Parse the argument** for an optional domain/problem hint (e.g., `task management`,
   `developer tooling`, `e-commerce`). If `open` or no argument, start from scratch.

2. **Check for existing concept work**:
   - Read `design/product-concept.md` if it exists (resume, don't restart)
   - Read `design/product-pillars.md` if it exists (build on established pillars)

3. **Run through ideation phases** interactively, asking the user questions at
   each phase. Do NOT generate everything silently — the goal is **collaborative
   exploration** where the AI acts as a product facilitator, not a replacement
   for the human's vision.

   **Use `AskUserQuestion`** at key decision points:
   - Problem space questions (domain, users, pain points)
   - Concept selection ("Which 2-3 directions resonate?") after presenting options
   - Direction choices ("Develop further, validate, or prototype?")
   Write full analysis in conversation text first, then use `AskUserQuestion`
   to capture the decision with concise labels.

   Professional product discovery principles:
   - Start with the problem, not the solution
   - Distinguish user needs from user requests
   - Use constraints as creative fuel
   - Time-box each phase — keep momentum, don't over-deliberate early
   - Validate assumptions before committing to a direction

---

### Phase 1: Problem Discovery

Start by understanding the problem space and users. Ask these questions
conversationally (not as a checklist):

**Problem anchors**:
- What problem are you trying to solve? Who has this problem?
- How do people solve this problem today? What's frustrating about current solutions?
- Is this a problem you've personally experienced, or one you've observed?

**User profile**:
- Who is the primary user? (role, context, technical level)
- What does success look like for that user?
- What would make them switch away from existing solutions?

**Practical constraints** (shape the sandbox before ideating):
- Solo developer or team? What skills are available?
- Timeline and scope: weekend project, MVP, full product?
- Any platform constraints? (web, mobile, desktop, API only?)
- Existing codebase or greenfield?

**Synthesize** the answers into a **Problem Brief** — a 3-5 sentence
summary of the user, their pain, and the constraints. Read the brief back
and confirm it captures their intent.

---

### Phase 2: Concept Generation

Using the problem brief, generate **3 distinct product directions** that each
take a different approach. Use these techniques:

**Technique 1: Core Action First**
Start with the primary user action (automate, visualize, collaborate, analyze,
integrate) and build outward. The core action IS the product.

**Technique 2: Constraint Flip**
Take a common constraint and make it a feature. "Too complex" → radically simple.
"Too slow" → real-time first. "Too generic" → hyper-specialized.

**Technique 3: Jobs-to-be-Done**
Focus on the outcome the user wants to achieve ("when I..., I want to..., so that...").
Design the smallest product that delivers that outcome reliably.

For each concept, present:
- **Working Name**
- **One-Line Value Proposition** (must pass the "so what?" test)
- **Core User Action** (the primary thing users do)
- **Key Differentiator** (what makes this different from existing solutions)
- **Primary User Segment** (who will love this most)
- **Estimated Scope** (small / medium / large)
- **Biggest Risk** (the hardest unanswered question)

Present all three. Ask the user to pick one, combine elements, or request
new directions.

---

### Phase 3: Core Flow Design

For the chosen direction, build the core user flow — the beating heart of
the product. If this flow isn't compelling, no features will save it.

**Onboarding moment** (first minute):
- How does a new user get value within 60 seconds?
- What's the "aha moment" — when does the user first feel the value?

**Core loop** (daily/weekly use):
- What does the user do most often?
- How does it get better with repeated use?
- Where is the natural stopping and returning point?

**Growth and retention**:
- Why does a user keep coming back?
- How does the product evolve as the user becomes an expert?
- What would make a user recommend this to a colleague?

---

### Phase 4: Product Pillars and Anti-Features

Define **3-5 product pillars** — design principles that guide every decision:
- Each pillar has a **name** and **one-sentence definition**
- Each pillar has a **decision test**: "If we're choosing between X and Y, this pillar says we choose __"

Then define **3+ anti-features** (what this product is NOT):
- Anti-features prevent scope creep and keep the vision sharp
- Frame as: "We will NOT build [thing] because it dilutes [pillar]"

---

### Phase 5: MVP Definition and Feasibility

Ground the concept in reality:

- **Stack recommendation** (based on the concept, team skills, and platform targets)
- **MVP scope**: What's the absolute minimum that delivers the core value?
- **Must-have vs. nice-to-have**: Force prioritization of features
- **Biggest risks**: Technical risk, adoption risk, competition risk
- **Scope tiers**: Full vision vs. what ships if time runs out

---

4. **Generate the product concept document** and ask: "May I write this to
   `design/product-concept.md`?" Fill in ALL sections from the ideation phases.

5. **Suggest next steps**:
   - "Run `/design-review design/product-concept.md` to validate completeness"
   - "Use `/design-system <feature-name>` to spec out individual features"
   - "Prototype the core flow with `/prototype <core-concept>`"
   - "Plan the first sprint with `/sprint-plan new`"

6. **Output a summary** with the chosen concept's value proposition, pillars,
   primary user segment, stack recommendation, biggest risk, and file path.
