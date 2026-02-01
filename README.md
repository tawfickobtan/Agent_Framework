"# Agent Framework

A simple Python framework for building AI agents using OpenAI's API with tool-calling capabilities.

## Features

- Easy integration with OpenAI's chat completions API
- Support for tool calling (function calling)
- Customizable system prompts
- Message history management
- Function registry for handling tools

## Requirements

- Python 3.7+
- OpenAI API key
- `openai` Python package

## Installation

1. Clone the repository:
   ```
   git clone <repository-url>
   cd agent-framework
   ```

2. Install dependencies:
   ```
   pip install openai
   ```

## Usage

### Basic Setup

```python
from agent import Agent

# Initialize the agent
agent = Agent(
    base_url="https://api.openai.com/v1",
    api_key="your-openai-api-key",
    model="gpt-4",
    system_prompt="You are a helpful AI assistant."
)
```

### Adding Tools

```python
# Define tool descriptions
tools = [
    {
        "type": "function",
        "function": {
            "name": "get_weather",
            "description": "Get the current weather for a location",
            "parameters": {
                "type": "object",
                "properties": {
                    "location": {
                        "type": "string",
                        "description": "The city and state, e.g. San Francisco, CA"
                    }
                },
                "required": ["location"]
            }
        }
    }
]

# Define function registry
def get_weather(location: str) -> str:
    # Implement your weather function
    return f"The weather in {location} is sunny."

function_registry = {
    "get_weather": get_weather
}

# Create agent with tools
agent = Agent(
    base_url="https://api.openai.com/v1",
    api_key="your-openai-api-key",
    model="gpt-4",
    toolsDesc=tools,
    function_registry=function_registry
)
```

### Interacting with the Agent

```python
# Send a prompt
response = agent.prompt("What's the weather like in New York?")
print(response)
```

### Handling Tool Call Outputs

The `prompt` method returns a list containing the AI's response and, if applicable, the results of any tool calls.

- If no tool calls are made, the list contains one element: the AI's message object.
- If tool calls are made, the list contains two elements: the AI's message object and the tool result dictionary.

Example:

```python
response_list = agent.prompt("What's the weather like in New York?")

# Access the AI's response
ai_response = response_list[0]
print("AI Response:", ai_response.content)

# Check if tool calls were made
if len(response_list) > 1:
    tool_result = response_list[1]
    print("Tool Result:", tool_result['content'])
```

The tool result is a dictionary with:
- `role`: "tool"
- `tool_call_id`: The ID of the tool call
- `content`: The output of the executed function

### Resetting Conversation

```python
agent.reset_messages()  # Clears message history except system prompt
```

## API Reference

### Agent Class

- `__init__(base_url, api_key, model, toolsDesc=[], function_registry={}, system_prompt="You are an AI Agent")`: Initialize the agent.
- `add_message(role, content)`: Add a message to the conversation.
- `complete()`: Get a completion from the model.
- `handle_tool_call(tool_call)`: Handle a tool call.
- `prompt(user_input)`: Send user input and get response.
- `reset_messages()`: Reset the message history.

## License

MIT License" 
