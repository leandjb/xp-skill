# Proposal

## Why

Provide a small, approachable AI-assisted Extreme Programming (XP) workflow that takes a software problem through clarification, user stories, a minimal implementation plan, and quality-assured pair programming. Persistent handoff documents and practical examples keep the workflow comfortable to use while leaving the user in control.

## What Changes

- Create `backlog`, `story`, and `pair` under `.agents/skills/`, using standard [Agent Skills](https://agentskills.io/specification) frontmatter and professional English.
- Preserve the [inspiration skills'](https://github.com/nschurmann/hola-ai/tree/main/skills) workflow, explanations, scope boundaries, approval gates, stop rules, and prohibition on AI-created commits, except for the explicitly requested persistence changes. Omit client-specific frontmatter extensions as previously agreed.
- `/backlog` MUST ask focused questions when requirements are unclear, one question at a time with a suggested answer. It MUST NOT invent requirements. It MUST produce a small list of vertical user stories, treating 10-15 stories as a maximum rather than a minimum. After user approval, it MUST save the list to `ref/BACKLOG.md` and agreed project context to `AGENT.md` in the target project.
- `/story` MUST refine one selected story into 2-3 Given/When/Then acceptance criteria and a plan of at most five steps. After approval, it MUST save the story, criteria, scope, and related verifiable tasks to the target project's `ref/PLAN.md`. It MUST NOT implement code.
- `/pair` MUST read the approved plan, obtain approval for the test list, and implement tasks through observable red -> green -> approved refactoring cycles. It MUST apply KISS, DRY, and clean-code practices without speculative abstractions.
- `/pair` MUST retain the original small-step and stop rules: one story per session and one TDD cycle per approval by default, with explicit user-requested batches supported. Repeating the workflow implements the full backlog without removing user control.
- `/pair` MUST record verified task progress, distinguish automated checks from manual QA, and report blockers honestly. It MUST NOT claim unexecuted checks passed or treat its own checks as independent review.
- Each skill MUST include practical examples of its conversation and output, covering clarification, user-story generation, plan creation, and pairing feedback across the workflow.
- Normative instructions and tasks MUST use keywords with their [RFC 2119](https://www.rfc-editor.org/rfc/rfc2119.html) meanings.
- File updates MUST preserve unrelated content and existing work; conflicting instructions or unclear requirements MUST be resolved with the user. Skills MUST explicitly read the requested singular `AGENT.md`, rather than relying on client auto-discovery.

## Capabilities

### New Capabilities

None. Retain the instruction-content-only scope and existing `skip_specs: true` declaration. This change documents agent workflows; it does not add an application runtime.

### Modified Capabilities

None.

## Impact

- Implementation delivers three skill files: `.agents/skills/backlog/SKILL.md`, `.agents/skills/story/SKILL.md`, and `.agents/skills/pair/SKILL.md`.
- During later use in a target project, the skills create or maintain `ref/BACKLOG.md`, `AGENT.md`, and `ref/PLAN.md`. These are target-project outputs, not additional files to generate while updating this change.
- The design defines file formats and handoffs; implementation tasks cover per-skill verification and an end-to-end workflow walkthrough.
- No application code, API, or dependency changes. No application backlog generation or feature implementation during this planning update.