# Spec Format

Synthesize the spec from what the grilling session already established — do NOT start a new interview. Save it to `Docs/<FeatureName>/<FeatureName>Spec.md`.

<spec-template>

## Problem Statement

The problem that the user is facing, from the user's perspective.

## Solution

The solution to the problem, from the user's perspective.

## Behaviors

A LONG, numbered list of concrete behaviors. Each behavior should describe what the player/user can do or what the system should do.

<behavior-examples>
1. The player can dash in the direction they are moving
2. The system cancels the dash if the player takes damage mid-animation
3. The player sees a stamina bar decrease when dashing
</behavior-examples>

This list of behaviors should be extremely extensive and cover all aspects of the feature.

## Implementation Decisions

A list of implementation decisions that were made. This can include:

- The modules that will be built/modified
- Technical clarifications from the developer
- Architectural decisions
- Schema changes
- Specific interactions

Do NOT include specific file paths or code snippets. They may end up being outdated very quickly.

Exception: if a prototype produced a snippet that encodes a decision more precisely than prose can (state machine, reducer, schema, type shape), inline it within the relevant decision and note briefly that it came from a prototype. Trim to the decision-rich parts — not a working demo, just the important bits.

## Validation Criteria

A description of how to verify the feature works correctly. If the project has automated tests, name the tests to add or run. For anything tests can't cover (or if the project has no tests), describe:

- What the human should observe when running the app
- What behavior confirms the feature is working
- Edge cases to manually check
- What error-free logs should look like

## Out of Scope

A description of the things that are out of scope for this spec.

## Further Notes

Any further notes about the feature.

</spec-template>
