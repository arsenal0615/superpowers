---
name: using-superpowers
description: Use when starting any conversation - establishes how to find and use skills, requiring Skill tool invocation before ANY response including clarifying questions
---

<EXTREMELY-IMPORTANT>
If you think there is even a 1% chance a skill might apply to what you are doing, you ABSOLUTELY MUST invoke the skill.

IF A SKILL APPLIES TO YOUR TASK, YOU DO NOT HAVE A CHOICE. YOU MUST USE IT.

This is not negotiable. This is not optional. You cannot rationalize your way out of this.
</EXTREMELY-IMPORTANT>

## How to Access Skills

**In Claude Code:** Use the `Skill` tool. When you invoke a skill, its content is loaded and presented to you—follow it directly. Never use the Read tool on skill files.

**In other environments:** Check your platform's documentation for how skills are loaded.

# Using Skills

## The Rule

**Invoke relevant or requested skills BEFORE any response or action.** Even a 1% chance a skill might apply means that you should invoke the skill to check. If an invoked skill turns out to be wrong for the situation, you don't need to use it.

```dot
digraph skill_flow {
    "User message received" [shape=doublecircle];
    "About to EnterPlanMode?" [shape=doublecircle];
    "Wants to think/explore?" [shape=diamond];
    "Invoke exploring skill" [shape=box];
    "Wants structured change?" [shape=diamond];
    "Invoke managing-changes skill" [shape=box];
    "Already brainstormed?" [shape=diamond];
    "Invoke brainstorming skill" [shape=box];
    "Might any skill apply?" [shape=diamond];
    "Invoke Skill tool" [shape=box];
    "Announce: 'Using [skill] to [purpose]'" [shape=box];
    "Has checklist?" [shape=diamond];
    "Create TodoWrite todo per item" [shape=box];
    "Follow skill exactly" [shape=box];
    "Respond (including clarifications)" [shape=doublecircle];

    "User message received" -> "Wants to think/explore?";
    "Wants to think/explore?" -> "Invoke exploring skill" [label="yes"];
    "Wants to think/explore?" -> "Wants structured change?" [label="no"];
    "Wants structured change?" -> "Invoke managing-changes skill" [label="yes"];
    "Wants structured change?" -> "Might any skill apply?" [label="no"];
    "Invoke exploring skill" -> "Follow skill exactly";
    "Invoke managing-changes skill" -> "Follow skill exactly";

    "About to EnterPlanMode?" -> "Already brainstormed?";
    "Already brainstormed?" -> "Invoke brainstorming skill" [label="no"];
    "Already brainstormed?" -> "Might any skill apply?" [label="yes"];
    "Invoke brainstorming skill" -> "Might any skill apply?";

    "Might any skill apply?" -> "Invoke Skill tool" [label="yes, even 1%"];
    "Might any skill apply?" -> "Respond (including clarifications)" [label="definitely not"];
    "Invoke Skill tool" -> "Announce: 'Using [skill] to [purpose]'";
    "Announce: 'Using [skill] to [purpose]'" -> "Has checklist?";
    "Has checklist?" -> "Create TodoWrite todo per item" [label="yes"];
    "Has checklist?" -> "Follow skill exactly" [label="no"];
    "Create TodoWrite todo per item" -> "Follow skill exactly";
}
```

## Red Flags

These thoughts mean STOP—you're rationalizing:

| Thought | Reality |
|---------|---------|
| "This is just a simple question" | Questions are tasks. Check for skills. |
| "I need more context first" | Skill check comes BEFORE clarifying questions. |
| "Let me explore the codebase first" | Skills tell you HOW to explore. Check first. |
| "I can check git/files quickly" | Files lack conversation context. Check for skills. |
| "Let me gather information first" | Skills tell you HOW to gather information. |
| "This doesn't need a formal skill" | If a skill exists, use it. |
| "I remember this skill" | Skills evolve. Read current version. |
| "This doesn't count as a task" | Action = task. Check for skills. |
| "The skill is overkill" | Simple things become complex. Use it. |
| "I'll just do this one thing first" | Check BEFORE doing anything. |
| "This feels productive" | Undisciplined action wastes time. Skills prevent this. |
| "I know what that means" | Knowing the concept ≠ using the skill. Invoke it. |

## Skill Priority

When multiple skills could apply, use this order:

1. **Thinking skills first** (exploring, brainstorming, debugging) — these determine HOW to approach the task
2. **Lifecycle skills second** (managing-changes) — these provide structure for multi-artifact work
3. **Implementation skills third** (frontend-design, mcp-builder) — these guide execution

### Mode Selection Guide

When the user wants to build or change something, choose the right entry point:

| User Intent | Skill | Why |
|-------------|-------|-----|
| "Let's think about X" / "explore" / "investigate" | **exploring** | Free-form thinking, no commitment |
| "Let's build X" (simple, quick) | **brainstorming** | Quick mode: design → plan → execute |
| "Let's build X" (complex, needs tracking) | **managing-changes** | Change mode: propose → design → plan → execute → archive |
| "Create a change" / "new feature" / "write proposal" | **managing-changes** | Explicit lifecycle request |
| "What changes do we have?" / "status" / "archive" | **managing-changes** | Lifecycle operations |
| "Fix this bug" | **debugging** | Systematic root cause analysis |

**Key distinction:**
- **exploring** = thinking without commitment (may not produce artifacts)
- **brainstorming** = designing with commitment (always produces design doc + plan)
- **managing-changes** = structured lifecycle (produces change directory with proposal, specs, design, plan)

## Skill Types

**Rigid** (TDD, debugging): Follow exactly. Don't adapt away discipline.

**Flexible** (patterns): Adapt principles to context.

The skill itself tells you which.

## User Instructions

Instructions say WHAT, not HOW. "Add X" or "Fix Y" doesn't mean skip workflows.
