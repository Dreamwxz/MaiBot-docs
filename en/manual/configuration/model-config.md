---
title: Model Configuration
---

# Set Up the AI Brain

`model_config.toml` is the file used to configure MaiMai's "AI brain". It decides which LLM each MaiMai component uses.

We recommend assigning different models based on the characteristics of different tasks.

MaiMai's configuration must include one LLM model (or VLM), one VLM model, and one embedding model.

## Configuration File Structure

```toml
# Configuration file version
[inner]
version = "1.14.0"

# API provider list (AI service providers)
[[api_providers]]
name = "deepseek"
base_url = "https://api.deepseek.com/v1"
api_key = "sk-xxxxxxxxxxxxxxxxxxxxxxxx"
auth_type = "bearer"                    # Auth type: bearer/header/query/none

# Model list (which specific AI to use)
[[models]]
name = "deepseek-chat"
model_identifier = "deepseek-chat"
api_provider = "deepseek"
visual = false                          # Whether supports vision
price_in = 0.1                          # Input price (yuan/million tokens)
price_out = 0.2                         # Output price (yuan/million tokens)

# Task assignment (different work uses different AI)
[model_task_config.replyer]
model_list = ["deepseek-chat"]
max_tokens = 1024
temperature = 0.3
slow_threshold = 15.0                   # Slow request threshold (seconds)
selection_strategy = "balance"          # Model selection strategy: balance/random/sequential
```

## API Providers [[api_providers]]

API providers represent the services that provide LLM capabilities.

### Basic Configuration (Required)

| 🏷️ Item | 💡 What It Is | 📝 How to Fill |
|-----------|----------|----------|
| `name` | Provider name | Choose a name yourself, such as "deepseek" or "openai" |
| `base_url` | API address | The URL provided by the service provider |
| `api_key` | Key | The key you get after registration |
| `auth_type` | Authentication type | `bearer` (default), `header`, `query`, `none` |

### Common Provider Configuration Examples

::: code-group

```toml [DeepSeek]
[[api_providers]]
name = "deepseek"
base_url = "https://api.deepseek.com/v1"
api_key = "sk-your-key"
client_type = "openai"
auth_type = "bearer"
```

```toml [OpenAI]
[[api_providers]]
name = "openai"
base_url = "https://api.openai.com/v1"
api_key = "sk-your-key"
client_type = "openai"
auth_type = "bearer"
```

```toml [Alibaba Bailian]
[[api_providers]]
name = "aliyun"
base_url = "https://dashscope.aliyuncs.com/compatible-mode/v1"
api_key = "sk-your-key"
client_type = "openai"
auth_type = "bearer"
```

```toml [ByteDance Volcengine]
[[api_providers]]
name = "volcengine"
base_url = "https://ark.cn-beijing.volces.com/api/v3"
api_key = "sk-your-key"
client_type = "openai"
auth_type = "bearer"
```

```toml [Custom Header Auth]
[[api_providers]]
name = "custom"
base_url = "https://api.example.com/v1"
api_key = "your-api-key"
client_type = "openai"
auth_type = "header"
auth_header_name = "X-API-Key"
auth_header_prefix = ""
```

```toml [Query Parameter Auth]
[[api_providers]]
name = "query_auth"
base_url = "https://api.example.com/v1"
api_key = "your-api-key"
client_type = "openai"
auth_type = "query"
auth_query_name = "key"
```

:::

