# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Purpose

This repository contains shared Claude Code skills used across the dot-ai ecosystem. Skills are included as a git submodule at `.claude/skills/shared` in consuming repositories (dot-ai, dot-ai-ui, etc.) and symlinked to `.claude/skills/[skill-name]`.

## Skills Overview

| Skill | Purpose |
|-------|---------|
| `changelog-fragment` | Create towncrier changelog fragments for release notes |
| `process-feature-request` | Process incoming feature requests or responses from sibling projects |
| `query-dot-ai` | Query sibling dot-ai projects to verify feature availability |
| `request-dot-ai-feature` | Generate feature requests for sibling projects |
| `tag-release` | Create semantic version tags based on changelog fragments |
| `worktree-prd` | Create git worktrees with descriptive branch names for PRD work |

## Skill Structure

Each skill is a directory containing:
- `SKILL.md` - Main skill definition with YAML frontmatter and instructions
- Optional scripts (e.g., `create-worktree.sh`) referenced by the skill

When referencing bundled scripts from SKILL.md, use paths relative to the project root where the skill is installed:
```bash
.claude/skills/[skill-name]/script.sh
```

## Related Projects

Skills in this repo interact with sibling projects located at `../`:
- `dot-ai` - Main MCP server
- `dot-ai-ui` - Web UI
- `dot-ai-controller` - Kubernetes controller
- `dot-ai-stack` - Stack deployment configs
- `dot-ai-website` - Documentation website

## After Making Changes

After modifying skills, update the submodule in all consuming repos:
```bash
cd ../dot-ai && git submodule update --remote .claude/skills/shared
cd ../dot-ai-ui && git submodule update --remote .claude/skills/shared
cd ../dot-ai-controller && git submodule update --remote .claude/skills/shared
cd ../dot-ai-stack && git submodule update --remote .claude/skills/shared
cd ../dot-ai-website && git submodule update --remote .claude/skills/shared
```
