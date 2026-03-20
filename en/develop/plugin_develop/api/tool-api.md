# Tool API

The Tool API module provides functionality for obtaining and managing tool instances, allowing plugins to access tools registered in the system.

## Import Method

```python
from src.plugin_system.apis import tool_api
# or
from src.plugin_system import tool_api
```

## Main Functions

### 1. Get Tool Instance

```python
def get_tool_instance(tool_name: str) -> Optional[BaseTool]:
```

Get tool instance with specified name.

**Args**:
- `tool_name`: Tool name string

**Returns**:
- `Optional[BaseTool]`: Tool instance, returns None if tool doesn't exist

### 2. Get LLM Available Tool Definitions

```python
def get_llm_available_tool_definitions():
```

Get all LLM available tool definition list.

**Returns**:
- `List[Tuple[str, Dict[str, Any]]]`: Tool definition list, each element is a tuple of `(tool name, tool definition dictionary)`
  - For specific definitions, refer to the tool definition format in [tool-components.md](../tool-components.md).

#### Example:

```python
# Get all LLM available tool definitions
tools = tool_api.get_llm_available_tool_definitions()
for tool_name, tool_definition in tools:
    print(f"Tool: {tool_name}")
    print(f"Definition: {tool_definition}")
```

## Notes

1. **Tool Existence Check**: Check if tool instance is None before using
2. **Permission Control**: Some tools may have usage permission restrictions
3. **Asynchronous Calls**: Most tool methods are asynchronous, require await
4. **Error Handling**: Implement proper exception handling when calling tools