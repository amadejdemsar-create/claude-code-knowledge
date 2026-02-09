# The Claude Code Tutorial Level 2

**Source:** https://x.com/eyad_khrais/status/2010810802023141688
**Author:** Eyad (@eyad_khrais)
**Date:** Jan 12, 2026
**Category:** Best Practices / Pro Tips (Advanced)

---

Advanced concepts: skills, subagents, and MCP connectors.

## Context Window: Claude Code vs Cursor
- Claude Code: consistent 200K tokens
- Cursor: often truncates to 70-120K due to internal safeguards
- For larger projects, use Claude Code directly

## Skills: Teaching Claude Your Workflows
A skill is a markdown file that teaches Claude something specific to your work.

**Location:** `~/.claude/skills/skill-name/SKILL.md` (user-level) or `.claude/skills/skill-name/SKILL.md` (project-level)

**Structure:**
```yaml
---
name: commit-messages
description: Generate commit messages following our team's conventions.
---
# Commit Message Format
All commits follow conventional commits:
- feat: new feature
- fix: bug fix
- refactor: code change that neither fixes nor adds
Format: `type(scope): description`
```

**Key points:**
- Description is critical - Claude uses it to decide when to apply the skill
- Progressive disclosure: only name+description loaded at startup (~100 tokens each), full instructions loaded when relevant
- Not limited to code: database queries, API docs, meeting notes, personal workflows
- Check loaded skills: ask Claude "What skills do you have available?"

## Subagents: Parallel Processing with Isolated Context
Separate Claude instance with its own context window, system prompt, and tool permissions.

**Built-in subagents:**
- **Explore**: Fast, read-only agent for searching/analyzing codebases
- **Plan**: Research agent for plan mode
- **General-purpose**: Multi-step tasks requiring exploration + action

**Create custom subagents:**
Location: `~/.claude/agents/` or `.claude/agents/`

```yaml
---
name: security-reviewer
description: Reviews code for security vulnerabilities.
tools: Read, Grep, Glob
---
Check for: auth gaps, injection vulnerabilities, data exposure, insecure dependencies.
```

**Tools field** controls permissions: Read/Grep/Glob for read-only, add Write/Edit/Bash for implementation.

**Communication pattern:**
1. Main agent identifies task for delegation
2. Main agent invokes subagent with specific prompt
3. Subagent executes in own context window
4. Subagent returns summary (not full context) to main agent

Subagents cannot spawn other subagents.

**Patterns:**
- Large refactoring: subagent per logical group of files
- Code review pipeline: style-checker + security-scanner + test-coverage in parallel
- Research: delegate to Explore with specific questions

## MCP Connectors
Standardized way for AI to call external tools/data sources.

**Add connector:**
```bash
# HTTP transport
claude mcp add --transport http notion https://mcp.notion.com/mcp

# With authentication
claude mcp add --transport http github https://api.github.com/mcp \
  --header "Authorization: Bearer your-token"
```

Or via web app: settings -> connectors -> find server -> configure -> give permissions.

**Recommended MCP servers:** GitHub, Slack, Google Drive, PostgreSQL/databases, Linear/Jira

**Check connections:** `/mcp` in Claude Code

Note: Third-party MCP servers aren't verified by Anthropic - review source code for sensitive integrations.

## The Compound Effect
- Skill (encodes patterns) + Subagent (handles subtasks) + MCP (connects services) = multiplied capabilities
- Start with one skill for something you explain repeatedly
- Engineers who get the most treat Claude Code as a system, not for one-off tasks
