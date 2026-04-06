---
name: create-a-plan
description: Use when the user wants to break down a PRD into implementation phases, create a phased plan, or turn a spec into vertical slices using tracer-bullet methodology. Part of a pipeline — typically preceded by /create-a-spec.
---

# Plan from PRD

Break a PRD into a phased implementation plan using vertical slices (tracer bullets). Output is a Markdown file co-located with the source PRD.

## Process

### 1. Confirm the PRD is in context

The PRD should already be in the conversation (typically from the `/create-a-spec` skill). If it isn't, ask the user to paste it or point you to the file. The PRD is expected to follow the `/create-a-spec` skill template (Problem Statement, Solution, Functional Behaviors, Implementation Decisions, Validation Criteria).

### 2. Explore the codebase (conditional)

If the codebase has already been explored in this conversation (e.g., during the spec phase), use that existing context. Otherwise, do a focused exploration to understand the current architecture, existing patterns, system responsibilities, and integration points.

### 3. Identify durable architectural decisions

Before slicing, identify high-level decisions that are unlikely to change throughout implementation. Derive these from the codebase context already gathered. Only include sections that are actually relevant to the feature — omit any that don't apply:

- **Core data models** — data structures, schemas, configuration formats, enums
- **System responsibilities** — which systems/modules own what behavior, authority boundaries
- **Event / messaging patterns** — how systems communicate (events, pub/sub, message bus, direct references, APIs)
- **Component / module structure** — composition approach, key abstractions, hierarchy decisions
- **Third-party dependencies** — libraries, plugins, external services

These go in the plan header so every phase can reference them.

### 4. Draft vertical slices

Break the PRD into **tracer bullet** phases. Each phase is a thin vertical slice that cuts through ALL relevant architectural layers end-to-end, NOT a horizontal slice of one layer. Identify the project's architectural layers from the codebase exploration — every project has its own layer stack.

<vertical-slice-rules>
- Each slice delivers one narrow but COMPLETE user-facing interaction, demoable and verifiable on its own
- Aim for one user-facing interaction per phase
- Prefer many thin slices over few thick ones
- Do NOT include specific method names, variable names, or line-level implementation details
- DO include durable architectural names: type names, system/module names, event names — these are anchors referenced across the project
</vertical-slice-rules>

### 5. Quiz the user

Present the proposed breakdown as a numbered list. For each phase show:

- **Title**: short descriptive name
- **Functional Behaviors covered**: which functional behaviors from the PRD this phase delivers

Ask the user:

- Does the granularity feel right? (too coarse / too fine)
- Should any phases be merged or split further?

Iterate until the user approves the breakdown.

### 6. Write the plan file

Save the plan co-located with the source PRD at `Docs/<Feature-Name>/<feature-name>-plan.md`, using the same Feature-Name directory as the PRD. Use the template below.

<plan-template>
# Plan: <Feature Name>

> Source PRD: `<relative path to PRD file>`

## Architectural decisions

Durable decisions that apply across all phases:

- **Core data models**: ...
- **System responsibilities**: ...
- **Event / messaging patterns**: ...
- **Component / module structure**: ...
- **Third-party dependencies**: ...

(Only include sections relevant to this feature.)

---

## Phase 1: <Title>

**Functional Behaviors**: <list of functional behaviors from the PRD this phase delivers>

### What to build

A concise description of this vertical slice. Describe the end-to-end user-facing interaction, not layer-by-layer implementation.

### How to verify

Observable outcomes to confirm this phase works correctly:

- [ ] <what to observe when running/testing>
- [ ] <edge case to test>
- [ ] <system interaction to verify>

---

## Phase 2: <Title>

**Functional Behaviors**: <list from PRD>

### What to build

...

### How to verify

- [ ] ...

<!-- Repeat for each phase -->
</plan-template>
