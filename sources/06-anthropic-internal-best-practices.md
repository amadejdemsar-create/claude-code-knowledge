# Claude Code: Best Practices for Agentic Coding (from Anthropic)

**Source:** https://x.com/alexalbert__/status/1914333320877584397
**Author:** Alex Albert (@alexalbert__) - Claude Relations at Anthropic
**Date:** Apr 21, 2025
**Category:** Best Practices / Pro Tips (Official Anthropic)
**Full docs:** https://code.claude.com

---

What Anthropic learned using Claude Code internally. Most effective patterns found (many apply to coding with LLMs generally).

## 1. CLAUDE.md Files Are the Main Hidden Gem
Simple markdown files that give Claude context about your project - bash commands, code style, testing patterns. Claude loads them automatically. You can add to them with the # key.

## 2. The Explore-Plan-Code Workflow
Instead of letting Claude jump straight to coding, have it read files first, make a plan (add "think" for deeper reasoning), then implement. Quality improves dramatically with this approach.

## 3. Test-Driven Development Works Very Well
Write tests, commit them, let Claude implement until they pass. TDD keeps Claude focused on the actual goal.

## 4. Keyboard Shortcuts
- **ESC** interrupts Claude
- **Double ESC** edits previous prompts
These shortcuts save lots of wasted work when you spot Claude heading the wrong direction.

## 5. Codebase Onboarding
Anthropic uses Claude for codebase onboarding. Engineers ask it "why does this work this way?" and it searches git history and code for answers. This has cut onboarding time significantly.

## 6. Headless Mode for Automation
`claude -p` handles everything from PR labeling to subjective code review beyond what linters catch. Or try `--dangerously-skip-permissions` mode. Spin up a container and let Claude loose on something like linting or boilerplate generation.

## 7. Multi-Claude Workflow
One instance writes code, another follows up behind and reviews. You can also use git worktrees for parallel Claude sessions on different features.

## Further Reading
Check out the full blog post and best practices at: https://code.claude.com