> 📖 See: [Model Advanced Parameters](./model-extra-params#api-provider-advanced-config) for advanced auth, parameters, and runtime settings.

## Model List [[models]]

Models are specific LLMs, such as GPT-5.4, DeepSeek V4, and so on.

### Basic Configuration (Required)

| 🏷️ Item | 💡 What It Is | 📝 How to Fill |
|-----------|----------|----------|
| `name` | Model name | Choose a name yourself, such as "gpt-5" or "deepseek-v4" |
| `model_identifier` | Model ID | The specific model name provided by the service provider |
| `api_provider` | Which provider to use | Fill in the `name` from `api_providers` above |
| `visual` | Whether to enable vision | Only multimodal models can enable this option |
| `price_in` | Input price | Yuan per million tokens, default `0.0` |
| `price_out` | Output price | Yuan per million tokens, default `0.0` |

### Billing Configuration (Optional)

| 🏷️ Item | 💡 What It Is | 📝 How to Fill |
|-----------|----------|----------|
| `price_in` | Input price | Yuan per million tokens, default `0.0` |
| `price_out` | Output price | Yuan per million tokens, default `0.0` |
| `cache` | Enable cache billing | `false` (default), use cache_price_in for cache hits when enabled |
| `cache_price_in` | Cache input price | Yuan per million tokens, default `0.0` |

> 📖 See: [Model Advanced Parameters](./model-extra-params#model-advanced-parameters) for model-level overrides and advanced parameters.

> 📖 **See: [Model Extra Parameters (extra_params)](./model-extra-params)** — Complete guide covering thinking mode, reasoning depth, custom HTTP parameters, and more.
>

### Model Configuration Example

```toml
[[models]]
name = "deepseek-chat"                    # I call it "deepseek-chat"
model_identifier = "deepseek-chat"        # The provider also calls it this
api_provider = "deepseek"                 # Use the deepseek provider
visual = false                            # Cannot see images
price_in = 0.1                            # Input price: 0.1 yuan per million tokens
price_out = 0.2                           # Output price: 0.2 yuan per million tokens

[[models]]
name = "qwen3.5-vl"                       # Vision model, can see images
model_identifier = "qwen3.5-flash"
api_provider = "aliyun"
visual = true                             # ✅ Can see images
price_in = 0.05                           # Input price: 0.05 yuan per million tokens
price_out = 0.1                           # Output price: 0.1 yuan per million tokens

[[models]]
name = "gpt-4-cache"                      # Model with cache billing
temperature = 0.7                         # Model-level temperature setting
max_tokens = 2048                         # Model-level max tokens
model_identifier = "gpt-4"
api_provider = "openai"
visual = false
cache = true                              # Enable cache billing
cache_price_in = 0.025                    # Cache hit price
price_in = 0.1                            # Normal input price
price_out = 0.2                           # Output price
```

### Extra Parameters `extra_params`

`extra_params` is a model-level field for service-provider-specific API parameters. It is configured under a single `[[models]]` entry and only applies to that model.

```toml
[[models]]
name = "deepseek-v4-pro-nonthink"
model_identifier = "deepseek-v4-pro"
api_provider = "aliyun"
extra_params = {enable_thinking = "false"}
```

`extra_params` is an internal MaiBot configuration field. **It is not sent to the model provider as a top-level `extra_params` key**. Before the request is sent, the client converts it into the extra arguments supported by the corresponding SDK.

#### OpenAI-Compatible Client

When `api_provider.client_type = "openai"`, or when the default OpenAI-compatible client is used, `extra_params` is split using these rules:

| Syntax | Actual Use |
|------|----------|
| `headers` | Sent as request headers |
| `query` | Sent as URL query parameters |
| `body` | Merged into the request body |
| Other plain keys | Sent as extra request body fields |

For example:

```toml
extra_params = {
  headers = {"X-Trace-Id" = "test-001"},
  query = {version = "2024-01-01"},
  body = {metadata = {source = "maibot"}},
  enable_thinking = "false"
}
```

This is converted to request extras similar to:

```python
extra_headers = {"X-Trace-Id": "test-001"}
extra_query = {"version": "2024-01-01"}
extra_body = {
    "metadata": {"source": "maibot"},
    "enable_thinking": "false",
}
```

So this common configuration:

```toml
extra_params = {enable_thinking = "false"}
```

sends `enable_thinking` as a request body field. It does not send this structure:

```json
{
  "extra_params": {
    "enable_thinking": "false"
  }
}
```

#### `temperature` and `max_tokens`

`temperature` and `max_tokens` can be written in `extra_params` as model-level defaults, but the dedicated model fields are recommended:

```toml
temperature = 0.7
max_tokens = 4096
```

This keeps the configuration clearer and avoids confusion with provider-specific request body fields that may use the same names.

The priority order is:

1. Values explicitly passed by the current request
2. Dedicated fields in the current model config, such as `temperature` and `max_tokens`
3. Same-name fields in the current model's `extra_params`
4. Defaults from the current task config

#### Gemini Client

When `api_provider.client_type = "gemini"`, `extra_params` does not follow the OpenAI-compatible `headers/query/body` splitting rules. The Gemini client filters and maps fields according to what it supports.

Common uses include:

- Content generation: mapped to supported `GenerateContentConfig` fields
- Embeddings: mapped to supported `EmbedContentConfig` fields, such as `task_type` and `output_dimensionality`
- Thinking parameters, such as `thinking_budget` and `include_thoughts`
- Search capability, such as `enable_google_search`
- Audio requests, such as `audio_mime_type`

Fill these fields according to the official parameter documentation of the provider and model. MaiBot only forwards these model-level extra parameters to the corresponding client.

## Task Configuration [model_task_config]

You need to assign models to different tasks based on model characteristics to achieve the best performance and efficiency.

### Task Type Description

| 🏷️ Task | 💡 What It Does | 🎯 Recommended Model | Example Model |
|----------|----------|------------|------------|
| `utils` | Tool tasks: emoji, learning analysis | Cheap and practical model | dsv4/qwen3.5-35A3B/gemini3.1-flash/gptmini |
| `planner` | Planner: decides action logic, collects information, decides when to reply, etc. | Practical model (needs tool calling support | dsv4/qwen3.5-35A3B/gemini3.1|
| `replyer` | Replyer: generates the actual reply | High-quality model | dsv4(thinking)/gemini3.1 |
| `vlm` | Image understanding: talks about pictures | Vision model | qwen3.5-35A3B/gemini3.1-flash |
| `voice` | Speech recognition: voice to text | Speech model | whisper-1/qwen-audio |
| `embedding` | Generate vectors: used for memory search | Embedding model | qwen3-embbeding |

### Task Configuration Example

```toml
[model_task_config.utils]             # Tool tasks, use cheap and practical ones
model_list = ["deepseek-chat"]
max_tokens = 1024
temperature = 0.3
slow_threshold = 15.0
selection_strategy = "balance"

[model_task_config.replyer]           # Replyer, use a better model
model_list = ["deepseek-chat"]
max_tokens = 1024
temperature = 0.7                     # Higher temperature for more creative replies
slow_threshold = 15.0
selection_strategy = "balance"
```

> 💡 `planner`, `vlm`, and `voice` share the exact same config structure. Just swap the model names in `model_list`. For example, use a vision model like `"qwen3.5-flash"` for `vlm`, or a speech model like `"whisper-1"` for `voice`.

### Parameter Description

| 🏷️ Parameter | 💡 What It Is | 📊 Recommended Value |
|----------|----------|----------|
| `max_tokens` | Maximum output length | `1024` |
| `temperature` | Creativity (0-2) | `0.3` conservative, `0.7` creative |
| `model_list` | Which models to use | Multiple models can be written, automatically switched |
| `slow_threshold` | Slow request threshold (seconds) | `15.0`, outputs warning log when exceeded |
| `selection_strategy` | Model selection strategy | `balance` (default), `random`, `sequential` |

## 🎯 Recommended Configuration (For Beginners)

Below is a **single-model configuration** where all tasks use the same model. Good for getting started quickly:

```toml
[[api_providers]]
name = "deepseek"
base_url = "https://api.deepseek.com/v1"
api_key = "sk-your-key"
auth_type = "bearer"

[[models]]
name = "deepseek-chat"
model_identifier = "deepseek-chat"
api_provider = "deepseek"
visual = false

[model_task_config.utils]
model_list = ["deepseek-chat"]
max_tokens = 1024
temperature = 0.3
slow_threshold = 15.0
selection_strategy = "balance"

[model_task_config.planner]
model_list = ["deepseek-chat"]
max_tokens = 1024
temperature = 0.3
slow_threshold = 15.0
selection_strategy = "balance"

[model_task_config.replyer]
model_list = ["deepseek-chat"]
max_tokens = 1024
temperature = 0.7                    # Higher temperature for more creative replies
slow_threshold = 15.0
selection_strategy = "balance"

[model_task_config.voice]
model_list = ["deepseek-chat"]
max_tokens = 1024
temperature = 0.3
slow_threshold = 15.0
selection_strategy = "balance"
```

> 💡 **Going further**: Once you have multiple models, you can assign different tasks to different ones. For example, use a cheap model for `utils` and `planner`, and a high-quality model for `replyer`. Just change the `model_list` for each task. The config structure is the same.
