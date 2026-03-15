# 🔧 Model Configuration Guide

## Introduction

This configuration file mainly tells you what functions each model used by MaiMai serves, and which large models are more suitable.

## API Provider Configuration

```toml
[[api_providers]] # API service providers (can configure multiple)
name = "DeepSeek"                       # API provider name (can name arbitrarily, need to use this name in models' api-provider)
base_url = "https://api.deepseek.com/v1" # API provider's BaseURL
api_key = "your-api-key-here"           # API key (please replace with actual API key)
client_type = "openai"                  # Request client (optional, default value "openai", when using Google family models like gimini please configure as "gemini")
max_retry = 2                           # Maximum retry count (single model API call failure, maximum retry count)
timeout = 120                           # API request timeout (unit: seconds)
retry_interval = 10                     # Retry interval time (unit: seconds)
```

### Supported Client Types

- `openai` - OpenAI compatible API (default value)
- `gemini` - Google Gemini API

### Configuration Examples

```toml
[[api_providers]] # SiliconFlow API provider configuration
name = "SiliconFlow"
base_url = "https://api.siliconflow.cn/v1"
api_key = "your-siliconflow-api-key"
client_type = "openai"
max_retry = 3
timeout = 120
retry_interval = 5

[[api_providers]] # Special: Google's Gimini uses special API, not compatible with OpenAI format, needs to configure client as "gemini"
name = "Google"
base_url = "https://generativelanguage.googleapis.com/v1beta"
api_key = "your-google-api-key-1"
client_type = "gemini"
max_retry = 2
timeout = 120
retry_interval = 10

[[api_providers]] # Alibaba Bailian API provider configuration
name = "BaiLian"
base_url = "https://dashscope.aliyuncs.com/compatible-mode/v1"
api_key = "your-bailian-key"
client_type = "openai"
max_retry = 2
timeout = 120
retry_interval = 5
```

<hr class="custom_hr"/>

## Model Definition Configuration, How to Define Your AI Models

```toml
[[models]] # Models (can configure multiple)
model_identifier = "deepseek-chat" # Model identifier (model identifier provided by API provider)
name = "deepseek-v3"               # Model name (can name arbitrarily, need to use this name later)
api_provider = "DeepSeek"          # API provider name (corresponding to provider name configured in api_providers)
price_in = 2.0                     # Input price (for API call statistics, unit: yuan/ M token) (optional, if field absent, default value 0)
price_out = 8.0                    # Output price (for API call statistics, unit: yuan/ M token) (optional, if field absent, default value 0)
#force_stream_mode = true          # Force streaming output mode (if model doesn't support non-streaming output, please uncomment this, enable forced streaming output, if field absent, default value false)
```

### Model Configuration Field Explanation

- `model_identifier`: Model identifier provided by API provider
- `name`: Custom model name, referenced in task configuration
- `api_provider`: Corresponding API provider name
- `price_in/price_out`: Price information (optional, for statistics)
- `force_stream_mode`: Force streaming output (optional)

### Additional Parameter Configuration

```toml
[[models]]
model_identifier = "deepseek-ai/DeepSeek-V3.2-Exp"
name = "siliconflow-deepseek-v3.2"
api_provider = "SiliconFlow"
price_in = 2.0
price_out = 8.0
[models.extra_params] # Optional additional parameter configuration
enable_thinking = false # Don't enable thinking
```

### More Model Examples

```toml
[[models]]
model_identifier = "deepseek-ai/DeepSeek-V3.2-Exp"
name = "siliconflow-deepseek-v3.2-think"
api_provider = "SiliconFlow"
price_in = 2.0
price_out = 3.0
[models.extra_params]
enable_thinking = true # Enable reasoning mode

[[models]]
model_identifier = "Qwen/Qwen3-30B-A3B-Instruct-2507"
name = "qwen3-30b"
api_provider = "SiliconFlow"
price_in = 0.7
price_out = 2.8

[[models]]
model_identifier = "Qwen/Qwen3-VL-30B-A3B-Instruct"
name = "qwen3-vl-30"
api_provider = "SiliconFlow"
price_in = 4.13
price_out = 4.13

[[models]]
model_identifier = "FunAudioLLM/SenseVoiceSmall"
name = "sensevoice-small"
api_provider = "SiliconFlow"
price_in = 0
price_out = 0

[[models]]
model_identifier = "BAAI/bge-m3"
name = "bge-m3"
api_provider = "SiliconFlow"
price_in = 0
price_out = 0
```

<hr class="custom_hr"/>

## Task Model Configuration

### **Required: Component Models**

These models are **required** for MaiMai to run, but don't directly generate replies, instead participate in memory, image recognition, relationships, emotions, etc.

```toml
[model_task_config.utils] # Models used in some of MaiMai's components, e.g., emoji pack module, naming module, relationship module, MaiMai's emotional changes, etc., are required models for MaiMai
model_list = ["siliconflow-deepseek-v3.2"] # Used model list, each item corresponds to above model name (name)
temperature = 0.2                        # Model temperature, new V3 recommended 0.1-0.3
max_tokens = 4096                         # Maximum output token count
slow_threshold = 15.0                     # Slow request threshold (seconds), model waiting reply time exceeding this value outputs warning log
selection_strategy = "random"           # Model selection strategy: random (load balancing) or random (random selection)
```

