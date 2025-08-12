# Deep Agents

Build AI agents that handle complex, multi-step tasks with intelligent planning and execution.

## Installation

```bash
pip install deepagents
```

## Quick Start

```python
from deepagents import create_deep_agent

agent = create_deep_agent(
    tools=[your_functions],
    instructions="Your agent's behavior and goals"
)

# Agents are LangGraph graphs - use all LangGraph features
result = agent.invoke({"messages": [{"role": "user", "content": "Your task"}]})
```

## Configuration

### Tools (Required)
List of functions or LangChain `@tool` objects your agent can use.

### Instructions (Required)
Custom behavior guidelines that work with the built-in [system prompt](src/deepagents/prompts.py).

### Sub-agents (Optional)
Create specialized agents for specific tasks:

```python
subagents = [{
    "name": "researcher",
    "description": "Handles research tasks", 
    "prompt": "You excel at research...",
    "tools": ["search_tool"]  # Optional - defaults to all tools
}]

agent = create_deep_agent(tools, instructions, subagents=subagents)
```

### Custom Model (Optional)
Defaults to `claude-sonnet-4-20250514`. Use any LangChain model:

```python
from langchain_ollama import ChatOllama

agent = create_deep_agent(
    tools, instructions, 
    model=ChatOllama(model="gpt-oss:20b")
)
```

## Built-in Features

### Planning System
Automatic task breakdown and progress tracking to keep agents focused.

### File Operations
Virtual file system with `ls`, `read_file`, `edit_file`, `write_file`:

```python
result = agent.invoke({
    "messages": [...],
    "files": {"data.txt": "content"}  # Pass files in
})

updated_files = result["files"]  # Get files back
```

### Sub-agent Delegation  
Main agent intelligently delegates to specialized sub-agents. Includes a default general-purpose sub-agent plus any custom ones you define.