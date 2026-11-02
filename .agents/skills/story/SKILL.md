---
name: story
description: Take one user story from a backlog, add 2-3 Given/When/Then acceptance criteria, propose at least 5-bullet minimal plan with tasks. Saves the approved definition to ref/PLAN.md. Does not write code. Use when you need to refine a single story for implementation.
license: GPL-3.0
---

# Story

## Goal

Refine a single user story into acceptance criteria and an actionable minimal plan. It never writes code; implementation is handled by /pair.

## When to use

- After /backlog has produced a vertical user story.
- When the user wants to define what "done" means for a story.
- When the user wants a minimal, verifiable plan before coding.

## Conversation flow

1. **Read the story.** If the story is missing or ambiguous, ask one clarifying question at a time with a suggested answer. Do not invent details.
2. **Read the project context.** Look for existing agreed files: AGENT.md and ref/BACKLOG.md. Use them to keep the story consistent with the agreed purpose and stack.
3. **Propose acceptance criteria.** Provide 2-3 Given/When/Then statements. They describe what the user sees or gets, not how it is implemented.
4. **If more than three criteria are needed, do not bloat.** Propose splitting the story into shorter stories in backlog format and wait for the user to add them.
5. **Propose a minimal plan.** A maximum of five short steps. Each step MUST be a checkbox with a verification condition (e.g., - [ ] `npm test` passes). Do not mention implementation details or file names.
6. **Add scope boundaries.** List what is out of scope to keep the story focused, and optionally suggest improvements with justification.
7. **Wait for approval.** Do not write files or code until the user says yes.
8. **After approval**, persist the refined story, criteria, plan, and any newly approved stories to `ref/PLAN.md`. If the file or ref/ directory does not exist, create it.

### Example: reading a story

User: Add a dark mode toggle.
Agent: I'll first check the existing context.

- Read AGENT.md, ref/BACKLOG.md, and the current code.

[Assume the user wants to add a dark mode toggle to the dashboard.]

Agent: Here are the proposed criteria:

- [ ] Given the dashboard is visible, when the user toggles dark mode, then the background switches to a dark palette and text remains readable.
- [ ] Given dark mode is enabled, when the page refreshes, then the theme stays dark.
- [ ] Given dark mode is disabled, when the user opens settings, then the toggle shows the current theme.

Plan:

- [ ] Define the theme context and provider (or equivalent).
- [ ] Add CSS or component variants for the dark palette.
- [ ] Add a settings toggle to switch the theme.
- [ ] Persist the choice (localStorage or equivalent).
- [ ] Run `npm test` and verify no regressions.

Out of scope: export of theme preferences, a dark mode for the entire application (include only the dashboard initially).

### Example: splitting a story

User: Make the app work on mobile, desktop, and tablet.
Agent: That is a large story. I'll propose splitting it:

- As a user, I want the layout to use a single column on mobile, so that I can read content comfortably on small screens.
- As a user, I want a responsive sidebar on tablets, so that I can navigate without zooming.
- As a user, I want consistent spacing and breakpoints, so that the app looks polished on all devices.

After you add these to the tracker, we can refine one at a time.

## Rules

- **Don't invent requirements.** If information is missing, ask. Do not fill gaps.
- **The simplest thing that meets the criteria.** No abstractions, layers, configuration, or patterns for the future.
- **Suggest, don't decide.** If a pattern could help, mention it under "Suggestions" only if there is a concrete signal in the code.
- **Do not write code.**
- **Use RFC 2119 keywords (MUST, MUST NOT, SHOULD, SHOULD NOT, MAY) for all normative statements.**
- The skill **MUST** ask one question at a time when clarification is needed.
- The skill **MUST NOT** create files without user approval.

## Output format

Provide a block ready to paste into the issue tracker:

```markdown
## Story

As a <actor>, I want <action>, so that <benefit>.

## Acceptance criteria

- [ ] Given <context>, when <action>, then <result>.
- [ ] ...

## Plan

- [ ] <short step>: <verification condition>
- [ ] ...

## Out of scope

- <what's left out of this story>

## Suggestions (your call)

- <optional>
```

## Saving the results

After the user approves, update `ref/PLAN.md` in the target project with the story, criteria, plan, scope, and suggestions. If `ref` does not exist, create it. Preserve unrelated content in the file. Do not overwrite existing plans for other stories.
