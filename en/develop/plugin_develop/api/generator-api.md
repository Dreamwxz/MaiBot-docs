# Reply Generator API

The Reply Generator API module provides intelligent reply generation functionality, allowing plugins to use the system's reply generator to produce natural chat responses.

## Import Method

```python
from src.plugin_system.apis import generator_api
# or
from src.plugin_system import generator_api
```

## Main Functions

### 1. Replyer Acquisition
```python
def get_replyer(
    chat_stream: Optional[ChatStream] = None,
    chat_id: Optional[str] = None,
    model_set_with_weight: Optional[List[Tuple[TaskConfig, float]]] = None,
    request_type: str = "replyer",
) -> Optional[DefaultReplyer]:
```
Get replyer object

Prefer using chat_stream, if not available then use chat_id directly.

Uses ReplyerManager to manage instances, avoiding duplicate creation.

**Args:**
- `chat_stream`: Chat stream object
- `chat_id`: Chat ID (actually `stream_id`)
- `model_set_with_weight`: Model configuration list, each element is a `(TaskConfig, weight)` tuple
- `request_type`: Request type, used for recording LLM usage, can be omitted

**Returns:**
- `DefaultReplyer`: Replyer object, returns None if acquisition fails

#### Example
```python
# Get replyer using chat stream
replyer = generator_api.get_replyer(chat_stream=chat_stream)

# Get replyer using platform and ID
replyer = generator_api.get_replyer(chat_id="123456789")
```

### 2. Reply Generation
```python
async def generate_reply(
    chat_stream: Optional[ChatStream] = None,
    chat_id: Optional[str] = None,
    action_data: Optional[Dict[str, Any]] = None,
    reply_message: Optional["DatabaseMessages"] = None,
    think_level: int = 1,
    extra_info: str = "",
    reply_reason: str = "",
    available_actions: Optional[Dict[str, ActionInfo]] = None,
    chosen_actions: Optional[List["ActionPlannerInfo"]] = None,
    unknown_words: Optional[List[str]] = None,
    enable_tool: bool = False,
    enable_splitter: bool = True,
    enable_chinese_typo: bool = True,
    request_type: str = "generator_api",
    from_plugin: bool = True,
    reply_time_point: Optional[float] = None,
) -> Tuple[bool, Optional["LLMGenerationDataModel"]]:
```
Generate reply

Prefer using chat_stream, if not available then use chat_id directly.

**Args:**
- `chat_stream`: Chat stream object (preferred)
- `chat_id`: Chat ID (fallback)
- `action_data`: Action data (backward compatibility, contains `reply_to` and `extra_info`)
- `reply_message`: Message object to reply to
- `think_level`: Thinking level, 0 for lightweight reply, 1 for medium reply
- `extra_info`: Extra information for supplementing context
- `reply_reason`: Reply reason
- `available_actions`: Available actions dictionary, format is `{"action_name": ActionInfo}`
- `chosen_actions`: Selected actions list
- `unknown_words`: Unknown words list provided by Planner in reply action, used for slang retrieval
- `enable_tool`: Whether to enable tool calling
- `enable_splitter`: Whether to enable message splitter
- `enable_chinese_typo`: Whether to enable typo generator
- `request_type`: Request type (optional, records LLM usage)
- `from_plugin`: Whether from plugin
- `reply_time_point`: Reply time point

**Returns:**
- `Tuple[bool, Optional["LLMGenerationDataModel"]]`: (success status, LLM generation data model)
  - Returns `LLMGenerationDataModel` object when successful, containing `reply_set`, `content`, `prompt`, `model` and other attributes
  - Returns `None` when failed

#### Example
```python
success, llm_response = await generator_api.generate_reply(
    chat_stream=chat_stream,
    reply_message=message,
    extra_info="This is additional context information",
    reply_reason="User asked a question",
    available_actions=action_info,
    enable_tool=True,
    think_level=1
)
if success and llm_response:
    # Access reply set
    if llm_response.reply_set:
        for reply_content in llm_response.reply_set.reply_data:
            print(f"Reply type: {reply_content.content_type}, Content: {reply_content.content}")
    # Access original content
    print(f"Original reply: {llm_response.content}")
    # Access prompt
    print(f"Used prompt: {llm_response.prompt}")
    # Access model information
    print(f"Used model: {llm_response.model}")
```

