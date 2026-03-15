# Configuration API

The Configuration API module provides configuration reading functionality, allowing plugins to safely access global configurations and plugin configurations.

## Import Method

```python
from src.plugin_system.apis import config_api
# or
from src.plugin_system import config_api
```

## Main Functions

### 1. Access Global Configuration

```python
def get_global_config(key: str, default: Any = None) -> Any:
```

**Args**:
- `key`: Namespace-style configuration key name, using nested access, such as "section.subsection.key", case-sensitive
- `default`: Default value returned when configuration doesn't exist

**Returns**:
- `Any`: Configuration value or default value

#### Example:
Get bot nickname
```python
bot_name = config_api.get_global_config("bot.nickname", "MaiBot")
```

### 2. Get Plugin Configuration

```python
def get_plugin_config(plugin_config: dict, key: str, default: Any = None) -> Any:
```
**Args**:
- `plugin_config`: Plugin configuration dictionary
- `key`: Configuration key name, supports nested access such as "section.subsection.key", case-sensitive
- `default`: Default value returned when configuration doesn't exist

**Returns**:
- `Any`: Configuration value or default value

## Notes

1. **Read-Only Access**: Configuration API only provides reading functionality, plugins cannot modify global configurations
2. **Error Handling**: All functions have error handling, logs errors and returns default value on failure
3. **Security**: Plugin access to configurations through this API is safe and isolated
4. **Performance**: Frequently accessed configurations recommended to be obtained and cached during plugin initialization