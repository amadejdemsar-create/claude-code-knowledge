# The Complete Claude Code Tutorial (Level 1)

**Source:** https://x.com/eyad_khrais/status/2010076957938188661
**Author:** Eyad (@eyad_khrais) - SWE 7 years (Amazon, Disney, Capital One), CTO building agents
**Date:** Jan 10, 2026
**Category:** Best Practices / Pro Tips

---

## Think First
- 10/10 times, plan mode output is significantly better than just talking
- Shift+Tab twice to enter plan mode
- Before asking Claude to build a feature, think about the architecture
- Be specific: "Build email/password auth using the existing User model, store sessions in Redis with 24hr expiry, add middleware for /api/protected" vs "build me an auth system"
- If you lack experience: 1) Start learning 2) Have deep back-and-forth with an LLM about system design

## CLAUDE.md
- Markdown file Claude reads at the start of every session
- Keep it short: Claude reliably follows ~150-200 instructions, system prompt uses ~50
- Make it specific to YOUR project - tell it the weird stuff, not obvious things
- Tell it WHY not just what: "Use TypeScript strict mode because we've had production bugs from implicit any types"
- Update constantly: Press # key to add instructions
- Bad CLAUDE.md = documentation for a new hire. Good CLAUDE.md = notes for yourself with amnesia tomorrow

## Context Window Limitations
- Opus 4.5: 200K token window
- Quality starts deteriorating at 20-40% usage
- Compaction doesn't magically restore quality
- **Tips:**
  - One conversation per feature/task - don't bleed contexts
  - External memory: write plans to SCRATCHPAD.md or plan.md files
  - Copy-paste reset: copy important stuff, /compact, /clear, paste back only what matters
  - Know when to /clear - often better than struggling through degraded context

## Prompts Are Everything
- Be specific about what you want
- Tell it what NOT to do - Claude 4.5 likes to overengineer
- Give it context about why: "needs to be fast because it runs on every request"
- Always cross-reference what Claude produces

## Bad Input == Bad Output
- If output sucks with a good model, your input sucks
- **Sonnet**: Faster, cheaper. Great for execution tasks where path is clear
- **Opus**: Slower, more expensive. Better for complex reasoning and planning
- **Workflow**: Use Opus to plan, Shift+Tab to switch to Sonnet for implementation

## MCP, Tools, and Configurations
- **MCP**: Connect to Slack, GitHub, databases, APIs. If you constantly copy info from one place to Claude, there's probably an MCP server for it
- **Hooks**: Run code before/after Claude makes changes (Prettier, type checking). Catches problems immediately
- **Custom slash commands**: `.claude/commands/` folder with markdown files, run with `/commandname`
- If you have Pro Max, try everything - you're paying for it

## When Claude Gets Stuck
- /clear and start fresh
- Simplify - break into smaller pieces
- Show instead of tell - write a minimal example
- Reframe the problem differently
- If you've explained the same thing 3 times, change something

## Build Systems
- `-p` flag for headless mode - runs prompt without interactive interface
- Script it, pipe output, chain with bash commands
- Enterprises use for: auto PR reviews, support tickets, logging, documentation
- **Flywheel**: Claude mistake -> review logs -> improve CLAUDE.md -> Claude gets better
