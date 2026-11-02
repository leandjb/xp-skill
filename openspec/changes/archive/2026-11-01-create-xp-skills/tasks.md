# Tasks

## 1. Create the three XP skills

- [x] 1.1 Create `.agents/skills/backlog/SKILL.md` with standard frontmatter (`name: backlog`, description, license `GPL-3.0`, metadata author/version) and verify the file exists and name matches directory
- [x] 1.2 Create `.agents/skills/story/SKILL.md` with standard frontmatter (`name: story`) and verify the file exists and name matches directory
- [x] 1.3 Create `.agents/skills/pair/SKILL.md` with standard frontmatter (`name: pair`) and verify the file exists and name matches directory

## 2. Content requirements

- [x] 2.1 Write `backlog` body with XP planning-game rules in RFC 2119, professional English tone, and at least one worked example of user-story generation; verify example block is present
- [x] 2.2 Write `story` body with 2-3 Given/When/Then criteria rules, <=5-bullet mini plan, KISS, no code, RFC 2119 wording; verify rules use RFC 2119 keywords
- [x] 2.3 Write `pair` body with review/test-list/cycle/check/commit sections, strict TDD red-green-refactor, DRY/KISS, stop rules, never-commit rule, RFC 2119 wording; verify TDD and DRY/KISS are stated

## 3. Validation

- [x] 3.1 Validate each skill with `skills-ref` if available, otherwise manually check frontmatter fields, name/dir match, description length <=1024, and no non-standard top-level fields; verify all three pass
- [x] 3.2 Confirm each `SKILL.md` is under 500 lines and that RFC 2119 keywords appear in each; verify line counts and keyword presence

## Workflow follow-up

- Archive the change after review requirements are satisfied.