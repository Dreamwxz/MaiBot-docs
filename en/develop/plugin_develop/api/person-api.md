# Personal Information API

The Personal Information API module provides user information querying and management functionality, allowing plugins to obtain and use user-related information.

## Import Method

```python
from src.plugin_system.apis import person_api
# or
from src.plugin_system import person_api
```

## Main Functions

### 1. Person ID Acquisition
```python
def get_person_id(platform: str, user_id: int) -> str:
```
Get person_id based on platform and user ID

**Args:**
- `platform`: Platform name, such as "qq", "telegram", etc.
- `user_id`: User ID

**Returns:**
- `str`: Unique person_id (MD5 hash value)

#### Example
```python
person_id = person_api.get_person_id("qq", 123456)
```

### 2. User Information Query
```python
async def get_person_value(person_id: str, field_name: str, default: Any = None) -> Any:
```
Query single user information field value

**Args:**
- `person_id`: User's unique identifier ID
- `field_name`: Field name to get
- `default`: Default value when field doesn't exist

**Returns:**
- `Any`: Field value or default value

#### Example
```python
nickname = await person_api.get_person_value(person_id, "nickname", "Unknown User")
impression = await person_api.get_person_value(person_id, "impression")
```

### 3. Batch User Information Query
```python
async def get_person_values(person_id: str, field_names: list, default_dict: Optional[dict] = None) -> dict:
```
Batch get user information field values

**Args:**
- `person_id`: User's unique identifier ID
- `field_names`: List of field names to get
- `default_dict`: Default value dictionary, keys are field names, values are default values

**Returns:**
- `dict`: Field name to value mapping dictionary

#### Example
```python
values = await person_api.get_person_values(
    person_id,
    ["nickname", "impression", "know_times"],
    {"nickname": "Unknown User", "know_times": 0}
)
```

### 4. Determine if User is Known
```python
async def is_person_known(platform: str, user_id: int) -> bool:
```
Determine if a user is known

**Args:**
- `platform`: Platform name
- `user_id`: User ID

**Returns:**
- `bool`: Whether the user is known

### 5. Get Person ID by Username
```python
def get_person_id_by_name(person_name: str) -> str:
```
Get person_id based on username

**Args:**
- `person_name`: Username

**Returns:**
- `str`: person_id, returns empty string if not found

## Common Field Explanations

### Basic Information Fields
- `nickname`: User nickname
- `platform`: Platform information
- `user_id`: User ID

### Relationship Information Fields
- `impression`: Impression of the user
- `points`: User characteristic points

Other fields can refer to the attributes of the `PersonInfo` class (located in `src.common.database.database_model`)

## Notes

1. **Asynchronous Operations**: Some query functions are asynchronous and require `await`
2. **Performance Considerations**: Batch queries are better than single queries
3. **Privacy Protection**: Ensure user information usage complies with privacy policies
4. **Data Consistency**: person_id is the user's unique identifier, should be properly saved and used