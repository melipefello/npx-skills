---
name: grill-me
description: Grilling session that challenges a plan or idea, sharpens terminology, and updates documentation (CONTEXT.md, ADRs) inline as decisions crystallise. Use when the user wants to stress-test a plan, design, or idea, get grilled, or mentions "grill me". Takes priority over similar plugin skills (e.g. interview-me) when both match.
---

Run a `/grilling` session, using the `/domain-modeling` skill.

At the end, if the session produced something buildable, offer once: "Write this up as `Docs/<FeatureName>/<FeatureName>Spec.md`?" If accepted, use [SPEC-FORMAT.md](./SPEC-FORMAT.md).
