---
name: backlog
description: Split a software idea or feature into a set of vertical user stories. Asks clarifying questions, surfaces expensive-to-change decisions, and saves the agreed backlog to ref/BACKLOG.md and project context to AGENT.md after approval.
license: GPL-3.0
---

# Backlog

## Goal

Turn a vague idea or discussed feature into a vertical list of user stories. It follows the planning game: clarify, propose, agree, and persist the results. It deliberately leaves implementation to the story and pair skills.

## When to use

- At the start of a new feature, module, or project.
- When the user has a problem or outcome in mind but no concrete list of work.
- When the user wants a lightweight list of stories before design or implementation.

## Conversation flow

1. **Listen first.** Ask only if something is unclear. One question at a time, with a suggested answer clearly marked (suggestion). Do not infer or fill gaps.
2. **If it is a new project, agree on the stack only.** Keep the conversation to the technology stack; do not discuss architecture or implementation details.
3. **Ask about expensive-to-change decisions.** Only ask; do not design. Options:
   - Multi-tenant or multi-organization?
   - Permissions or roles model?
   - Offline or sync?
   - Multi-language or multi-currency?
4. **Propose a backlog.** Follow the rules below to generate a small list of vertical stories. The first story is the smallest end-to-end piece (walking skeleton). Keep the list to 10-15 stories; fewer is fine.
5. **After approval**, persist the agreed context and stories. The skill writes:
   - `ref/BACKLOG.md` with the context, stack, expensive decisions, and the full story list.
   - `AGENT.md` with the agreed project purpose, users, stack, constraints, expensive decisions, and workflow locations.
6. **Offer to refine a story next.** Do not automatically invoke another skill.

### Example: clarifying an unclear request

User: I want a way for users to pay for the app.
Agent: To write good stories, I need a bit more context.

- **Question:** Is the payment meant for a one-time purchase, a subscription, or both?
  (Suggestion: user says "subscription" or "recurrent")
- **Question:** Should the payment support multiple providers (for example: debit card, credit card, stripe, mercadopago, and PayPal) or just one?
  (Suggestion: "both, but credit card first")
- **Question:** Who is the actor making the payment?
  (Suggestion: "the buyer")

After these questions, you can propose stories such as:

1. As a buyer, I want to choose a subscription plan, so that I can pay for ongoing access.
2. As a buyer, I want to pay with a credit card, so that I can start using the service.
3. As a buyer, I want to upgrade or downgrade my plan, so that I can match my usage.


Backlog:

1. As a user, I want to switch between light and dark themes, so that I can use the dashboard comfortably in low-light environments.
2. As a user, I want the selected theme to persist across sessions, so that I do not have to change it every time I open the app.
3. As a user, I want a visual indicator in the settings, so that I can see which theme is active.

## Rules

- **Don't invent requirements.** If something is missing, ask. Do not fill gaps with your own assumptions.
- **Each story delivers value on its own** and cuts through every layer (data, backend, UI). Never split by layer.
- **No technical stories.** For example, "set up the database" or "create the user model" is not a story. Infrastructure belongs inside the first story that needs it.
- **Few stories.** Ten to fifteen at most. You can always add more later.
- **One line per story.** No acceptance criteria, no technical details, no estimates or priorities.
- **The first story is the smallest one usable end to end.** It defines the walking skeleton.
- **Use RFC 2119 keywords (MUST, MUST NOT, SHOULD, SHOULD NOT, MAY) for all normative statements.**
- The skill **MUST** ask one question at a time when requirements are unclear.
- The skill **MUST NOT** create stories, files, or acceptance criteria without user approval.
- The skill **SHOULD** keep the story list small and easy to discuss.
- The user **MAY** request additional clarification anytime during the conversation.

## Output format

When you have proposed a backlog, use this template:

```markdown
## Context

What it is and who it's for (one or two lines).
Stack: what we agreed.
Expensive decisions: the ones that apply, or "none".

## Backlog

1. As a <actor>, I want <action>, so that <benefit>.
2. As a <actor>, I want <action>, so that <benefit>. Requires: #1
```

Add `Requires: #N` only when a story makes no sense without another.

## Saving the results

If `ref` does not exist, create it. After the user approves the backlog, write the following files in the target project:

### ref/BACKLOG.md

Keep the same sections as the proposal, but add the full story list after the header.

```markdown
# Backlog

## Context

<one or two lines: what it is and who it's for>
Stack: <what we agreed>
Expensive decisions: <the ones that apply, or "none">

## Stories

1. As a <actor>, I want <action>, so that <benefit>.
2. As a <actor>, I want <action>, which enables me to <benefit>.
3. As a <actor>, I want <action>, allowing me to <benefit>.
```

### AGENT.md

Write or update the project context:

```markdown
# Project Context

- Purpose: <one or two sentences>
- Primary users: <who benefits>
- Stack: <agreed technologies>
- Constraints: <any important constraints>
- Expensive decisions: <list or none>
- Workflow files: ref/BACKLOG.md, ref/PLAN.md, AGENT.md
```
