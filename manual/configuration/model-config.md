---
title: 模型配置
---

# 设置AI大脑

`model_config.toml` 是给麦麦配置"AI大脑"的文件，决定了麦麦的不同组件使用什么LLM

我们推荐根据不同任务的特性来分配不同的模型

麦麦的配置必须一个LLM模型（或VLM），一个VLM模型和嵌入模型

## 配置文件结构

```toml
# 配置文件版本
[inner]
version = "1.14.0"

# API 提供商列表（AI服务商）
[[api_providers]]
name = "deepseek"
base_url = "https://api.deepseek.com/v1"
api_key = "sk-xxxxxxxxxxxxxxxxxxxxxxxx"

# 模型列表（具体用哪个AI）
[[models]]
name = "deepseek-chat"
model_identifier = "deepseek-chat"
api_provider = "deepseek"

# 任务分配（不同工作用不同AI）
[model_task_config.replyer]
model_list = ["deepseek-chat"]
max_tokens = 1024
temperature = 0.3
```

## API 提供商 [[api_providers]]

API 提供商代表提供LLM服务的对象

### 基础配置（必填）

| 🏷️ 配置项 | 💡 是什么 | 📝 怎么填 |
|-----------|----------|----------|
| `name` | 服务商名字 | 自己取个名字，比如"deepseek"、"openai" |
| `base_url` | API地址 | 服务商提供的url |
| `api_key` | 密钥 | 注册后获得的密钥 |

### 常见服务商配置示例

::: code-group

```toml [DeepSeek]
[[api_providers]]
name = "deepseek"
base_url = "https://api.deepseek.com/v1"
api_key = "sk-你的密钥"
client_type = "openai"
auth_type = "bearer"
```

```toml [OpenAI]
[[api_providers]]
name = "openai"
base_url = "https://api.openai.com/v1"
api_key = "sk-你的密钥"
client_type = "openai"
auth_type = "bearer"
```

```toml [阿里百炼]
[[api_providers]]
name = "aliyun"
base_url = "https://dashscope.aliyuncs.com/compatible-mode/v1"
api_key = "sk-你的密钥"
client_type = "openai"
auth_type = "bearer"
```

```toml [字节火山]
[[api_providers]]
name = "volcengine"
base_url = "https://ark.cn-beijing.volces.com/api/v3"
api_key = "sk-你的密钥"
client_type = "openai"
auth_type = "bearer"
```

:::

### 高级配置（可选）

| 🏷️ 配置项 | 💡 作用 | 📊 推荐值 |
|-----------|--------|----------|
| `timeout` | 超时时间 | `10` 秒 |
| `max_retry` | 失败重试次数 | `2` 次 |
| `retry_interval` | 重试间隔 | `10` 秒 |

## 模型列表 [[models]]

模型就是具体的LLM，比如GPT-5.4、Claude-4.6-opus、DeepSeek V4等。

### 基础配置（必填）

| 🏷️ 配置项 | 💡 是什么 | 📝 怎么填 |
|-----------|----------|----------|
| `name` | 模型名字 | 自己取个名字，比如"gpt-5"、"deepseek-v4" |
| `model_identifier` | 模型ID | 服务商提供的具体模型名称 |
| `api_provider` | 用哪个服务商 | 填上面api_providers（提供商）里的name |
| `visual` | 是否启用视觉 | 只有多模态模型可以打开此选项 |

### 模型配置示例

```toml
[[models]]
name = "deepseek-chat"                    # 我叫它"deepseek-chat"
model_identifier = "deepseek-chat"        # 服务商也这么叫
api_provider = "deepseek"                 # 用deepseek这个服务商
visual = false                            # 不能看图

[[models]]
name = "qwen3.5-vl"                          # 视觉模型，能看图
model_identifier = "qwen3.5-flash"
api_provider = "aliyun"
visual = true                             # ✅ 能看图
```

### 额外参数 `extra_params`

`extra_params` 是模型级额外参数，用来给某个模型附加服务商特有的 API 参数。它配置在单个 `[[models]]` 下，只对当前模型生效。

```toml
[[models]]
name = "deepseek-v4-pro-nonthink"
model_identifier = "deepseek-v4-pro"
api_provider = "aliyun"
extra_params = {enable_thinking = "false"}
```

这里的 `extra_params` 是 MaiBot 内部配置字段，**不会以 `extra_params` 这个键整体发送给模型服务商**。实际请求前，客户端会把它转换成对应 SDK 支持的附加参数。

#### OpenAI 兼容客户端

当 `api_provider.client_type = "openai"`，或使用默认 OpenAI 兼容客户端时，`extra_params` 会按下面规则拆分：

| 写法 | 实际用途 |
|------|----------|
| `headers` | 作为请求头传入 |
| `query` | 作为 URL 查询参数传入 |
| `body` | 合并到请求体 |
| 其他普通键 | 作为请求体额外字段传入 |

例如：

```toml
extra_params = {
  headers = {"X-Trace-Id" = "test-001"},
  query = {version = "2024-01-01"},
  body = {metadata = {source = "maibot"}},
  enable_thinking = "false"
}
```

会被转换为近似下面的请求附加参数：

