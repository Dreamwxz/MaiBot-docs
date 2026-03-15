# Message Sending API

The Message Sending API module is specifically responsible for sending various types of messages, supporting text, emojis, images, and other message types.

## Import Method

```python
from src.plugin_system.apis import send_api
# or
from src.plugin_system import send_api
```

## Main Functions

### 1. Send Text Message
```python
async def text_to_stream(
    text: str,
    stream_id: str,
    typing: bool = False,
    set_reply: bool = False,
    reply_message: Optional["DatabaseMessages"] = None,
    storage_message: bool = True,
    selected_expressions: Optional[List[int]] = None,
) -> bool:
```
Send text message to the specified stream

**Args:**
- `text` (str): Text content to send
- `stream_id` (str): Chat stream ID
- `typing` (bool): Whether to show typing indicator
- `set_reply` (bool): Whether to set as a reply
- `reply_message` (Optional["DatabaseMessages"]): Message object to reply to (required when `set_reply=True`)
- `storage_message` (bool): Whether to store message to database
- `selected_expressions` (Optional[List[int]]): List of selected emoji IDs

**Returns:**
- `bool` - Whether sending was successful

### 2. Send Emoji
```python
async def emoji_to_stream(
    emoji_base64: str,
    stream_id: str,
    storage_message: bool = True,
    set_reply: bool = False,
    reply_message: Optional["DatabaseMessages"] = None,
) -> bool:
```
Send emoji to the specified stream.

**Args:**
- `emoji_base64` (str): Base64 encoded emoji
- `stream_id` (str): Chat stream ID
- `storage_message` (bool): Whether to store message to database
- `set_reply` (bool): Whether to set as a reply
- `reply_message` (Optional["DatabaseMessages"]): Message object to reply to (required when `set_reply=True`)

**Returns:**
- `bool` - Whether sending was successful

### 3. Send Image
```python
async def image_to_stream(
    image_base64: str,
    stream_id: str,
    storage_message: bool = True,
    set_reply: bool = False,
    reply_message: Optional["DatabaseMessages"] = None,
) -> bool:
```
Send image to the specified stream.

**Args:**
- `image_base64` (str): Base64 encoded image
- `stream_id` (str): Chat stream ID
- `storage_message` (bool): Whether to store message to database
- `set_reply` (bool): Whether to set as a reply
- `reply_message` (Optional["DatabaseMessages"]): Message object to reply to (required when `set_reply=True`)

**Returns:**
- `bool` - Whether sending was successful

### 4. Send Command
```python
async def command_to_stream(command: Union[str, dict], stream_id: str, storage_message: bool = True, display_message: str = "") -> bool:
```
Send command to the specified stream.

**Args:**
- `command` (Union[str, dict]): Command content
- `stream_id` (str): Chat stream ID
- `storage_message` (bool): Whether to store message to database
- `display_message` (str): Display message

**Returns:**
- `bool` - Whether sending was successful

### 5. Send Custom Type Message
```python
async def custom_to_stream(
    message_type: str,
    content: str | Dict,
    stream_id: str,
    display_message: str = "",
    typing: bool = False,
    reply_message: Optional["DatabaseMessages"] = None,
    set_reply: bool = False,
    storage_message: bool = True,
    show_log: bool = True,
) -> bool:
```
Send custom type message to the specified stream.

**Args:**
- `message_type` (str): Message type, such as "text", "image", "emoji", "video", "file", etc.
- `content` (str | Dict): Message content (usually base64 encoded or text, can also be a dictionary)
- `stream_id` (str): Chat stream ID
- `display_message` (str): Display message
- `typing` (bool): Whether to show typing indicator
- `reply_message` (Optional["DatabaseMessages"]): Message object to reply to
- `set_reply` (bool): Whether to set as a reply
- `storage_message` (bool): Whether to store message to database
- `show_log` (bool): Whether to show log

**Returns:**
- `bool` - Whether sending was successful

## Usage Examples

### 1. Basic Text Sending with Reply

```python
from src.plugin_system.apis import send_api

async def send_hello(chat_stream, reply_to_message):
    """Send greeting message with reply"""
    
    success = await send_api.text_to_stream(
        text="Hello, world!",
        stream_id=chat_stream.stream_id,
        typing=True,
        set_reply=True,
        reply_message=reply_to_message,
        storage_message=True
    )
    
    return success
```

### 2. Send Emoji

```python
from src.plugin_system.apis import emoji_api
async def send_emoji_reaction(chat_stream, emotion):
    """Send emoji based on emotion"""
    # Get emoji
    emoji_result = await emoji_api.get_by_emotion(emotion)
    if not emoji_result:
        return False
    
    emoji_base64, description, matched_emotion = emoji_result
    
    # Send emoji
    success = await send_api.emoji_to_stream(
        emoji_base64=emoji_base64,
        stream_id=chat_stream.stream_id,
        storage_message=False # Don't store to database
    )
    
    return success
```

## Message Type Explanation

### Supported Message Types
- `"text"`: Plain text message
- `"emoji"`: Emoji message
- `"image"`: Image message
- `"command"`: Command message
- `"video"`: Video message (if supported)
- `"audio"`: Audio message (if supported)

### Reply Message Explanation
To reply to a message, you need to pass a `DatabaseMessages` object to the `reply_message` parameter and set `set_reply=True`.

The `DatabaseMessages` object can be obtained through `message_api`, for example:
```python
from src.plugin_system.apis import message_api, send_api

# Get recent messages
messages = message_api.get_recent_messages(chat_stream.stream_id, hours=1, limit=1)
if messages:
    latest_message = messages[0]
    # Reply to this message
    await send_api.text_to_stream(
        text="This is a reply",
        stream_id=chat_stream.stream_id,
        set_reply=True,
        reply_message=latest_message
    )
```

## Notes

1. **Asynchronous Operations**: All sending functions are asynchronous and must use `await`
2. **Error Handling**: Returns False when sending fails, True when successful
3. **Sending Frequency**: Control sending frequency to avoid platform restrictions
4. **Content Restrictions**: Be aware of platform restrictions on message content and length
5. **Permission Check**: Ensure the bot has permission to send messages
6. **Encoding Format**: Images and emojis need to be base64 encoded
7. **Storage Option**: You can choose whether to store sent messages to the database