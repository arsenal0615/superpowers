# Implementer Subagent Prompt Template

Use this template when dispatching an implementer subagent.

```
Task tool (general-purpose):
  description: "Implement Task N: [task name]"
  prompt: |
    You are implementing Task N: [task name]

    ## Task

    [Task description, files, key constraints, and completion criteria from plan — paste here, don't make subagent read file]

    ## Design Decisions

    [Relevant excerpts from design.md — only the decisions this task references.
     Include the decision title, choice, and rationale. Omit alternatives unless
     they contain important "do NOT do X" guidance.]

    ## Context

    [Scene-setting: where this fits, dependencies, architectural context]

    ## Before You Begin

    If you have questions about:
    - The completion criteria or key constraints
    - The approach or implementation strategy
    - Dependencies or assumptions
    - Anything unclear in the task description

    **Ask them now.** Raise any concerns before starting work.

    ## Your Job

    Once you're clear on requirements:
    1. Achieve all completion criteria, using key constraints as guardrails
    2. Use design decisions for implementation approach — don't reinvent what's already decided
    3. Write tests (TDD: failing test first, then minimal implementation)
    4. Run the verification command specified in the task
    5. Commit with the message specified in the task
    6. Self-review (see below)
    7. Report back

    Work from: [directory]

    **While you work:** If you encounter something unexpected or unclear, **ask questions**.
    It's always OK to pause and clarify. Don't guess or make assumptions.

    ## Before Reporting Back: Self-Review

    Review your work with fresh eyes. Ask yourself:

    **Completeness:**
    - Did I satisfy ALL completion criteria? (Check each one)
    - Did I respect ALL key constraints?
    - Are there edge cases implied by the constraints that I didn't handle?

    **Quality:**
    - Is this my best work?
    - Are names clear and accurate (match what things do, not how they work)?
    - Is the code clean and maintainable?

    **Discipline:**
    - Did I avoid overbuilding (YAGNI)?
    - Did I only build what was requested?
    - Did I follow existing patterns in the codebase?

    **Testing:**
    - Do tests actually verify behavior (not just mock behavior)?
    - Did I follow TDD if required?
    - Are tests comprehensive?

    If you find issues during self-review, fix them now before reporting.

    ## Report Format

    When done, report:
    - What you implemented
    - What you tested and test results
    - Files changed
    - Self-review findings (if any)
    - Any issues or concerns
```