```python
extra_headers = {"X-Trace-Id": "test-001"}
extra_query = {"version": "2024-01-01"}
extra_body = {
    "metadata": {"source": "maibot"},
    "enable_thinking": "false",
}
```

所以常见写法：

```toml
extra_params = {enable_thinking = "false"}
```

最终会把 `enable_thinking` 作为请求体字段传给服务商，而不是发送下面这种结构：

```json
{
  "extra_params": {
    "enable_thinking": "false"
  }
}
```

#### `temperature` 和 `max_tokens`

`temperature` 和 `max_tokens` 可以写在 `extra_params` 中作为模型级默认值，但更推荐使用模型配置里的同名独立字段：

```toml
temperature = 0.7
max_tokens = 4096
```

这样配置意图更清楚，也能避免和服务商请求体中的同名字段混淆。

生效优先级为：

1. 调用方本次请求显式传入的值
2. 当前模型配置里的独立字段，例如 `temperature`、`max_tokens`
3. 当前模型 `extra_params` 中的同名字段
4. 当前任务配置中的默认值

#### Gemini 客户端

当 `api_provider.client_type = "gemini"` 时，`extra_params` 不按 OpenAI 兼容客户端的 `headers/query/body` 规则处理，而是由 Gemini 客户端按自身支持的字段进行筛选和映射。

常见用途包括：

- 生成内容请求：映射到 `GenerateContentConfig` 支持的字段
- 嵌入请求：映射到 `EmbedContentConfig` 支持的字段，例如 `task_type`、`output_dimensionality`
- 思考参数：例如 `thinking_budget`、`include_thoughts`
- 搜索能力：例如 `enable_google_search`
- 音频请求：例如 `audio_mime_type`

请根据对应服务商和模型的官方参数说明填写，MaiBot 只负责把这些模型级附加参数转交给相应客户端处理。


## 任务配置 [model_task_config]

你需要根据模型的特点分给各种任务，实现最好的表现和最优的效率

### 任务类型说明

| 🏷️ 任务 | 💡 干什么 | 🎯 推荐模型 | 示例模型 |
|----------|----------|------------|------------|
| `utils` | 工具类：表情包、学习分析 | 便宜实用的模型 | dsv4/qwen3.5-35A3B/gemini3.1-flash/gptmini |
| `planner` | 规划器：决定行动逻辑，搜集信息，何时回复等 | 实用的模型（需要支持tool调用 | dsv4/qwen3.5-35A3B/gemini3.1|
| `replyer` | 回复器：生成实际回复 | 质量好的模型 | dsv4(思考)/claude-4.6-opus/gemini3.1 |
| `vlm` | 看图说话：理解图片 | 视觉模型 | qwen3.5-35A3B/gemini3.1-flash |
| `embedding` | 生成向量：用于记忆搜索 | 嵌入模型 | qwen3-embbeding |

### 任务配置示例

```toml
# 工具类任务：用便宜实用的
[model_task_config.utils]
model_list = ["deepseek-chat"]
max_tokens = 1024
temperature = 0.3

# 规划器：用实用的
[model_task_config.planner]
model_list = ["deepseek-chat"]
max_tokens = 1024
temperature = 0.3

# 回复器：用好一点的模型
[model_task_config.replyer]
model_list = ["deepseek-chat"]  # 或者gpt-4等更好的
max_tokens = 1024
temperature = 0.7

# 看图：用视觉模型
[model_task_config.vlm]
model_list = ["qwen3.5-flash"]
max_tokens = 1024
temperature = 0.3
```

### 参数说明

| 🏷️ 参数 | 💡 是什么 | 📊 推荐值 |
|----------|----------|----------|
| `max_tokens` | 最多输出多少字 | `1024` |
| `temperature` | 创造性（0-2） | `0.3` 保守，`0.7` 有创意 |
| `model_list` | 用哪些模型 | 可以写多个，自动切换 |

## 🎯 推荐配置（新手专用）

### 方案1：单模型配置（最简单）

```toml
# 只用DeepSeek，所有任务都用它
[[api_providers]]
name = "deepseek"
base_url = "https://api.deepseek.com/v1"
api_key = "sk-你的密钥"

[[models]]
name = "deepseek-chat"
model_identifier = "deepseek-chat"
api_provider = "deepseek"

[model_task_config.utils]
model_list = ["deepseek-chat"]

[model_task_config.planner]
model_list = ["deepseek-chat"]

[model_task_config.replyer]
model_list = ["deepseek-chat"]
```

### 方案2：分工配置（性价比）

```toml
# 工具类用便宜的，回复用好的
[[api_providers]]
name = "deepseek"
base_url = "https://api.deepseek.com/v1"
api_key = "sk-你的密钥"

[[models]]
name = "deepseek-chat"
model_identifier = "deepseek-chat"
api_provider = "deepseek"

[model_task_config.utils]
model_list = ["deepseek-chat"]  # 工具类用便宜的

[model_task_config.planner]
model_list = ["deepseek-chat"]  # 规划也用便宜的

[model_task_config.replyer]
model_list = ["deepseek-chat"]  # 回复可以用好一点的（等你有更好的模型再换）
```

