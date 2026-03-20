# LLM API

The LLM API module provides functionality for interacting with large language models, allowing plugins to use system-configured LLM models for content generation.

## Import Method

```python
from src.plugin_system.apis import llm_api
# or
from src.plugin_system import llm_api
```

## Main Functions

### 1. Query Available Models
```python
def get_available_models() -> Dict[str, TaskConfig]:
```
Get all available model configurations.

**Return:**
- `Dict[str, TaskConfig]`: Model configuration dictionary, key is model name, value is model configuration object.

### 2. Generate Content with Model
```python
async def generate_with_model(
    prompt: str, model_config: TaskConfig, request_type: str = "plugin.generate", **kwargs
) -> Tuple[bool, str, str, str]:
```
Generate content using the specified model.

**Args:**
- `prompt`: Prompt text.
- `model_config`: Model configuration object (obtained from `get_available_models`).
- `request_type`: Request type identifier, default is `"plugin.generate"`.
- `**kwargs`: Other model-specific parameters, such as `temperature`, `max_tokens`, etc.

**Return:**
- `Tuple[bool, str, str, str]`: Returns a tuple containing (success status, generated content, reasoning process, model name).