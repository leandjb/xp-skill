---
name: pair
description: AI-driven pair programming for one refined story. The AI writes code in tiny test-driven steps, waits for user approval each time, and never commits. Use when you are pairing on implementation of an approved story.
license: GPL-3.0
---

# Pair

## Goal

Guide the user through implementation of a single story using strict test-driven development, with the AI as driver and the user as navigator. It is called once per story and walks through the approved plan.

## When to use

- After /story has produced an approved story, acceptance criteria, and a plan.
- When the user wants to implement a story with small, verifiable TDD cycles.
- When the user wants a quality-assured coding session with clean code, DRY, and KISS.

## Conversation flow

1. **Read the approved story and plan.** Look for the story in `ref/PLAN.md` and the associated criteria and tasks. If the file or tasks are missing, ask the user to run /story first.
2. **Check the plan against the current code.** Tell the user if something no longer fits. If the plan is stale, ask which part to update before proceeding.
3. **Propose the test list.** From the acceptance criteria, propose a short list of test names and where they will be tested (endpoint, service, component, unit, manual). Mark criteria that cannot be tested automatically as manual verification. Wait for the user to approve the list.
4. **Run one TDD cycle.** By default, run a single step:
   - Write the next test from the approved list.
   - Run the test and show that it fails (red).
   - Write the minimum code needed to make it pass.
   - Run the tests again and show they pass (green).
5. **Pause after the step.** Send a micro message summarizing:
   - ✅ Passes "<test name>".
   - <what changed> (files and lines).
   - Decided: any judgment call you made.
   - Ask: continue with "<next test>" or change anything?
6. **Respect the user's batch.** If the user says "do N", run N full cycles, one after another. Do not run multiple tests then implement them all. After each cycle, pause for user approval only after N cycles if the user asked for a batch.
7. **Stop conditions.** Stop immediately if:
   - You need to make a design decision that is not covered by the plan.
   - You think of adding something the user did not ask for, even if it seems reasonable.
   - Making a test pass would require changing, skipping, or deleting a test.
   - You have made two attempts without reaching green.
   - The step exceeds ~50 lines or touches more than three files. Propose splitting it.
8. **Refactor only after tests are green and with user approval.** Do not mix refactor and new functionality. Suggest refactorings that remove duplication or improve readability.
9. **Record verified progress.** After each cycle, update the plan file with the status of the task, the command used, the expected output, and manual QA notes. Only mark a task as done after its verification command succeeded.
10. **At the end of the story**, do a final review:
   - Confirm all tasks are green.
   - Run the relevant regression or smoke checks.
   - Walk through any criteria marked manual verification together.
   - Suggest the user run /code-review if available. If not, suggest a separate review session.
   - Ask whether to commit. Never commit on your own.

### Example: first cycle

User: Continue with story "Add dark mode to the dashboard" in ref/PLAN.md.
Agent: I have read the story and plan. The test list is as follows (approved by you):

1. Theme provider loads user preference (unit).
2. Dark mode class applied when theme is dark (component).
3. Settings toggle switches theme (integration).

Next step: 1.

Agent: *Writing the test...* [shows failing test]

*Running the test...* [shows it fails]

*Implementing the minimum code...* [shows updated provider]

*Running the tests...* [all green].

Micro message:

```
✅ Passes "Theme provider loads user preference".
Changed: src/theme/context.ts (lines 12-20).
Decided: I kept the default theme as light on first render.
Continue with "2"?
```

## Rules

- **The AI writes code; the user decides and approves.**
- **One story per session; one TDD cycle by default.**
- **Strict TDD: red, green, then refactor.** Refactor only with green tests and with user approval.
- **DRY and KISS:** avoid duplication and overly complex solutions. Apply only where there is a clear signal.
- **Use RFC 2119 keywords (MUST, MUST NOT, SHOULD, SHOULD NOT, MAY) for all normative statements.**
- The AI **MUST** ask for approval before writing each test, before reading each code file, and before any refactor.
- The AI **MUST NOT** commit changes.
- The user **MAY** stop at any time.

## Testing rules

- Derive expected values from the acceptance criterion, not from recomputing the same logic under test.
- Assert behavior through the public interface; avoid asserting on internal implementation details.

## Stop conditions

Abort the current work immediately, including mid-batch, if any of the following arise:

- A design decision must be made that is not already captured in the plan.
- An unrequested feature or improvement suggests itself. Do not add it; flag it instead.
- Making a test pass would require changing, skipping, or deleting a test.
- Two independent attempts have not reached green.
- A step exceeds roughly 50 lines or touches more than three files - propose a split before continuing.

## Checks

When the checklist is green:

- Review together any item marked for **manual verification**.
- Suggest run linter or test-runner for an independent review (it uses a clean context. Do not perform the review yourself).
- If patterns repeat or the code looks messy, **suggest** a refactor; the decision remains mine. Refactor only with tests green, and never mix refactoring with new functionality in the same change.
- If a decision we made applies project-wide, propose a line to add to `AGENTS.md`.

## Output format

Use the micro message format between steps:

```markdown
✅ Passes "<test name>".
<what changed> (<file>:<line>)
Decided: <judgment call>
Continue with "<next test>"?
```

## QA status

At the end of a cycle or story, you may use this format to show QA progress:

```markdown
## Test status

- [ ] Automated checks
   - run: <command>
   - result: <passed or failed>
- [ ] Manual checks
   - <what the user must verify>
- Blockers:
   - <what is stopping progress>
```

## Commits

**Do not commit.** When a logical unit is complete and tests are green, surface it and propose a commit message that separates functionality from refactor into distinct commits. I will make the commit.
