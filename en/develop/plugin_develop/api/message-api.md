# Message API

The Message API provides powerful message querying, counting, and formatting capabilities, allowing you to easily handle chat message data.

## Import Method

```python
from src.plugin_system.apis import message_api
# or
from src.plugin_system import message_api
```

## Function Overview

The Message API mainly provides three categories of functions:
- **Message Querying** - Query messages by time, chat, user, and other conditions
- **Message Counting** - Count new message quantities
- **Message Formatting** - Convert messages to readable format

## Main Functions

### 1. Query Messages by Time
```python
def get_messages_by_time(
    start_time: float, end_time: float, limit: int = 0, limit_mode: str = "latest", filter_mai: bool = False
) -> List[DatabaseMessages]:
```
Get messages within the specified time range.

**Args:**
- `start_time` (float): Start timestamp
- `end_time` (float): End timestamp
- `limit` (int): Limit number of returned messages, 0 for no limit
- `limit_mode` (str): Limit mode, `"earliest"` gets earliest records, `"latest"` gets latest records
- `filter_mai` (bool): Whether to filter out bot messages, default False

**Returns:**
- `List[DatabaseMessages]` - Message list (returns a list of `DatabaseMessages` objects)

**Note:** The `DatabaseMessages` object contains the following main attributes:
- `message_id`: Message ID
- `time`: Timestamp
- `processed_plain_text`: Processed plain text content
- `user_info`: User information object
- `chat_info`: Chat information object
- `additional_config`: Additional configuration dictionary

For more information, refer to the `src.common.data_models.database_data_model.DatabaseMessages` class.

### 2. Get Messages in Specified Chat Within Time Range
```python
def get_messages_by_time_in_chat(
    chat_id: str,
    start_time: float,
    end_time: float,
    limit: int = 0,
    limit_mode: str = "latest",
    filter_mai: bool = False,
    filter_command: bool = False,
    filter_intercept_message_level: Optional[int] = None,
) -> List[DatabaseMessages]:
```
Get messages in the specified chat within the specified time range.

**Args:**
- `chat_id` (str): Chat ID
- `start_time` (float): Start timestamp
- `end_time` (float): End timestamp
- `limit` (int): Limit number of returned messages, 0 for no limit
- `limit_mode` (str): Limit mode, `"earliest"` gets earliest records, `"latest"` gets latest records
- `filter_mai` (bool): Whether to filter out bot messages, default False
- `filter_command` (bool): Whether to filter command messages, default False
- `filter_intercept_message_level` (Optional[int]): Filter intercept message level, None for no filtering

**Returns:**
- `List[DatabaseMessages]` - Message list (returns a list of `DatabaseMessages` objects, not dictionaries)

### 3. Get Messages in Specified Chat Within Time Range (Inclusive)
```python
def get_messages_by_time_in_chat_inclusive(
    chat_id: str,
    start_time: float,
    end_time: float,
    limit: int = 0,
    limit_mode: str = "latest",
    filter_mai: bool = False,
    filter_command: bool = False,
    filter_intercept_message_level: Optional[int] = None,
) -> List[DatabaseMessages]:
```
Get messages in the specified chat within the specified time range (inclusive).

**Args:**
- `chat_id` (str): Chat ID
- `start_time` (float): Start timestamp (inclusive)
- `end_time` (float): End timestamp (inclusive)
- `limit` (int): Limit number of returned messages, 0 for no limit
- `limit_mode` (str): Limit mode, `"earliest"` gets earliest records, `"latest"` gets latest records
- `filter_mai` (bool): Whether to filter out bot messages, default False
- `filter_command` (bool): Whether to filter command messages, default False
- `filter_intercept_message_level` (Optional[int]): Filter intercept message level, None for no filtering

**Returns:**
- `List[DatabaseMessages]` - Message list (returns a list of `DatabaseMessages` objects)

### 4. Get Messages for Specified Users in Specified Chat Within Time Range
```python
def get_messages_by_time_in_chat_for_users(
    chat_id: str,
    start_time: float,
    end_time: float,
    person_ids: List[str],
    limit: int = 0,
    limit_mode: str = "latest",
) -> List[Dict[str, Any]]:
```
Get messages for specified users in the specified chat within the specified time range.

