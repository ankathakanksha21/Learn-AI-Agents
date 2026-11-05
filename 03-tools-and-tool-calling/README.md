# 03 · Tools and Tool Calling

## Why agents need tools

An agent isn't powerful if it can't interact with the real world, which means acting and doing things. That ability to act comes entirely from tools.

LLMs predict text based on their training data, so their knowledge has a cutoff. Ask one about yesterday's stock price and it doesn't know. Tell it to update a record in your database and it can't touch it.

<p align="center">
  <img src="images/llm-vs-agent-weather.png" alt="Asked for the current weather in Paris, the LLM hallucinates 30°C and sunny, while an agent with a web search tool returns 10°C and rain." width="500">
</p>

## What tools are

A tool is anything your LLM can invoke to interact with the outside world: an API, a database query or an internal function written in code.

Give an LLM tools and generation turns into execution. Instead of only answering, it does the work.

## How an agent uses tools

1. The user gives a task.
2. The LLM understands it and plans.
3. It structures the plan into a tool call, such as `get_status(order_id=12345)` or `send_email(to=akanksha@gmail.com)`.
4. The tool runs externally (a Python function, an API call or a database query) and returns a result.
5. The LLM looks at the result and decides whether to call another tool or answer the user.
6. The loop continues until the task is done.

<p align="center">
  <img src="images/tool-calling-loop.png" alt="The tool-calling loop: the user gives a task, the LLM understands and plans, structures a tool call such as get_status(order_id=12345), the tool runs externally as a Python function, API or database query, and the LLM receives the result and either calls another tool or answers the user." width="500">
</p>

The LLM is still only generating text. That text gets structured and executed externally through APIs, functions or database queries. The results come back in and the cycle repeats. That loop of reasoning and action is what makes it an agent.

<p align="center">
  <img src="images/llm-parser-tool-observation.png" alt="The agent loop: a user question goes to the LLM, a parser turns its output into a tool call, the tool runs and produces an observation, and the loop repeats until the LLM gives a final response." width="500">
</p>

## How the LLM knows which tool to use

You maintain a tool registry. Every tool in it has a name, a description of when to use it, input parameters and expected output.

The LLM gets this metadata and uses it to reason about which tool to call and how. This is tool selection, also called tool routing.

<p align="center">
  <img src="images/tool-registry-loop.png" alt="The same loop with a tool registry: while the LLM understands and plans, it checks the registry of tool names, descriptions, inputs and outputs to select the right tool." width="500">
</p>

An example tool definition:

<p align="center">
  <img src="images/tool-definition-example.png" alt="Example tool definition in JSON for get_weather, with a description of fetching weather for a location and a required string parameter called city." width="500">
</p>
