# 5-Day-AI-Agents-Intensive-Vibe-Coding-Course-With-Google
Contains code, material relevant to AI Agents

# Day 1

Paradigm shift from SDLC to agent AI engineering
Google Agent Platform : Antigravity


# Day 3

Agent Skills

Issue in AI Agent development:

- Context Rot : too many instructions in single prompt, poor results
- agents skills tell how to do things, lacking in LLM (procedural memory)
- instead of multi agent, create 1 agent that takes up role accordingly
- portability

Structure

- Describe in SKILL.md
- Scripts. Deterministic work (parsing exports, math, formatting) lives in scripts/. The model
decides what to do; the script does the heavy lifting.
- References. Knowledge that is only relevant once the skill is running (domain principles,
definitions, edge case handling) lives in references/ and loads on demand.
- Assets. Templates and schemas live in assets/.
- Example : Anthropic's skill-creator


MCP vs Skills 

Model Context Protocol is about reach: an MCP server connects the agent to an external system (Drive, Salesforce, BigQuery, or an
internal API).
A Skill is about know-how: it teaches the agent how to think about a particular
kind of work. When a Skill needs data, it tells the agent to call a tool, typically one provided by
an MCP server.


From one side AGENTS.md is always loaded within the project; Skills
load on demand. 

Evaluating skills

Fortunately, these failures
are predictable and fall into four distinct modes:
1. Trigger Failure: The wrong skill fires, or the correct one fails to fire.
2. Execution Failure: The skill triggers correctly, but produces incorrect output or errant
tool calls.
3. Token Budget Failure: A massive skill body crowds the context window, degrading
performance on unrelated turns.
4. Regression: A newly added skill overlaps with an existing one, breaking previously
working routing.

The evaluation toolkit

Eval-as-Unit-Test
Golden Dataset
LLM-as-Judge
dversarial /Red-Team
Canary / Shadow Mode


your SKILL.md **description**, the only thing the model sees during routing, must pass four checks:
- Testable specificity: You must write 3 positive and 3 negative triggers.
- Clarity: Ambiguous queries don't overlap with adjacent skills.
- Execution fidelity: It describes actual performance, not aspirational behavior.
- Rephrasing stability: It routes consistently regardless of how the user phrases the intent.


Token budget: isolation is a trap
- Never evaluate a skill purely in isolation. Agents in production co-load 5 to 15 skills simultaneously.
- A skill body exceeding 5,000 tokens might work perfectly alone,
- but it will cause context rot when co-loaded

# How to decide among hundereds of skills that exist

- First, prefer first-party skills for vendor-specific tools. Google's BigQuery skill, the official
Stripe skill, anything written by the people who built the underlying system. They will be
more correct and more maintained than community alternatives.
- Second, pin everything you depend on. Community skills evolve, and an unpinned
dependency that worked yesterday can fail tomorrow.
- Third, audit before adopting. A skill is code that runs in your context. Treat it like any other
dependency, with the same supply-chain hygiene.


Format to follow


---
name: skill-name
description: | [What it does in one verb-led sentence.] Use this skill when the user [trigger phrase 1], [trigger phrase 2], or [trigger phrase 3].Do NOT use for [anti-trigger 1] or [anti-trigger 2].
version: 1.0.0
license: MIT
allowed-tools: [Optional] Read Bash Write
metadata:
 author: [Optional] your-handle
---
# Skill Name
## When to use
- [Concrete scenario]
- [Concrete scenario]
## When NOT to use
- [Out-of-scope scenario]
## Workflow
1. [Step]
2. [Step]
3. See `references/advanced.md` for [edge case].

## Examples
- Input: "..." → Output: "..."
## Output format
- Use `assets/template.md` etc.
## Anti-patterns to avoid
- Don't [...]


Folder Structure

skill_name/
├── SKILL.md # Required: YAML frontmatter + markdown instructions
├── scripts/ # Optional: executable helper scripts (Python, Bash)
├── references/ # Optional: supplementary context loaded as needed
├── assets/ # Optional: files used in output (templates,resources)
├── ... # Any additional files or directories