- `utils`: Recommended to use high-performance V3.2 (non-thinking version), most system components depend on it.
- `slow_threshold`: Slow request threshold (seconds), model waiting reply time exceeding this value outputs warning log
- `selection_strategy`: Model selection strategy, options "random" (random selection) or "load_balance" (load balancing)

### **Reply and Decision Models**

These models are responsible for generating replies and making decisions.

```toml
[model_task_config.tool_use] # Function models, need to use models supporting tool calls, please use faster small models (high call volume)
model_list = ["qwen3-30b","qwen3-next-80b"]
temperature = 0.7
max_tokens = 1024
slow_threshold = 10.0
selection_strategy = "random"           # Model selection strategy: random (load balancing) or random (random selection)

[model_task_config.replyer] # Primary reply model, also used for expression learning
model_list = ["siliconflow-deepseek-v3.2","siliconflow-deepseek-v3.2-think","siliconflow-glm-4.6","siliconflow-glm-4.6-think"]
temperature = 0.3                        # Model temperature, new V3 recommended 0.1-0.3
max_tokens = 2048
slow_threshold = 25.0
selection_strategy = "random"           # Model selection strategy: random (load balancing) or random (random selection)

[model_task_config.planner] # Decision: model responsible for deciding when MaiMai should reply
model_list = ["siliconflow-deepseek-v3.2"]
temperature = 0.3
max_tokens = 800
slow_threshold = 12.0
selection_strategy = "random"           # Model selection strategy: random (load balancing) or random (random selection)
```

- `tool_use`: **Tool call model**, needs to use models with function calling capability, please use faster small models (high call volume)
- `replyer`: **Primary reply model**, also used for expression learning, can mix reasoning version and regular version to balance effect and cost
- `planner`: **Decision model**, responsible for deciding when MaiMai should reply
- `slow_threshold`: Slow request threshold (seconds), model waiting reply time exceeding this value outputs warning log
- `selection_strategy`: Model selection strategy, options "random" (random selection) or "load_balance" (load balancing)

### **Image and Voice Models**

```toml
[model_task_config.vlm] # Image recognition model
model_list = ["qwen3-vl-30"]
max_tokens = 256
slow_threshold = 15.0
selection_strategy = "random"           # Model selection strategy: random (load balancing) or random (random selection)

[model_task_config.voice] # Voice recognition model
model_list = ["sensevoice-small"]
slow_threshold = 12.0
selection_strategy = "random"           # Model selection strategy: random (load balancing) or random (random selection)
```

- `vlm`: For **image recognition**, needs a model supporting image understanding
- `voice`: For **voice recognition**, needs a model supporting speech-to-text
- `slow_threshold`: Slow request threshold (seconds), model waiting reply time exceeding this value outputs warning log
- `selection_strategy`: Model selection strategy, options "random" (random selection) or "load_balance" (load balancing)

### **Embedding Models**

```toml
# Embedding models
[model_task_config.embedding]
model_list = ["bge-m3"]
slow_threshold = 5.0
selection_strategy = "random"           # Model selection strategy: random (load balancing) or random (random selection)
```

- `embedding`: Used by **knowledge base**, can use other embedding models
- `slow_threshold`: Slow request threshold (seconds), model waiting reply time exceeding this value outputs warning log
- `selection_strategy`: Model selection strategy, options "random" (random selection) or "load_balance" (load balancing)

### **LPMM Knowledge Base Models**

If `lpmm_knowledge` is enabled, need to configure the following models.

```toml
#------------LPMM Knowledge Base Models------------

[model_task_config.lpmm_entity_extract] # Entity extraction model
model_list = ["siliconflow-deepseek-v3.2"]
temperature = 0.2
max_tokens = 800

[model_task_config.lpmm_rdf_build] # RDF construction model
model_list = ["siliconflow-deepseek-v3.2"]
temperature = 0.2
max_tokens = 800

[model_task_config.lpmm_qa] # Q&A model
model_list = ["qwen3-30b"]
temperature = 0.7
max_tokens = 800
```

- `lpmm_entity_extract`: **Extract entities** from knowledge text
- `lpmm_rdf_build`: **Build RDF triples** based on entities
- `lpmm_qa`: **Q&A** based on knowledge base

<hr class="custom_hr"/>

## Notes

1. **API Key Security**:
    - Properly store API keys
    - Don't upload configuration files containing keys to public repositories

2. **Configuration Modification**:
    - Need to restart service after modifying configuration
    - No need to modify model configuration when using default service (SiliconFlow)
    - **Model names may update, need to regularly check console model names**

3. **Configuration Structure**:
    - Models must first be defined in `[[models]]`, then referenced in `[model_task_config.*]`
    - Model names must exactly match `name` field in `[[models]]`

4. **Other Notes**:
    - Project is in testing phase, may have unknown issues
    - Recommended to keep default configuration for first use

5. **Error Troubleshooting**:
    - `401` error: Check if corresponding `api_key` valid
    - `404` error: Confirm corresponding `base_url` path correct
    - Model not found: Check if model names in `model_list` correct