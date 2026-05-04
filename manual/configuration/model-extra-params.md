---
title: 模型额外参数
titleTemplate: :title · 模型高级参数
---

# 模型高级参数

`model_config.toml` 中每个模型都可以设置 `extra_params` 字段，让你在 API 调用时传递特定服务商的额外参数。

> 💡 **提示**：除非是 `headers`、`query`、`body` 这三个特殊 key，其他所有 key 都会被合并到请求体中（即 OpenAI SDK 的 `extra_body` 参数）。

## API 提供商高级配置

### 高级鉴权配置

| 🏷️ 配置项 | 💡 是什么 | 📝 怎么填 |
|-----------|----------|----------|
| `auth_header_name` | Header鉴权名称 | `Authorization`（默认） |
| `auth_header_prefix` | Header鉴权前缀 | `Bearer`（默认） |
| `auth_query_name` | Query鉴权参数名 | `api_key`（默认） |

### 高级参数

| 🏷️ 配置项 | 💡 作用 | 📊 推荐值 |
|-----------|--------|----------|
| `default_headers` | 默认HTTP头 | `{}` |
| `default_query` | 默认查询参数 | `{}` |
| `organization` | OpenAI组织（可选） | `None` |
| `project` | OpenAI项目（可选） | `None` |
| `model_list_endpoint` | 模型列表端点 | `/models` |
| `reasoning_parse_mode` | 推理内容解析模式 | `auto` |
| `tool_argument_parse_mode` | 工具参数解析模式 | `auto` |

### 运行时配置

| 🏷️ 配置项 | 💡 作用 | 📊 推荐值 |
|-----------|--------|----------|
| `timeout` | 超时时间 | `10` 秒 |
| `max_retry` | 失败重试次数 | `2` 次 |
| `retry_interval` | 重试间隔 | `10` 秒 |

## 模型高级参数

### 参数覆盖

| 🏷️ 配置项 | 💡 是什么 | 📝 怎么填 |
|-----------|----------|----------|
| `temperature` | 模型级温度 | 覆盖任务配置的温度，可选 |
| `max_tokens` | 模型级最大token | 覆盖任务配置的max_tokens，可选 |

### 其他高级参数

| 🏷️ 配置项 | 💡 是什么 | 📝 怎么填 |
|-----------|----------|----------|
| `force_stream_mode` | 强制流式输出 | `false`（默认），不支持非流式时设为`true` |
| `extra_params` | 额外参数 | `{}`（默认），传递自定义API参数的键值对字典，详见下方各场景 |

---

## 控制大模型思考模式

很多大模型支持"思考模式"——让模型在回答前先进行深度推理，从而提升复杂问题的回答质量。

### DeepSeek

```toml
[[models]]
name = "deepseek-r1"
model_identifier = "deepseek-reasoner"
api_provider = "deepseek"
visual = false
extra_params = {enable_thinking = true}   # 开启思考模式
```

| 参数 | 类型 | 说明 |
|------|------|------|
| `enable_thinking` | `bool` | `true` 开启思考，`false` 关闭 |

### 阿里百炼 (Qwen)

Qwen 3.6 及以上模型默认开启思考模式，无需额外配置。

> 💡 旧版 Qwen 3.5 系列部分模型可通过 `enable_thinking` 控制，但 Qwen 3.6+ 始终开启推理，质量更好。

## 调整推理深度

OpenAI 的推理模型通过 `reasoning_effort` 参数控制推理深度。

| 等级 | 适用场景 | 特点 |
|------|----------|------|
| `none` | 简单问答、信息检索 | 速度最快，不推理 |
| `minimal` | 最低限度推理 | 几乎不增加延迟 |
| `low` | 工具调用、搜索、多步决策 | 少量推理 |
| `medium` | 规划、复杂推理（默认） | 质量与速度平衡 |
| `high` | 复杂调试、深度规划 | 质量优先 |
| `xhigh` | 深度研究、异步任务 | 质量最高，延迟最大 |

```toml
[[models]]
name = "gpt-5"
model_identifier = "gpt-5.5"
api_provider = "openai"
visual = false
extra_params = {reasoning_effort = "medium"}
```

> 💡 **建议**：日常使用推荐 `medium`，对速度敏感用 `low`，深度分析用 `high`。

## 关于 client_type 和 Gemini

`client_type` 决定 MaiBot 使用哪种客户端与 API 通信：

| 值 | 说明 |
|----|------|
| `openai` | OpenAI 兼容接口（默认），适用于 DeepSeek、阿里百炼、OpenAI 等 |
| `google` | Google Gemini 原生接口，支持思考预算控制 |

### Gemini 思考配置

Gemini 模型通过 `extra_params` 中的 `thinking_config` 控制思考：

```toml
[[models]]
name = "gemini-2.5-flash"
model_identifier = "gemini-2.5-flash"
api_provider = "google-gemini"
visual = true
client_type = "google"
extra_params = {thinking_config = {thinking_budget = 4096}}
```

> ⚠️ Google API 在国内无法直接访问，需要代理。

## 自定义 HTTP 请求

`extra_params` 支持三个特殊 key 来精确控制 API 请求：

| 特殊 Key | 作用 | 示例 |
|----------|------|------|
| `headers` | 添加 HTTP 请求头 | `{headers: {"X-Custom": "value"}}` |
| `query` | 添加 URL 查询参数 | `{query: {"key": "value"}}` |
| `body` | 覆盖请求体字段 | `{body: {"field": "value"}}` |

```toml
[[models]]
name = "custom-model"
model_identifier = "custom-model-v1"
api_provider = "custom"
visual = false
extra_params = {headers = {"X-API-Version" = "2024-06", "X-Priority" = "high"}}
```

## 组合使用

你可以同时使用多个参数：

```toml
[[models]]
name = "gpt-5-advanced"
model_identifier = "gpt-5.5"
api_provider = "openai"
visual = true
extra_params = {
    reasoning_effort = "high",
    headers = {"X-Request-ID" = "custom-id", "X-Priority" = "high"}
}
```

## 常用参数速查

| 参数 | 适用服务商 | 类型 | 说明 |
|------|-----------|------|------|
| `enable_thinking` | DeepSeek | `bool` | 开启思考模式 |
| `reasoning_effort` | OpenAI | `str` | 推理深度等级 |
| `headers` | 全部 | `dict` | 自定义 HTTP 请求头 |
| `query` | 全部 | `dict` | 自定义 URL 查询参数 |
| `body` | 全部 | `dict` | 自定义请求体字段 |
| `thinking_config` | Gemini | `dict` | 思考预算配置 |

> ⚠️ **注意**：参数会原样传递给 LLM API，务必与你使用的服务商文档一致，否则可能导致调用失败。