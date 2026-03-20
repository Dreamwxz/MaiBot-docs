# Database API

The Database API module provides general database operation functionality, supporting querying, creating, updating, and deleting records, using Peewee ORM models.

## Import Method

```python
from src.plugin_system.apis import database_api
# or
from src.plugin_system import database_api
```

## Main Functions

### 1. General Database Operations

```python
async def db_query(
    model_class: Type[Model],
    data: Optional[Dict[str, Any]] = None,
    query_type: Optional[str] = "get",
    filters: Optional[Dict[str, Any]] = None,
    limit: Optional[int] = None,
    order_by: Optional[List[str]] = None,
    single_result: Optional[bool] = False,
) -> Union[List[Dict[str, Any]], Dict[str, Any], None]:
```
General interface for executing database query operations.

**Args:**
- `model_class`: Peewee model class.
    - Peewee model classes can be found in the `src.common.database.database_model` module, such as `ActionRecords`, `Messages`, etc.
- `data`: Data for creating or updating
- `query_type`: Query type
    - Optional values: `get`, `create`, `update`, `delete`, `count`.
- `filters`: Filter condition dictionary, keys are field names, values are values to match.
- `limit`: Limit result count.
- `order_by`: Sorting field list, using field names, prefix '-' indicates descending order.
    - Sorting field, prefix `-` indicates descending order, for example `-time` means descending order by time field (i.e., `time` field)
- `single_result`: Whether to return only single result.

**Returns:**
- Returns different results based on query type:
    - `get`: Returns query result list or single result. (if `single_result=True`)
    - `create`: Returns created record.
    - `update`: Returns affected row count.
    - `delete`: Returns affected row count.
    - `count`: Returns record count.

#### Examples

1. Query recent 10 messages
```python
messages = await database_api.db_query(
    Messages,
    query_type="get",
    filters={"chat_id": chat_stream.stream_id},
    limit=10,
    order_by=["-time"]
)
```
2. Create a record
```python
new_record = await database_api.db_query(
    ActionRecords,
    data={"action_id": "123", "time": time.time(), "action_name": "TestAction"},
    query_type="create",
)
```
3. Update record
```python
updated_count = await database_api.db_query(
    ActionRecords,
    data={"action_done": True},
    query_type="update",
    filters={"action_id": "123"},
)
```
4. Delete record
```python
deleted_count = await database_api.db_query(
    ActionRecords,
    query_type="delete",
    filters={"action_id": "123"}
)
```
5. Count
```python
count = await database_api.db_query(
    Messages,
    query_type="count",
    filters={"chat_id": chat_stream.stream_id}
)
```

### 2. Database Save
```python
async def db_save(
    model_class: Type[Model], data: Dict[str, Any], key_field: Optional[str] = None, key_value: Optional[Any] = None
) -> Optional[Dict[str, Any]]:
```
Save data to database (create or update)

If key_field and key_value are provided, will first try to find matching record for update;

If no matching record found, or key_field and key_value not provided, creates new record.

**Args:**
- `model_class`: Peewee model class.
- `data`: Data dictionary to save.
- `key_field`: Field name for finding existing record, e.g., "action_id".
- `key_value`: Field value for finding existing record.

**Returns:**
- `Optional[Dict[str, Any]]`: Saved record data, returns None on failure.

#### Example
Create or update a record
```python
record = await database_api.db_save(
    ActionRecords,
    {
        "action_id": "123",
        "time": time.time(),
        "action_name": "TestAction",
        "action_done": True
    },
    key_field="action_id",
    key_value="123"
)
```

### 3. Database Get
```python
async def db_get(
    model_class: Type[Model],
    filters: Optional[Dict[str, Any]] = None,
    limit: Optional[int] = None,
    order_by: Optional[str] = None,
    single_result: Optional[bool] = False,
) -> Union[List[Dict[str, Any]], Dict[str, Any], None]:
```

Get records from database

This is a simplified version of the db_query method, focused on data retrieval operations.

**Args:**
- `model_class`: Peewee model class.
- `filters`: Filter condition dictionary, keys are field names, values are values to match.
- `limit`: Limit result count.
- `order_by`: Sorting field, using field name, prefix '-' indicates descending order.
- `single_result`: Whether to return only single result, if True, returns single record dictionary or None; otherwise returns record dictionary list or empty list

**Returns:**
- `Union[List[Dict], Dict, None]`: Query result list or single result (if `single_result=True`), returns None on failure.

#### Examples
1. Get single record
```python
record = await database_api.db_get(
    ActionRecords,
    filters={"action_id": "123"},
    limit=1
)
```
2. Get recent 10 records
```python
records = await database_api.db_get(
    Messages,
    filters={"chat_id": chat_stream.stream_id},
    limit=10,
    order_by="-time",
)
```

### 4. Action Information Storage
```python
async def store_action_info(
    chat_stream=None,
    action_build_into_prompt: bool = False,
    action_prompt_display: str = "",
    action_done: bool = True,
    thinking_id: str = "",
    action_data: Optional[dict] = None,
    action_name: str = "",
    action_reasoning: str = "",
) -> Optional[Dict[str, Any]]:
```
Store action information to database, a wrapper function for `db_save()` specifically for Actions.

Saves Action execution related information to ActionRecords table, for subsequent memory and context construction.

**Args:**
- `chat_stream`: Chat stream object, contains chat ID and other information.
- `action_build_into_prompt`: Whether to build action information into prompt.
- `action_prompt_display`: Display text for action prompt.
- `action_done`: Whether action is completed.
- `thinking_id`: Thinking process ID.
- `action_data`: Action data dictionary.
- `action_name`: Action name.
- `action_reasoning`: Action execution reason.

**Returns:**
- `Optional[Dict[str, Any]]`: Stored record data, returns None on failure.

#### Example
```python
record = await database_api.store_action_info(
    chat_stream=chat_stream,
    action_build_into_prompt=True,
    action_prompt_display="Executed reply action",
    action_done=True,
    thinking_id="thinking_123",
    action_data={"content": "Hello"},
    action_name="reply_action",
    action_reasoning="User asked a question, needs reply"
)
```