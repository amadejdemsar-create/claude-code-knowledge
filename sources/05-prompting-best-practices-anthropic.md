# Anthropic's Prompting Best Practices - 10 Tips for Claude Code

**Source:** https://x.com/AlexFinn/status/2007585393584353688
**Author:** Alex Finn (@AlexFinn)
**Date:** Jan 3, 2026
**Category:** Best Practices / Pro Tips

---

Summary of Anthropic's official prompting best practices docs, tested and distilled into 10 actionable tips.

## 1. Use GitHub Early and Often
Once you connect Claude Code to git, it gets access to your entire commit history. It will know about EVERY change you've ever committed, making it much more powerful. This reduces hallucinations significantly. Connect CC to git right away.

## 2. Load Up Your Context
CC's context management has gotten massive improvements. It now compacts intelligently, allowing you to keep context for a long time. Don't be afraid to load context by having CC review your architecture at the beginning of sessions.

## 3. Adjust Eagerness
Claude Opus 4.5 is an incredibly eager model. It asks for forgiveness, not permission. If you don't like this eagerness, Anthropic wrote a new CLAUDE rule you can put in your rules file:

```xml
<do_not_act_before_instructions>
Do not jump into implementation or changes files unless clearly instructed to make changes. When the user's intent is ambiguous, default to providing information, doing research, and providing recommendations rather than taking action. Only proceed with edits, modifications, or implementations when the user explicitly requests them.
</do_not_act_before_instructions>
```

## 4. Be Careful with the Word 'Think'
'Think' is the only word you can use with Claude Code that triggers extra token usage. If you are tight on budget or tokens, avoid using the word 'think', 'think more', or 'ultrathink'. Instead use 'evaluate' or 'consider'.

## 5. Use Images
Claude Opus 4.5 is the greatest vision model of all time. It can analyze and understand images better than any other model. Make sure to use as many images as you can. Good for debugging and visual inspiration. Just paste directly into CC.

## 6. Parallel Tool Calls
Claude Opus 4.5 is the best model ever at tool usage. It's incredibly agentic. With this CLAUDE rule, you can have CC use tools in parallel, increasing efficiency:

```xml
<use_parallel_tool_calls>
If you intend to call multiple tools and there are no dependencies between the tool calls, make all of the independent tool calls in parallel. Prioritize calling tools simultaneously whenever the actions can be done in parallel rather than sequentially. For example, when reading 3 files, run 3 tool calls in parallel to read all 3 files into context at the same time. Maximize use of parallel tool calls where possible to increase speed and efficiency. However, if some tool calls depend on previous calls to inform dependent values, do NOT call these tools in parallel and instead call them sequentially. Never use placeholders or guess missing parameters in tool calls.
</use_parallel_tool_calls>
```

## 7. Reduce Hallucinations
Claude's eagerness has some downsides. It will often make edits to files without truly understanding its purpose first. Use this CLAUDE rule:

```xml
<investigate_before_answering>
Make sure to investigate and read relevant files BEFORE answering questions about the codebase. Never make any claims about code before investigating unless you are certain of the correct answer - give grounded and hallucination-free answers.
</investigate_before_answering>
```

## 8-10. Additional Tips
- Implement these tips and Claude Code will perform significantly better
- There's much more in Anthropic's full prompting guide
- The full Anthropic prompting guide for Claude Code is linked in Anthropic's docs
