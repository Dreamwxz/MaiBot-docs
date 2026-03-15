# Chat API

The Chat API module is specifically responsible for querying and managing chat information, helping plugins obtain and manage different chat streams.

## Import Method

```python
from src.plugin_system import chat_api
# or
from src.plugin_system.apis import chat_api
```

## Main Functions

### 1. Get All Chat Streams

```python
def get_all_streams(platform: Optional[str] | SpecialTypes = "qq") -> List[ChatStream]:
```

**Args**:
- `platform`: Platform filter, default is "qq", can use `SpecialTypes.ALL_PLATFORMS` from the `SpecialTypes` enum class to get chat streams from all platforms.

**Returns**:
- `List[ChatStream]`: Chat stream list

### 2. Get Group Chat Streams

```python
def get_group_streams(platform: Optional[str] | SpecialTypes = "qq") -> List[ChatStream]:
```

**Args**:
- `platform`: Platform filter, default is "qq", can use `SpecialTypes.ALL_PLATFORMS` from the `SpecialTypes` enum class to get group chat streams from all platforms.

**Returns**:
- `List[ChatStream]`: Group chat stream list

### 3. Get Private Chat Streams

```python
def get_private_streams(platform: Optional[str] | SpecialTypes = "qq") -> List[ChatStream]:
```

**Args**:
- `platform`: Platform filter, default is "qq", can use `SpecialTypes.ALL_PLATFORMS` from the `SpecialTypes` enum class to get private chat streams from all platforms.

**Returns**:
- `List[ChatStream]`: Private chat stream list

### 4. Get Chat Stream by Group ID

```python
def get_stream_by_group_id(group_id: str, platform: Optional[str] | SpecialTypes = "qq") -> Optional[ChatStream]:
```

**Args**:
- `group_id`: Group chat ID
- `platform`: Platform filter, default is "qq", can use `SpecialTypes.ALL_PLATFORMS` from the `SpecialTypes` enum class to get group chat streams from all platforms.

**Returns**:
- `Optional[ChatStream]`: Chat stream object, returns None if not found

### 5. Get Private Chat Stream by User ID

```python
def get_stream_by_user_id(user_id: str, platform: Optional[str] | SpecialTypes = "qq") -> Optional[ChatStream]:
```

**Args**:
- `user_id`: User ID
- `platform`: Platform filter, default is "qq", can use `SpecialTypes.ALL_PLATFORMS` from the `SpecialTypes` enum class to get private chat streams from all platforms.

**Returns**:
- `Optional[ChatStream]`: Chat stream object, returns None if not found

### 6. Get Chat Stream Type

```python
def get_stream_type(chat_stream: ChatStream) -> str:
```

**Args**:
- `chat_stream`: Chat stream object

**Returns**:
- `str`: Chat stream type, possible values include `private` (private chat stream), `group` (group chat stream), and `unknown` (unknown type).

### 7. Get Chat Stream Information

```python
def get_stream_info(chat_stream: ChatStream) -> Dict[str, Any]:
```

**Args**:
- `chat_stream`: Chat stream object

**Returns**:
- `Dict[str, Any]`: Detailed information about the chat stream, including but not limited to:
    - `stream_id`: Chat stream ID
    - `platform`: Platform name
    - `type`: Chat stream type
    - `group_id`: Group chat ID
    - `group_name`: Group chat name
    - `user_id`: User ID
    - `user_name`: User name

### 8. Get Chat Stream Statistics Summary

```python
def get_streams_summary() -> Dict[str, int]:
```

**Returns**:
- `Dict[str, int]`: Chat stream statistics summary, containing the following keys:
    - `total_streams`: Total number of chat streams
    - `group_streams`: Number of group chat streams
    - `private_streams`: Number of private chat streams
    - `qq_streams`: Number of QQ platform streams

## Notes

1. Most functions will throw exceptions when parameters are invalid, please ensure your program handles these exceptions.
2. The `ChatStream` object contains complete chat information, including user information, group information, etc.