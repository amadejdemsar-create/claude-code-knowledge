# The Complete Guide to CLAUDE.md

**Source:** https://x.com/CodevolutionWeb/status/2010715163859542086
**Author:** Vishwas (@CodevolutionWeb)
**Date:** Jan 12, 2026
**Category:** Builder Tools

---

One file. Loaded before every conversation. If you're using Claude Code, this is where your setup time pays off most.

## Why You Need a CLAUDE.md File
Claude starts every session with no memory. It doesn't know your code style, how to run tests, or your branch naming convention. CLAUDE.md fixes that - Claude reads it automatically so your preferences persist across sessions.

## How to Create Your CLAUDE.md File
- Fastest way: run `/init` in your project directory
- Use /init as starting point, delete what you don't need

**Locations:**
- **Project root**: Most common. Commit to version control.
- **.claude/CLAUDE.md**: Alternative subdirectory location
- **~/.claude/CLAUDE.md**: User-level defaults for all projects
- **CLAUDE.local.md**: Personal preferences, add to .gitignore

Filename is case-sensitive: must be exactly `CLAUDE.md`.

## How to Structure It

### The Essentials
- **Project context**: One-liner that orients Claude
- **Code style**: Specific formatting and pattern preferences
- **Commands**: How to run tests, build, lint, deploy
- **Gotchas**: Project-specific warnings

### Example
```markdown
# Project: ShopFront
Next.js 14 e-commerce app with App Router, Stripe, Prisma ORM.

## Code Style
- TypeScript strict mode, no `any` types
- Use named exports, not default exports
- CSS: Tailwind utility classes, no custom CSS files

## Commands
- `npm run dev`: Start dev server (port 3000)
- `npm run test`: Run Jest tests
- `npm run lint`: ESLint check
- `npm run db:migrate`: Run Prisma migrations

## Architecture
- `/app`: App Router pages and layouts
- `/components/ui`: Reusable UI components
- `/lib`: Utilities and shared logic

## Important Notes
- NEVER commit .env files
- Stripe webhook handler must validate signatures
- Product images stored in Cloudinary, not locally
```

### Length
Under 300 lines. Shorter is better. Context tokens are precious.

## The @imports System
Reference other files with `@path/to/file` syntax:
- `@docs/style-guide.md`
- `@~/.claude/my-preferences.md`
- Imports can be recursive (use sparingly)

Keep essentials in CLAUDE.md, move detailed guidance to separate files.

## Modular Rules with .claude/rules/
Split instructions into focused rule files:
```
.claude/
  CLAUDE.md
  rules/
    code-style.md
    testing.md
    security.md
```
All markdown files in .claude/rules/ are automatically loaded.

## Subdirectory CLAUDE.md Files
Claude picks up CLAUDE.md in subdirectories when working in that part of the codebase. Useful for monorepos.

## Maintenance

### Adding Instructions as You Work
When Claude makes wrong assumptions, tell it to add the correction to CLAUDE.md. Builds organically over time.

### Periodic Review
Every few weeks, ask Claude to review and optimize. Delete obsolete, consolidate redundant, clarify ambiguous.

### Emphasis for Critical Instructions
"IMPORTANT:" or "YOU MUST" increases odds Claude pays attention. Use sparingly.

### Improving from Code Reviews
When PRs reveal undocumented conventions, add to CLAUDE.md. Tag @claude in PR comments with `/install-github-action`.

## Best Practices
- Open with one-liner explaining the project
- Make code style specific and actionable
- Include key commands (test, build, lint, deploy)
- Detail gotchas enough to prevent mistakes
- Keep under 300 lines
- Move detailed guidance to @imported files
- Remove outdated/conflicting instructions
- Mark truly critical rules with emphasis
- Add instructions as you work
- Update from PR reviews
- Review periodically
