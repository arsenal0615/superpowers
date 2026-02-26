---
name: writing-plans
description: Use when you have a spec or requirements for a multi-step task, before touching code
---

# Writing Plans

## Overview

Write comprehensive implementation plans assuming the engineer has zero context for our codebase and questionable taste. Document everything they need to know: which files to touch for each task, code, testing, docs they might need to check, how to test it. Give them the whole plan as bite-sized tasks. DRY. YAGNI. TDD. Frequent commits.

Assume they are a skilled developer, but know almost nothing about our toolset or problem domain. Assume they don't know good test design very well.

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

**Each step is one action (2-5 minutes):**
- "Write the failing test" - step
- "Run it to make sure it fails" - step
- "Implement the minimal code to make the test pass" - step
- "Run the tests and make sure they pass" - step
- "Commit" - step

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

**Files:**
- Create: `exact/path/to/file.py`
- Modify: `exact/path/to/existing.py:123-145`
- Test: `tests/exact/path/to/test.py`

**Step 1: Write the failing test**

```python
def test_specific_behavior():
    result = function(input)
    assert result == expected
```

**Step 2: Run test to verify it fails**

Run: `pytest tests/path/test.py::test_name -v`
Expected: FAIL with "function not defined"

**Step 3: Write minimal implementation**

```python
def function(input):
    return expected
```

**Step 4: Run test to verify it passes**

Run: `pytest tests/path/test.py::test_name -v`
Expected: PASS

**Step 5: Commit**

```bash
git add tests/path/test.py src/path/file.py
git commit -m "feat: add specific feature"
```
````

## Change Mode Task Structure

When writing a plan for a change (`docs/changes/<name>/plan.md`), use checkbox format for persistent tracking:

````markdown
## 1. [Group Name]

- [ ] 1.1 [Task description]

**Files:**
- Create: `exact/path/to/file.py`
- Test: `tests/exact/path/to/test.py`

**Steps:**
1. Write failing test
2. Run to verify failure
3. Implement minimal code
4. Run to verify pass
5. Commit

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

## Remember
- Exact file paths always
- Complete code in plan (not "add validation")
- Exact commands with expected output
- Reference relevant skills with @ syntax
- DRY, YAGNI, TDD, frequent commits

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
