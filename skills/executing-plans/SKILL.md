---
name: executing-plans
description: Use when you have a written implementation plan to execute in a separate session with review checkpoints
---

# Executing Plans

## Overview

Load plan, review critically, execute tasks in batches, report for review between batches.

**Core principle:** Batch execution with checkpoints for architect review.

**Announce at start:** "I'm using the executing-plans skill to implement this plan."

## Plan Location Detection

Before loading the plan, determine the mode:

1. **Change Mode:** Plan is at `docs/changes/<name>/plan.md`
   - Detected when: user mentions a change name, or plan path is inside `docs/changes/`
   - Checkbox tracking: enabled (update `- [ ]` → `- [x]` in file after each task)
   - Progress persists across sessions

2. **Quick Mode:** Plan is at `docs/plans/*.md`
   - Detected when: plan path is inside `docs/plans/` or no change context
   - Checkbox tracking: not applicable (use TodoWrite as before)

## The Process

### Step 1: Load and Review Plan
1. Read plan file
2. **If Change Mode:** Check for existing progress — count `- [x]` vs `- [ ]` checkboxes
   - If progress exists: display summary ("N/M tasks complete, resuming from task K") and skip completed tasks
   - If no progress: start from the beginning
3. Review critically — identify any questions or concerns about the plan
4. If concerns: Raise them with your human partner before starting
5. If no concerns: Create TodoWrite for remaining tasks and proceed

### Step 2: Execute Batch
**Default: First 3 tasks** (skip already-completed tasks in Change Mode)

For each task:
1. Mark as in_progress (TodoWrite)
2. Achieve each task's completion criteria, using key constraints as guardrails
3. If in Change Mode, read `design.md` for implementation details referenced by task constraints
4. Run verifications as specified
5. Mark as completed (TodoWrite)
6. **If Change Mode:** Update the plan file — find the corresponding `- [ ]` line and replace with `- [x]`, write file immediately

### Step 3: Report
When batch complete:
- Show what was implemented
- Show verification output
- Say: "Ready for feedback."

### Step 4: Continue
Based on feedback:
- Apply changes if needed
- Execute next batch
- Repeat until complete

### Step 5: Complete Development

After all tasks complete and verified:
- Announce: "I'm using the finishing-a-development-branch skill to complete this work."
- **REQUIRED SUB-SKILL:** Use superpowers:finishing-a-development-branch
- Follow that skill to verify tests, present options, execute choice

## When to Stop and Ask for Help

**STOP executing immediately when:**
- Hit a blocker mid-batch (missing dependency, test fails, instruction unclear)
- Plan has critical gaps preventing starting
- You don't understand an instruction
- Verification fails repeatedly

**Ask for clarification rather than guessing.**

## When to Revisit Earlier Steps

**Return to Review (Step 1) when:**
- Partner updates the plan based on your feedback
- Fundamental approach needs rethinking

**Don't force through blockers** - stop and ask.

## Remember
- Review plan critically first
- Achieve completion criteria, using key constraints as guardrails
- Read design.md for implementation details when tasks reference design decisions
- Don't skip verifications
- Reference skills when plan says to
- Between batches: just report and wait
- Stop when blocked, don't guess
- Never start implementation on main/master branch without explicit user consent

## Integration

**Required workflow skills:**
- **superpowers:using-git-worktrees** - REQUIRED: Set up isolated workspace before starting
- **superpowers:writing-plans** - Creates the plan this skill executes
- **superpowers:finishing-a-development-branch** - Complete development after all tasks
