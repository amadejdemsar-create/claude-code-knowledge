# Claude Code Best Practices (from the Creator, Boris Cherny)

**Source:** https://x.com/bcherny/status/2007179832300581177
**Author:** Boris Cherny (@bcherny) - Creator of Claude Code at Anthropic
**Date:** Jan 2, 2026
**Category:** Best Practices / Pro Tips (Creator's Setup)

---

Boris's personal setup and tips. Surprisingly vanilla - Claude Code works great out of the box. Each person on the Claude Code team uses it very differently.

## 1. Vanilla Setup
Setup is minimal customization. Claude Code works great out of the box. No one correct way to use it - intentionally built to be customizable however you like.

## 2. Run Multiple Claudes in Parallel
Runs 5-10 Claudes on claude.ai/code in parallel with local Claudes. Hands off local sessions to web (using &), manually kicks off sessions in Chrome, and teleports back and forth between them.

## 3. Opus 4.5 with Thinking for Everything
Uses Opus 4.5 with thinking for everything. It's the best coding model he's ever used. Even though it's bigger and slower than Sonnet, you have to steer it less, it's better at tool use, and it's almost always faster than using a smaller model in the end.

## 4. Shared CLAUDE.md
Team shares a single CLAUDE.md for the Claude Code repo. It's checked into git, and the whole team contributes multiple times a week. Anytime they see Claude do something incorrectly, they add it to the CLAUDE.md so Claude knows not to do it next time.

Includes commands like:
```sh
# 1. Make changes
# 2. Typecheck (fast)
bun run typecheck
# 3. Run tests
bun run test -- -t "test name"    # Single suite
bun run test:file -- "glob"       # Specific files
# 4. Lint before committing
bun run lint:file -- "file1.ts"   # Specific files
bun run lint                       # All files
# 5. Before creating PR
bun run lint:claude && bun run test
```

## 5. Tag @claude in PR Reviews
During code review, tags @claude on coworkers' PRs to add something to the CLAUDE.md as part of the PR. Uses the Claude Code GitHub action (`/install-github-action`). Example: "nit: use a string literal, not ts enum" -> "@claude add to CLAUDE.md to never use enums, always prefer literal unions"

This is their version of Compounding Engineering.

## 6. Plan Mode First
Most sessions start in Plan mode (`shift+tab` twice). If the goal is to write a Pull Request, uses Plan mode and goes back and forth with Claude until the plan is good. Then switches into auto-accept edits mode and Claude can usually 1-shot it. A good plan is the key.

## 7. Slash Commands for Inner Loops
Uses slash commands for every "inner loop" workflow done many times a day. Saves from repeated prompting and makes it so Claude can use these workflows too. Commands are checked into git and live in `.claude/commands/`.

Example: `/commit-push-pr` - Commit, push, and open a PR.

## 8. Custom Subagents
Uses a few subagents regularly:
- **code-simplifier**: simplifies code after Claude is done working
- **verify-app**: has detailed instructions for testing Claude Code end to end
- **oncall-guide**: for on-call workflows

Located in `.claude/agents/`. Thinks of subagents as automating the most common workflows.

## 9. PostToolUse Hook for Formatting
Uses a PostToolUse hook to format Claude's code. Claude usually generates well-formatted code out of the box, and the hook handles the last 10% to avoid formatting errors in CI later.

```json
"PostToolUse": [
  {
    "matcher": "Write|Edit",
    "hooks": [
      {
        "type": "command",
        "command": "bun run format || true"
      }
    ]
  }
]
```

## 10. Permissions Instead of --dangerously-skip-permissions
Doesn't use `--dangerously-skip-permissions`. Instead, uses `/permissions` to pre-allow common bash commands that are safe in the environment, avoiding unnecessary permission prompts. Most of these are checked into `.claude/settings.json` and shared with the team.

Example allowed commands: `Bash(bun run test:*)`, `Bash(bun run typecheck:*)`, `Bash(bun test:*)`, `Bash(cc:*)`, `Bash(comm:*)`, `Bash(find:*)`, etc.

## 11. MCP for All Tools
Claude Code uses all his tools via MCP: searches and posts to Slack (via MCP server), runs BigQuery queries to answer analytics questions (using bq CLI), grabs error logs from Sentry, etc.

Slack MCP configuration checked into `.mcp.json` and shared with team:
```json
{
  "mcpServers": {
    "slack": {
      "type": "http",
      "url": "https://slack.mcp.anthropic.com/mcp"
    }
  }
}
```

## 12. Long-Running Tasks
For very long-running tasks, either:
- (a) Prompt Claude to verify its work with a background agent when it's done
- (b) Use an agent Stop hook to do that more deterministically
- (c) Use the ralph-wiggum plugin (originally by @GeoffreyHuntley)

Also uses `think` prompting for extra reasoning on complex tasks.

## 13. Claude Tests Every Change
Claude tests every single change landed to the Claude Code codebase.
