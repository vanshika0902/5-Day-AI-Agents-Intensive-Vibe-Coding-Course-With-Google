# 5-Day-AI-Agents-Intensive-Vibe-Coding-Course-With-Google
Contains code, material relevant to AI Agents


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


your SKILL.md description, the
only thing the model sees during routing, must pass four checks:
• Testable specificity: You must write 3 positive and 3 negative triggers.
• Clarity: Ambiguous queries don't overlap with adjacent skills.
• Execution fidelity: It describes actual performance, not aspirational behavior.
• Rephrasing stability: It routes consistently regardless of how the user phrases the intent.
