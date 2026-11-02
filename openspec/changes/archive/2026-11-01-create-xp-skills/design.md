# Design

## Context

See `proposal.md` - Why. The repository already contains `.agents/skills/` with OpenSpec workflow skills (`openspec-*`). New skills follow the same layout: one directory per skill, each with a `SKILL.md` that contains YAML frontmatter plus Markdown instructions. The three XP skills (`backlog`, `story`, `pair`) are documented solely as agent instructions; no scripts, references, assets, or code are introduced, and no client‑specific frontmatter fields are used.

## Goals / Non-Goals

**Goals:**
- Provide three agent skills that together form a comfortable, step‑by‑step XP workflow for a small AI‑assisted project team.
- `backlog` **clarifies** the problem before any work, asks one question at a time with a suggested answer, and, after approval, writes the agreed vertical user stories to `ref/BACKLOG.md` and the agreed project context to `AGENT.md`.
- `story` refines a single story into 2‑3 outcome‑based Given/When/Then acceptance criteria and a **minimal** ≤5‑step plan with verifiable tasks; after approval it writes the story, criteria, plan and tasks to `ref/PLAN.md`.
- `pair` acts as the driver: it reads the approved plan, proposes a test list for approval, then implements the tasks with strict TDD (red → green → refactor), applying KISS, DRY and clean‑code rules, stopping after one step unless the user asks for a batch, and never committing on its own.
- All three skills ask clarifying questions before inventing requirements, keep explanations separate from normative rules, and use professional English with RFC 2119 keywords for normative statements.
- Every skill is **self‑contained**: it reads any existing handoff files, preserves unrelated content, and only appends or updates sections it owns.

**Non‑Goals:**
- No new runtime behaviour, no scripts, references or assets, no client‑specific extensions, and no broader XP suite.
- The skills do not generate application code directly; they guide the user through planning and then rely on `pair` to implement, while leaving commit decisions to the human.

## Decisions

- **Skill names and directories:** `backlog`, `story`, `pair`; the `name` frontmatter field equals the directory name.
- **Frontmatter:** only the standard Agent Skills fields (`name`, `description`, `license`, `metadata`). Metadata contains `author: Leandro Barros` and `version: "0.1.0"`. No `compatibility`, `allowed-tools`, or other client extensions.
- **License:** `GPL-3.0`, matching the repository `LICENSE`.
- **Language & tone:** professional English; RFC 2119 keywords are used for every normative statement.
- **Evolution & discovery:** the skills explicitly read the singular `AGENT.md` (and any `ref/BACKLOG.md` or `ref/PLAN.md`) rather than relying on client‑side auto‑discovery. If a file does not exist, the skill creates it; if the file exists, the skill preserves unrelated content and appends or updates only the sections it owns.
- **Persistence contract:**
  - `/backlog` → `ref/BACKLOG.md` (context, stack, expensive decisions, story list), `AGENT.md` (purpose, users, stack, constraints, decisions, workflow locations).
  - `/story` → `ref/PLAN.md` (selected story, acceptance criteria, minimal plan, out‑of‑scope, suggestions, approved test list, QA status).
  - `/pair` → updates `ref/PLAN.md` QA status and task verification evidence; never writes to the backlog or plan files except status updates.
- **User‑control flow:**
  - `/backlog` always asks clarifying questions before inventing requirements and asks for explicit approval before writing files.
  - `/story` asks which story to refine, proposes criteria and a plan, and waits for approval before persisting.
  - `/pair` reads the approved plan, obtains approval for the test list, then runs **one TDD cycle** by default and pauses for the user after each step. A user request such as `do N` runs N full cycles, still one after another, and never mixes multiple tests with a single implementation step.
- **Testing & QA:** test names are derived from acceptance criteria; test list includes a testing level for each item; criteria that cannot be automated are marked manual verification. The AI must not claim a test passed unless it has just run the test, and must not treat its own checks as independent review. Manual verification is always separated from automated checks, and blockers are reported honestly.
- **Stop rules (pair):** unplanned design decision, out‑of‑scope addition, test would need to change to make a test pass, two consecutive failures, step > ~50 lines or touches > 3 files → propose splitting.
- **Refactoring:** only after tests are green and with explicit user approval; refactoring never mixes with new functionality.
- **Commit:** never commit; suggest a commit message for the user when the step is complete.

## Risks / Trade‑offs

- [Risk] Over‑asking may slow the conversation → Mitigation: ask only one question at a time, with a suggested answer, and only when something is truly unclear.
- [Risk] Persisting files may conflict with client project conventions → Mitigation: skills always read existing `AGENT.md` (and `ref/*`) first, preserve unrelated content, and ask before overwriting.
- [Risk] Manual verification may be forgotten → Mitigation: the test list explicitly marks manual items, and the QA status section in `ref/PLAN.md` reminds the user to verify them.
- [Risk] The user may miss the explicit `do N` meaning → Mitigation: the skill repeats the meaning of `do N` (run N full cycles, one after another) before executing.
- [Risk] Skills‑ref is not installed → Mitigation: manual frontmatter, naming and length checks are performed; skills are kept to < 500 lines and use standard frontmatter only.

## Migration Plan

No deployment or rollback is needed. New files are added; existing OpenSpec workflow files are untouched. After the skills are added, the user can run the three skills in sequence on any project that uses the `.agents/skills/` layout.

## Open Questions

None. All behaviour is explicitly described in the skills and in this design.