**Args:**
- `chat_id` (str): Chat ID
- `start_time` (float): Start timestamp
- `end_time` (float): End timestamp
- `person_ids` (List[str]): User ID list
- `limit` (int): Limit number of returned messages, 0 for no limit
- `limit_mode` (str): Limit mode, `"earliest"` gets earliest records, `"latest"` gets latest records

**Returns:**
- `List[DatabaseMessages]` - Message list (returns a list of `DatabaseMessages` objects)

### 5. Randomly Select a Chat and Return Messages Within Time Range
```python
def get_random_chat_messages(
    start_time: float,
    end_time: float,
    limit: int = 0,
    limit_mode: str = "latest",
    filter_mai: bool = False,
) -> List[Dict[str, Any]]:
```
Randomly select a chat and return messages within the specified time range.

**Args:**
- `start_time` (float): Start timestamp
- `end_time` (float): End timestamp
- `limit` (int): Limit number of returned messages, 0 for no limit
- `limit_mode` (str): Limit mode, `"earliest"` gets earliest records, `"latest"` gets latest records
- `filter_mai` (bool): Whether to filter out bot messages, default False

**Returns:**
- `List[DatabaseMessages]` - Message list (returns a list of `DatabaseMessages` objects)

### 6. Get Messages for Specified Users Across All Chats Within Time Range
```python
def get_messages_by_time_for_users(
    start_time: float,
    end_time: float,
    person_ids: List[str],
    limit: int = 0,
    limit_mode: str = "latest",
) -> List[Dict[str, Any]]:
```
Get messages for specified users across all chats within the specified time range.

**Args:**
- `start_time` (float): Start timestamp
- `end_time` (float): End timestamp
- `person_ids` (List[str]): User ID list
- `limit` (int): Limit number of returned messages, 0 for no limit
- `limit_mode` (str): Limit mode, `"earliest"` gets earliest records, `"latest"` gets latest records

**Returns:**
- `List[DatabaseMessages]` - Message list (returns a list of `DatabaseMessages` objects)

### 7. Get Messages Before Specified Timestamp
```python
def get_messages_before_time(
    timestamp: float,
    limit: int = 0,
    filter_mai: bool = False,
) -> List[Dict[str, Any]]:
```
Get messages before the specified timestamp.

**Args:**
- `timestamp` (float): Timestamp
- `limit` (int): Limit number of returned messages, 0 for no limit
- `filter_mai` (bool): Whether to filter out bot messages, default False

**Returns:**
- `List[Dict[str, Any]]` - Message list

### 8. Get Messages in Specified Chat Before Specified Timestamp
```python
def get_messages_before_time_in_chat(
    chat_id: str,
    timestamp: float,
    limit: int = 0,
    filter_mai: bool = False,
    filter_intercept_message_level: Optional[int] = None,
) -> List[DatabaseMessages]:
```
Get messages in the specified chat before the specified timestamp.

**Args:**
- `chat_id` (str): Chat ID
- `timestamp` (float): Timestamp
- `limit` (int): Limit number of returned messages, 0 for no limit
- `filter_mai` (bool): Whether to filter out bot messages, default False
- `filter_intercept_message_level` (Optional[int]): Filter intercept message level, None for no filtering

**Returns:**
- `List[DatabaseMessages]` - Message list (returns a list of `DatabaseMessages` objects)

### 9. Get Messages for Specified Users Before Specified Timestamp
```python
def get_messages_before_time_for_users(
    timestamp: float,
    person_ids: List[str],
    limit: int = 0,
) -> List[Dict[str, Any]]:
```
Get messages for specified users before the specified timestamp.

**Args:**
- `timestamp` (float): Timestamp
- `person_ids` (List[str]): User ID list
- `limit` (int): Limit number of returned messages, 0 for no limit

**Returns:**
- `List[DatabaseMessages]` - Message list (returns a list of `DatabaseMessages` objects)

### 10. Get Recent Messages in Specified Chat
```python
def get_recent_messages(
    chat_id: str,
    hours: float = 24.0,
    limit: int = 100,
    limit_mode: str = "latest",
    filter_mai: bool = False,
) -> List[Dict[str, Any]]:
```
Get recent messages in the specified chat.

