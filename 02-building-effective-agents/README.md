# 02 · Building Effective Agents

## Start simple

Anthropic studied dozens of AI applications in production. They recommend starting with the simplest solution possible and adding complexity only when it's needed.

<p align="center">
  <img src="images/when-to-use-agents-excerpt.png" alt="Excerpt from Anthropic's guide, When (and when not) to use agents: find the simplest solution possible and only increase complexity when needed. Agentic systems trade latency and cost for better task performance. Workflows offer predictability for well-defined tasks, agents suit flexibility and model-driven decisions at scale, and for many applications optimizing single LLM calls with retrieval and in-context examples is enough." width="700">
</p>

Jumping straight to a complex multi-agent system makes everything harder for no reason.

## Workflows

Most problems don't need an agent. They need a workflow, where the LLM follows a predefined path and you stay in control.

### Prompt chaining

Break a task into a sequence of steps. Each LLM call handles one step and passes its output to the next. One LLM trying to do everything at once gives you mediocre output.

<p align="center">
  <img src="images/prompt-chaining.png" alt="Prompt chaining workflow: LLM Call 1 feeds a gate. On pass, the output goes through LLM Call 2 and LLM Call 3 to the result. On fail, the workflow exits." width="700">
</p>

### Orchestrator-workers

A central LLM dynamically breaks down the task, delegates each part to worker LLMs and synthesizes the results.

<p align="center">
  <img src="images/orchestrator-workers.png" alt="Orchestrator-workers workflow: an orchestrator sends parts of the task to LLM Calls 1, 2 and 3, and a synthesizer combines their results." width="700">
</p>

## Agents

If workflow patterns don't fit your problem, that's when you need an agent.

The LLM sits in a loop. It takes an action, observes what happened in the environment and decides the next action based on that feedback, until the task is done.

<p align="center">
  <img src="images/autonomous-agent.png" alt="Autonomous agent: an LLM call takes actions on the environment, gets feedback, and loops until it stops. A human can interact with it along the way." width="700">
</p>

## Tool design

Tool design can make or break your agent in production.

<p align="center">
  <img src="images/tool-types.png" alt="Types of tools an agent can use: web, code, apps, data, functions and MCP." width="700">
</p>

Anthropic's own team spent more time optimizing tools than the agent prompt to eliminate failures.

<p align="center">
  <img src="images/swe-bench-tools-excerpt.png" alt="Excerpt from Anthropic's guide: while building their SWE-bench agent, they spent more time optimizing tools than the prompt. The model made mistakes with relative filepaths, so they changed the tool to always require absolute filepaths, and the model then used it flawlessly." width="700">
</p>

If your tools are poorly defined, your agent will misuse them consistently. Write every tool definition like an explainer for a junior engineer. Include examples, edge cases, the input format and clear boundaries.

## Further reading

[Building effective agents](https://www.anthropic.com/research/building-effective-agents) by Anthropic. The workflow and agent diagrams and both excerpts are from this guide.
