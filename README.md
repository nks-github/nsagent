# NSAgent - A Simple Agentic Framework

A lightweight, expandable agentic application built as a Python notebook. Given a task, the agent works through three blended phases: **Gather Context → Take Action → Verify Results**.

## Features

- 🔄 Three-phase agentic loop (context → action → verify) that blend together
- 🔧 Expandable tool and skill system with decorators
- 📦 Out-of-the-box tools: file I/O, shell commands, web fetch, Python exec
- 🌐 MCP server integration for browser automation via [Playwright MCP](https://github.com/microsoft/playwright-mcp)
- 🧠 Works with OpenAI-compatible APIs (Azure OpenAI, OpenAI, local models)

## Quick Start

1. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```

2. Set your API key:
   ```bash
   set OPENAI_API_KEY=your-key-here
   ```

3. Open `nsagent.ipynb` in VS Code or Jupyter and run all cells.

## Architecture

```
┌─────────────────────────────────────┐
│            Agent Loop               │
│  ┌───────────┐                      │
│  │  Gather   │──► decides what      │
│  │  Context  │   info is needed     │
│  └─────┬─────┘                      │
│        │ (blended)                  │
│  ┌─────▼─────┐                      │
│  │   Take    │──► calls tools/      │
│  │  Action   │   skills/MCP         │
│  └─────┬─────┘                      │
│        │ (blended)                  │
│  ┌─────▼─────┐                      │
│  │  Verify   │──► checks results,   │
│  │  Results  │   loops if needed    │
│  └───────────┘                      │
└─────────────────────────────────────┘
         │
    ┌────┴─────────────────────┐
    │      Tool Registry       │
    ├──────────┬───────────────┤
    │ Built-in │ Custom Skills │
    │ Tools    │ (decorators)  │
    ├──────────┼───────────────┤
    │ MCP Servers              │
    │ (Playwright, etc.)       │
    └──────────────────────────┘
```

## Adding Custom Tools

```python
@agent.tool(description="Describe what this tool does")
def my_tool(param1: str, param2: int = 5) -> str:
    return f"Result: {param1}, {param2}"
```

## Adding Custom Skills

```python
@agent.skill(name="research", description="Deep research on a topic")
def research_skill(topic: str) -> str:
    # A skill is a higher-level capability that may use multiple tools
    results = agent.use_tool("web_fetch", url=f"https://api.example.com/search?q={topic}")
    return f"Research findings: {results}"
```

## License

MIT