**Args:**
- `chat_id` (str): Chat ID
- `hours` (float): How many recent hours, default 24 hours
- `limit` (int): Limit number of returned messages, default 100
- `limit_mode` (str): Limit mode, `"earliest"` gets earliest records, `"latest"` gets latest records
- `filter_mai` (bool): Whether to filter out bot messages, default False

**Returns:**
- `List[DatabaseMessages]` - Message list (returns a list of `DatabaseMessages` objects)

### 11. Count New Messages in Specified Chat from Start to End Time
```python
def count_new_messages(
    chat_id: str,
    start_time: float = 0.0,
    end_time: Optional[float] = None,
) -> int:
```
Count new messages in the specified chat from start time to end time.

**Args:**
- `chat_id` (str): Chat ID
- `start_time` (float): Start timestamp
- `end_time` (Optional[float]): End timestamp, if None uses current time

**Returns:**
- `int` - Number of new messages

### 12. Count New Messages for Specified Users in Specified Chat from Start to End Time
```python
def count_new_messages_for_users(
    chat_id: str,
    start_time: float,
    end_time: float,
    person_ids: List[str],
) -> int:
```
Count new messages for specified users in the specified chat from start time to end time.

**Args:**
- `chat_id` (str): Chat ID
- `start_time` (float): Start timestamp
- `end_time` (float): End timestamp
- `person_ids` (List[str]): User ID list

**Returns:**
- `int` - Number of new messages

### 13. Build Readable String from Message List
```python
def build_readable_messages_to_str(
    messages: List[DatabaseMessages],
    replace_bot_name: bool = True,
    timestamp_mode: str = "relative",
    read_mark: float = 0.0,
    truncate: bool = False,
    show_actions: bool = False,
) -> str:
```
Build readable string from message list.

**Args:**
- `messages` (List[DatabaseMessages]): Message list (list of `DatabaseMessages` objects)
- `replace_bot_name` (bool): Whether to replace bot name with "you"
- `timestamp_mode` (str): Timestamp display mode, `"relative"` or `"absolute"`
- `read_mark` (float): Read mark timestamp, used to separate read and unread messages
- `truncate` (bool): Whether to truncate long messages
- `show_actions` (bool): Whether to show action records

**Returns:**
- `str` - Formatted readable string

### 14. Build Readable String from Message List with Details
```python
async def build_readable_messages_with_details(
    messages: List[DatabaseMessages],
    replace_bot_name: bool = True,
    timestamp_mode: str = "relative",
    truncate: bool = False,
) -> Tuple[str, List[Tuple[float, str, str]]]:
```
Build readable string from message list and return details.

**Args:**
- `messages` (List[DatabaseMessages]): Message list (list of `DatabaseMessages` objects)
- `replace_bot_name` (bool): Whether to replace bot name with "you"
- `timestamp_mode` (str): Timestamp display mode, `"relative"` or `"absolute"`
- `truncate` (bool): Whether to truncate long messages

**Returns:**
- `Tuple[str, List[Tuple[float, str, str]]]` - Formatted readable string and detail tuple list (timestamp, nickname, content)

### 15. Extract Unique User IDs from Message List
```python
async def get_person_ids_from_messages(
    messages: List[Dict[str, Any]],
) -> List[str]:
```
Extract unique user IDs from message list.

**Args:**
- `messages` (List[Dict[str, Any]]): Message list

**Returns:**
- `List[str]` - User ID list

### 16. Remove Bot Messages from Message List
```python
def filter_mai_messages(
    messages: List[DatabaseMessages],
) -> List[DatabaseMessages]:
```
Remove bot messages from message list.

**Args:**
- `messages` (List[DatabaseMessages]): Message list, each element is a `DatabaseMessages` object

**Returns:**
- `List[DatabaseMessages]` - Filtered message list

## Notes

1. **Timestamp Format**: All time parameters use Unix timestamps (float type)
2. **Asynchronous Functions**: Some functions are asynchronous and require `await`
3. **Performance Considerations**: When querying large numbers of messages, set reasonable `limit` parameters
4. **Message Format**: Returned messages are in dictionary format, containing timestamp, sender, content, etc.
5. **User IDs**: The `person_ids` parameter accepts string lists for filtering messages from specific users