### 3. Reply Rewriting
```python
async def rewrite_reply(
    chat_stream: Optional[ChatStream] = None,
    reply_data: Optional[Dict[str, Any]] = None,
    chat_id: Optional[str] = None,
    enable_splitter: bool = True,
    enable_chinese_typo: bool = True,
    raw_reply: str = "",
    reason: str = "",
    reply_to: str = "",
    request_type: str = "generator_api",
) -> Tuple[bool, Optional["LLMGenerationDataModel"]]:
```
Rewrite reply, replacing old reply content with new content.

Prefer using chat_stream, if not available then use chat_id directly.

**Args:**
- `chat_stream`: Chat stream object (preferred)
- `reply_data`: Reply data dictionary (backward compatibility fallback, used when other parameters are missing)
- `chat_id`: Chat ID (fallback)
- `enable_splitter`: Whether to enable message splitter
- `enable_chinese_typo`: Whether to enable typo generator
- `raw_reply`: Original reply content
- `reason`: Rewrite reason
- `reply_to`: Reply target
- `request_type`: Request type (optional, records LLM usage)

**Returns:**
- `Tuple[bool, Optional["LLMGenerationDataModel"]]`: (success status, LLM generation data model)
  - Returns `LLMGenerationDataModel` object when successful, containing `reply_set`, `content`, `prompt` and other attributes
  - Returns `None` when failed

#### Example
```python
success, llm_response = await generator_api.rewrite_reply(
    chat_stream=chat_stream,
    raw_reply="Original reply content",
    reason="Rewrite reason",
    reply_to="MaiMai: Hello"
)
if success and llm_response:
    # Access reply set
    if llm_response.reply_set:
        for reply_content in llm_response.reply_set.reply_data:
            print(f"Reply type: {reply_content.content_type}, Content: {reply_content.content}")
    # Access original content
    print(f"Rewritten reply: {llm_response.content}")
    # Access prompt
    print(f"Used prompt: {llm_response.prompt}")
```

## LLMGenerationDataModel Object Explanation

The `LLMGenerationDataModel` object returned by `generate_reply` and `rewrite_reply` functions contains the following main attributes:

- `reply_set`: `ReplySetModel` object, containing processed reply content list
- `content`: Original LLM generated text content
- `processed_output`: Processed output list (split text)
- `prompt`: Used prompt
- `model`: Used model name
- `timing`: Generation timing information
- `reasoning`: Reasoning process (if available)

### ReplySetModel Structure
`reply_set` is a `ReplySetModel` object containing the `reply_data` attribute, which is a list of `ReplyContent` objects.

Each `ReplyContent` object contains:
- `content_type`: Content type (`ReplyContentType.TEXT`, `ReplyContentType.EMOJI`, `ReplyContentType.IMAGE`, etc.)
- `content`: Content data (text string, base64 encoded image, etc.)

### Usage Example
```python
success, llm_response = await generator_api.generate_reply(...)
if success and llm_response and llm_response.reply_set:
    for reply_content in llm_response.reply_set.reply_data:
        if reply_content.content_type == ReplyContentType.TEXT:
            print(f"Text: {reply_content.content}")
        elif reply_content.content_type == ReplyContentType.EMOJI:
            print(f"Emoji: {reply_content.content[:50]}...")  # base64 data is long
```

### 4. Custom Prompt Reply
```python
async def generate_response_custom(
    chat_stream: Optional[ChatStream] = None,
    chat_id: Optional[str] = None,
    request_type: str = "generator_api",
    prompt: str = "",
) -> Optional[str]:
```
Generate custom prompt reply

Prefer using chat_stream, if not available then use chat_id directly.

**Args:**
- `chat_stream`: Chat stream object
- `chat_id`: Chat ID (fallback)
- `request_type`: Request type (optional, records LLM usage)
- `prompt`: Custom prompt

**Returns:**
- `Optional[str]`: Generated custom reply content, returns None if generation fails

## Notes

1. **Asynchronous Operations**: Some functions are asynchronous and must use `await`
2. **Chat Stream Dependency**: Requires valid chat stream object to work properly
3. **Performance Considerations**: Reply generation may take some time, especially when using LLM
4. **Reply Format**: Returned reply set is a list of tuples containing type and content
5. **Context Awareness**: Generator considers chat context and historical messages, unless you use custom prompts.