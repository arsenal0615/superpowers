---
name: writing-plans
description: Use when you have a spec or requirements for a multi-step task, before touching code
---

# Writing Plans

## Overview

Write declarative implementation plans as work orders — each task specifies goals, file paths, key constraints (from design decisions), completion criteria, and verification commands. Do NOT include inline implementation code. The executing agent reads design.md and the codebase for implementation details and writes code from constraints. Give them the whole plan as bite-sized tasks. DRY. YAGNI. TDD. Frequent commits.

Assume the executing agent has zero context for the codebase. Each task may be executed by an independent sub-agent that knows nothing about other tasks. Make each task self-contained.

**Language Rule:** Write the plan in the same language the user used in conversation. If the user writes in Chinese, the plan is in Chinese. If in English, the plan is in English. Field labels (Files, Key Constraints, Completion Criteria, Verify, Commit) remain in English for parseability.

**Announce at start:** "I'm using the writing-plans skill to create the implementation plan."

**Context:** This should be run in a dedicated worktree (created by brainstorming skill).

## Change Context Detection

Before writing the plan, detect whether you're working within a change:

1. **Check conversation context** — Has the user mentioned a change name? Have you been working on a change in `docs/changes/<name>/`?
2. **Check for explicit path** — Did the user reference a file inside `docs/changes/`?

**If change context detected (Change Mode):**
- Read `docs/changes/<name>/proposal.md`, `design.md`, and `specs/` as input context
- Save plan to: `docs/changes/<name>/plan.md`
- Use checkbox format for tasks (see Change Mode Task Structure below)

**If no change context (Quick Mode):**
- Save plans to: `docs/plans/YYYY-MM-DD-<feature-name>.md`
- Use original task format (see Task Structure below)

## Bite-Sized Task Granularity

**Each task is one cohesive unit of work (2-5 minutes):**
- One clear goal that produces a verifiable result
- Small enough to commit atomically
- Independent enough to execute with zero shared context from other tasks
- TDD is still the expectation — but the plan states criteria, not steps

## Plan Document Header

**Every plan MUST start with this header:**

```markdown
# [Feature Name] Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** [One sentence describing what this builds]

**Architecture:** [2-3 sentences about approach]

**Tech Stack:** [Key technologies/libraries]

---
```

## Task Structure

````markdown
### Task N: [Component Name]

**Files:** `exact/path/to/file.py`, `exact/path/to/existing.py:123-145`

**Key Constraints:**
- [Design decision or architectural rule relevant to this task]
- [Interface, data structure, or protocol to use/follow]
- [Boundary condition, compatibility requirement, or invariant]

**Completion Criteria:**
- [ ] [Measurable criterion 1 — what must be true when done]
- [ ] [Measurable criterion 2]

**Verify:** `command to run` — expected: [brief expected outcome]
**Commit:** `type(scope): message`
````

**Key Constraints come from:**
- `design.md` decisions (in Change Mode) — cite by decision name/number
- Codebase patterns discovered during planning
- API contracts, data formats, edge cases

**Completion Criteria must be measurable:**
- Good: "returns 400 for empty input", "second call returns in <10ms"
- Bad: "add validation", "improve performance"

## Change Mode Task Structure

When writing a plan for a change (`docs/changes/<name>/plan.md`), use checkbox format for persistent tracking:

````markdown
## 1. [Group Name]

- [ ] 1.1 [Task description — one line]

**Files:** `exact/path/to/file.py`, `exact/path/to/existing.py:123-145`

**Key Constraints:**
- [Cite design.md decision, e.g., "per Decision 3: templates in style-guide"]
- [Interface or data structure to follow]
- [Compatibility or boundary requirement]

**Completion Criteria:**
- [ ] [Measurable criterion 1]
- [ ] [Measurable criterion 2]

**Verify:** `command` — expected: [outcome]
**Commit:** `type(scope): message`

- [ ] 1.2 [Next task description]
...

## 2. [Next Group]

- [ ] 2.1 [Task description]
...
````

**Key differences from quick mode:**
- Tasks use `- [ ]` checkbox format (updated to `- [x]` during execution)
- Tasks are numbered as `N.M` (group.task)
- Groups are numbered `## N. Group Name`
- Progress persists across sessions via checkbox state
- Key Constraints cite design.md decisions by name

## Remember
- Exact file paths always (with line ranges when modifying existing files)
- Complete constraints and criteria in plan (not "add validation" but "returns 400 for empty input")
- NO inline implementation code — the executing agent writes code from design.md + constraints
- Key Constraints extracted from design.md decisions (cite by decision name/number)
- Verification commands with expected output
- Each task self-contained — executable with zero shared context from other tasks
- Reference relevant skills with @ syntax
- DRY, YAGNI, TDD, frequent commits
- Plan language follows user's input language

## Execution Handoff

After saving the plan, offer execution choice:

**Change Mode:**

**"Plan complete and saved to `docs/changes/<name>/plan.md`. Two execution options:**

**1. Subagent-Driven (this session)** - I dispatch fresh subagent per task, review between tasks, fast iteration

**2. Parallel Session (separate)** - Open new session with executing-plans, batch execution with checkpoints. Progress is tracked via checkboxes — you can resume anytime.

**Which approach?"**

**Quick Mode:**

**"Plan complete and saved to `docs/plans/<filename>.md`. Two execution options:**

**1. Subagent-Driven (this session)** - I dispatch fresh subagent per task, review between tasks, fast iteration

**2. Parallel Session (separate)** - Open new session with executing-plans, batch execution with checkpoints

**Which approach?"**

**If Subagent-Driven chosen:**
- **REQUIRED SUB-SKILL:** Use superpowers:subagent-driven-development
- Stay in this session
- Fresh subagent per task + code review

**If Parallel Session chosen:**
- Guide them to open new session in worktree
- **REQUIRED SUB-SKILL:** New session uses superpowers:executing-plans
