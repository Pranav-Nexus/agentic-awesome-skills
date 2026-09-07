---
name: skill-porter
description: "Imports, converts, and optimizes external agent skills (Claude Code, Cursor, generic LLMs) into native Google Antigravity plugins and skills."
category: developer-tools
risk: safe
source: community
source_repo: Pranav-Nexus/antigravity-skill-porter
source_type: community
date_added: "2026-09-07"
author: Pranav-Nexus
tags: [antigravity, claude, skills, migration, agent]
tools: [claude, cursor, gemini]
license: "MIT"
license_source: "https://github.com/Pranav-Nexus/antigravity-skill-porter/blob/main/LICENSE"
---

# Skill Porter & Optimizer for Google Antigravity

## Overview

Imports, converts, and optimizes external agent skills created for Claude Code, Cursor, Codex, or OpenAI into native Google Antigravity multi-agent plugins. It translates serial subagent patterns into Antigravity parallel `invoke_subagent` calls, maps legacy tool references (`view_file`, `replace_file_content`, `run_command`), and standardizes context anchoring to `GEMINI.md` and `AGENTS.md`.

## When to Use

- When importing or adapting skills from Claude Code or Cursor into Google Antigravity.
- When migrating multi-agent workflows that benefit from Antigravity parallel subagent execution.
- When converting single SKILL.md files or multi-skill directories and plugin bundles.
- When standardizing tool conventions and artifact generation for Antigravity workspaces.

## How It Works

### Step 1: Source Identification

Specify the source skill or repository:
- Local skill folder or `SKILL.md` file path
- Remote GitHub repository URL (e.g., `https://github.com/owner/repo`)

### Step 2: AST & Deterministic Translation

Run the bundled porting utility from the skill's scripts directory:

```bash
# Using the bundled utility for Antigravity IDE:
python scripts/port_skill.py --source "<source-path-or-url>" --dest "~/.agents/skills"

# Or for Antigravity CLI (agy):
python scripts/port_skill.py --source "<source-path-or-url>" --dest "~/.gemini/antigravity-cli/skills"

# Or preview transformations without writing:
python scripts/port_skill.py --source "<source-path-or-url>" --dry-run
```

The converter performs:
1. **Tool Standardizing**: Maps `View` -> `view_file`, `Edit` -> `replace_file_content`, `Bash` -> `run_command`, `Grep` -> `grep_search`, `Find` -> `find_by_name`.
2. **Subagent Parallelization**: Transforms serial subagent invocations into native parallel `invoke_subagent` arrays.
3. **Artifact Generation**: Converts standard markdown code outputs into Antigravity interactive artifacts.
4. **Context Anchoring**: Migrates `CLAUDE.md` and `CURSOR.md` references to `GEMINI.md` and `AGENTS.md`.

### Step 3: Verification & Installation

Verify the converted skill passes frontmatter schema checks and test invoking it within Antigravity.

## Examples

### Example 1: Converting a Remote Claude Skill

```bash
python scripts/port_skill.py --source "https://github.com/anthropics/skills/tree/main/skills/frontend-design" --dest "~/.agents/skills"
```

### Example 2: Workspace-Scoped Port

```bash
python scripts/port_skill.py --source "./my-claude-skill" --workspace
```

## Limitations

- **Untrusted Source Code**: Always inspect and audit third-party skills and executable scripts before running them or granting filesystem permissions.
- **Semantic Prompt Nuances**: While structural tool bindings and subagent calls are rewritten deterministically, complex custom prompt policies may still require manual review and verification.
- **External MCP Prerequisites**: Skills that rely on specialized proprietary MCP servers require that the corresponding server be separately declared in your `mcp_config.json`.
- **Environment Compatibility**: The automated converter targets Google Antigravity >= 1.0.0; older preview formats are not backwards compatible.
