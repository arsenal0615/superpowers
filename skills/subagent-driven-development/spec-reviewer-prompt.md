# Spec Compliance Reviewer Prompt Template

Use this template when dispatching a spec compliance reviewer subagent.

**Purpose:** Verify implementer achieved all completion criteria and respected all key constraints (nothing more, nothing less)

```
Task tool (general-purpose):
  description: "Review spec compliance for Task N"
  prompt: |
    You are reviewing whether an implementation satisfies its completion criteria and respects its key constraints.

    ## What Was Requested

    **Completion Criteria:**
    [List of completion criteria from the task]

    **Key Constraints:**
    [List of key constraints from the task]

    **Design Decisions:**
    [Relevant design decisions the task references]

    ## What Implementer Claims They Built

    [From implementer's report]

    ## CRITICAL: Do Not Trust the Report

    The implementer finished suspiciously quickly. Their report may be incomplete,
    inaccurate, or optimistic. You MUST verify everything independently.

    **DO NOT:**
    - Take their word for what they implemented
    - Trust their claims about completeness
    - Accept their interpretation of requirements

    **DO:**
    - Read the actual code they wrote
    - Check each completion criterion against the actual implementation
    - Verify each key constraint was respected
    - Look for extra features beyond what criteria require

    ## Your Job

    Read the implementation code and verify:

    **Completion Criteria check:**
    - For each completion criterion: is it actually satisfied? Verify by reading code.
    - Are there criteria they claimed to satisfy but didn't?
    - Are any criteria only partially met?

    **Key Constraints check:**
    - For each constraint: was it respected?
    - Did they violate any boundary or compatibility requirement?
    - Did they use the specified interfaces/data structures?

    **Scope check:**
    - Did they build things beyond what the completion criteria require?
    - Did they add "nice to haves" not covered by criteria or constraints?
    - Did they deviate from design decisions without justification?

    **Verify by reading code, not by trusting report.**

    Report:
    - ✅ Criteria met, constraints respected (if all check out after code inspection)
    - ❌ Issues found:
      - Criteria not met: [which criterion, what's missing, file:line]
      - Constraints violated: [which constraint, how, file:line]
      - Scope exceeded: [what was added beyond criteria]
```
