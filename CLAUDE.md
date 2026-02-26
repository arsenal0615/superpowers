# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Superpowers is a composable skills library for AI coding agents (Claude Code, Cursor, Codex, OpenCode). It provides structured workflows — brainstorming, TDD, debugging, change management — that agents invoke automatically via a SessionStart hook. Skills are Markdown files with YAML frontmatter, not traditional source code.

**Author:** Jesse Vincent | **Version:** 4.3.1 | **License:** MIT

## Architecture

### How It Works

1. **SessionStart hook** (`hooks/session-start`) runs synchronously at session start, injecting the `using-superpowers` skill content as JSON context
2. **`using-superpowers`** (`skills/using-superpowers/SKILL.md`) is the master skill — it teaches the agent how to discover and invoke all other skills
3. **Skill invocation** happens via the `Skill` tool in Claude Code; other platforms have their own mechanisms
4. **Personal skills** in `~/.claude/skills/` can shadow superpowers skills (personal takes priority)

### Key Components

- **`skills/`** — 16 SKILL.md files, each a self-contained workflow with YAML frontmatter (`name`, `description`)
- **`commands/`** — Quick-launch command files (e.g., `/brainstorm`, `/explore`, `/new-change`)
- **`hooks/`** — SessionStart hook (`session-start` bash script) + `run-hook.cmd` polyglot wrapper for Windows/Unix
- **`lib/skills-core.js`** — Node.js skill discovery engine: `extractFrontmatter()`, `findSkillsInDir()`, `resolveSkillPath()`, `stripFrontmatter()`
- **`agents/`** — Subagent templates (e.g., `code-reviewer.md`)
- **`.claude-plugin/plugin.json`** — Claude Code plugin manifest
- **`.cursor-plugin/plugin.json`** — Cursor plugin manifest

### Two Workflow Modes

**Quick Mode:** brainstorming → writing-plans → executing-plans/subagent-driven-development
- Design doc saved to `docs/plans/YYYY-MM-DD-<topic>-design.md`

**Change Mode:** managing-changes lifecycle (propose → design → plan → execute → verify → archive)
- Change directory at `docs/changes/<name>/` with `.change.yaml`, `proposal.md`, `design.md`, `plan.md`
- Specs accumulate in `docs/specs/`

### Skill Frontmatter Format

```yaml
---
name: skill-name
description: Use when [condition] - [what it does]
---
```

## Running Tests

Tests require Claude Code installed (`claude` command available) and must run FROM the superpowers directory.

```bash
# Integration tests (subagent-driven-development)
cd tests/claude-code
./test-subagent-driven-development-integration.sh

# Explicit skill request tests (10+ scenarios)
cd tests/explicit-skill-requests
./run-all.sh

# Skill triggering behavior tests
cd tests/skill-triggering
./run-all.sh

# OpenCode-specific tests
cd tests/opencode
./run-tests.sh

# Token usage analysis
python3 tests/claude-code/analyze-token-usage.py ~/.claude/projects/<path>/<session>.jsonl
```

## Platform-Specific Notes

### Windows
- `hooks/run-hook.cmd` is a polyglot batch/bash file — do NOT convert to pure bash or pure batch
- Uses multi-location bash discovery (standard Git for Windows paths, then PATH fallback)
- JSON escaping in `session-start` uses bash parameter substitution (O(n)), not character loops
- `.gitattributes` enforces LF line endings for all scripts, markdown, and JSON

### Plugin Installation
- **Claude Code:** `/plugin marketplace add arsenal0615/superpowers-marketplace` then `/plugin install superpowers@qx-superpowers-marketplace`
- **Cursor:** `/plugin-add superpowers`
- **Codex/OpenCode:** Manual clone + symlink (see platform-specific docs in `docs/`)

## Development Conventions

- Skills are the primary artifact — they are executable Markdown instructions, not code
- Rigid skills (TDD, debugging) must be followed exactly; flexible skills (patterns) can adapt to context
- The SessionStart hook must remain synchronous (`async: false` in `hooks.json`) — async caused race conditions where skills weren't loaded before the first turn
- The hook outputs both `additional_context` (Cursor) and `hookSpecificOutput.additionalContext` (Claude Code) for cross-platform compatibility
