# 04 · Model Context Protocol (MCP)

For AI to do things in the real world, it needs tools. Connecting every tool to every model from scratch is painful. MCP is the better way.

## The M×N problem

To connect one tool to your AI, you build three things:

1. A description of what the tool does, so the model understands it.
2. The code that calls the tool's API.
3. The handling for what comes back.

Want to connect Gmail? Same three things. Notion? Same three things again.

<p align="center">
  <img src="images/without-mcp.png" alt="Without MCP, the AI app needs its own tool descriptions, tool call logic and response handling for each of GitHub, Gmail and Notion." width="700">
</p>

If you switch AI apps, say ChatGPT instead of Claude, each one has its own format for tool descriptions. You can't reuse any of it, so you start over. Want ChatGPT to talk to GitHub? Build all three. Want Claude to do the same? Build all three again.

<p align="center">
  <img src="images/per-model-tool-formats.png" alt="Claude and ChatGPT each connect to GitHub, Gmail and Notion separately, with tool descriptions written in Claude's format and in ChatGPT's format." width="700">
</p>

Two models and three tools means six separate integrations to build and maintain. That's the M×N problem, and MCP turns it into M+N.

<p align="center">
  <img src="images/before-after-mcp.png" alt="Before MCP, M apps connect to N tools directly, giving M times N integrations. After MCP, every app and tool connects through MCP, giving M plus N integrations." width="700">
  <br>
  <sub>Image: Daily Dose of Data Science</sub>
</p>

## How MCP works

<p align="center">
  <img src="images/mcp-flow.png" alt="MCP flow: the MCP host sends the prompt and tools to the LLM, the LLM suggests a tool, the MCP client communicates with the MCP server, the server calls the tool on GitHub, and results return the same way." width="700">
</p>

### MCP host

The host is your AI app, like Claude Desktop or Cursor. It's the entry point. You send a prompt, the model decides which tool it needs, and the host hands the call off.

<p align="center">
  <img src="images/mcp-host.png" alt="The MCP host is the AI app, such as Claude or ChatGPT." width="700">
</p>

### MCP client

The client lives inside the host and maintains the connection to the server. It fetches the tool descriptions, forwards the tool calls and gets the results back. It holds no logic and runs nothing. It's only the pipe.

<p align="center">
  <img src="images/mcp-clients-and-servers.png" alt="One MCP host with three MCP clients, each connected to its own MCP server for GitHub, Gmail and Notion." width="700">
</p>

### MCP server

Before MCP, all the tool logic lived inside your app, and you were the middleman. GitHub's API calls and the response handling all sat in your code.

The MCP server pulls that out. GitHub publishes one MCP server with the tool descriptions and API code bundled together. Your app connects to it and everything flows automatically. Instead of writing the GitHub connector yourself, you use one that already exists.

The connector code that used to live inside every app is now one standalone process, and any AI app can point to it and use it. Tool owners publish their server once, and nobody else has to write that integration again.

## Build once, connect everywhere

MCP is an agreed standard. Instead of every AI app building its own connector to every tool, you build one MCP server per tool, and any app that speaks MCP can connect to it. M×N becomes M+N.

<p align="center">
  <img src="images/mcp-standard.png" alt="Claude and ChatGPT both connect through MCP to GitHub, Gmail and Notion." width="700">
</p>

REST APIs became the standard for the web and USB became the standard for hardware. MCP is that standard for AI agents.
