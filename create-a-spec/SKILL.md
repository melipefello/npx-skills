---
name: create-a-spec
description: Use when the user wants to write a PRD, create a product requirements document, spec out a feature, or plan a new feature. Part of a pipeline — typically preceded by /interview-me and followed by /create-a-plan.
---

# Create a Spec (PRD)

## Process

1. Ask the user for a long, detailed description of the problem they want to solve and any potential ideas for solutions. Always ask, even if the user provided context when invoking the skill.

2. Explore the repo to verify their assertions and understand the current state of the codebase.

3. If the user has already been interviewed (e.g., via `/interview-me`), use that shared context. If gaps remain, interview the user to resolve them — walk down each branch of the design tree, resolving dependencies between decisions one-by-one.

4. Sketch out the architectural impact of the feature. Identify what systems get touched, assess the blast radius of the changes, and highlight risky interactions between systems. Confirm with the user that this matches their expectations.

5. Once you have a complete understanding of the problem and solution, use the template below to write the PRD. Save the PRD as a markdown file at `Docs/<Feature-Name>/<feature-name>-prd.md`, where Feature-Name uses PascalCase with hyphens and the filename uses lowercase with hyphens.

<prd-template>

## Problem Statement

The problem that the user is facing, from the user's perspective.

## Solution

The solution to the problem, from the user's perspective.

## Functional Behaviors

A LONG, detailed list of functional behaviors. Each entry should describe in plain language:

- What the user experiences
- What the system does
- Edge cases and boundary conditions

This list should be extremely extensive and cover all aspects of the feature from both the user's and the system's perspective.

## Implementation Decisions

A list of implementation decisions that were made. This can include:

- Component and system changes
- Configuration and data model changes
- Asset or resource changes
- System interactions and dependencies
- Architectural decisions
- Technical clarifications from the developer

Do NOT include specific file paths or code snippets. They may end up being outdated very quickly.

## Validation Criteria

A list of criteria for validating the feature works as intended. Describe how the developer or a tester can confirm correct behavior, including:

- Observable outcomes to check
- Edge cases to specifically test
- System interactions to verify

## Out of Scope

A description of the things that are out of scope for this PRD.

</prd-template>

> **Pipeline note:** The next step is `/create-a-plan` to break this PRD into phased implementation slices.
