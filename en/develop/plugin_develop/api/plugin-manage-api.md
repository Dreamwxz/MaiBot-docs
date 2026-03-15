# Plugin Management API

The Plugin Management API module provides functionality for loading, unloading, reloading plugins, and directory management.

## Import Method
```python
from src.plugin_system.apis import plugin_manage_api
# or
from src.plugin_system import plugin_manage_api
```

## Function Overview

The Plugin Management API mainly provides the following functions:
- **Plugin Query** - List currently loaded plugins or registered plugins.
- **Plugin Management** - Load, unload, reload plugins.
- **Plugin Directory Management** - Add plugin directories and rescan.

## Main Functions

### 1. List Currently Loaded Plugins
```python
def list_loaded_plugins() -> List[str]:
```
List all currently loaded plugins.

**Returns:**
- `List[str]` - List of currently loaded plugin names.

### 2. List All Registered Plugins
```python
def list_registered_plugins() -> List[str]:
```
List all registered plugins.

**Returns:**
- `List[str]` - List of registered plugin names.

### 3. Get Plugin Path
```python
def get_plugin_path(plugin_name: str) -> str:
```
Get path of specified plugin.

**Args:**
- `plugin_name` (str): Plugin name to query.
**Returns:**
- `str` - Plugin path, raises ValueError if plugin doesn't exist.

### 4. Unload Specified Plugin
```python
async def remove_plugin(plugin_name: str) -> bool:
```
Unload specified plugin.

**Args:**
- `plugin_name` (str): Plugin name to unload.

**Returns:**
- `bool` - Whether unloading was successful.

### 5. Reload Specified Plugin
```python
async def reload_plugin(plugin_name: str) -> bool:
```
Reload specified plugin.

**Args:**
- `plugin_name` (str): Plugin name to reload.

**Returns:**
- `bool` - Whether reloading was successful.

### 6. Load Specified Plugin
```python
def load_plugin(plugin_name: str) -> Tuple[bool, int]:
```
Load specified plugin.

**Args:**
- `plugin_name` (str): Plugin name to load.

**Returns:**
- `Tuple[bool, int]` - Whether loading was successful, count of successes or failures.

### 7. Add Plugin Directory
```python
def add_plugin_directory(plugin_directory: str) -> bool:
```
Add plugin directory.

**Args:**
- `plugin_directory` (str): Plugin directory path to add.

**Returns:**
- `bool` - Whether adding was successful.

### 8. Rescan Plugin Directory
```python
def rescan_plugin_directory() -> Tuple[int, int]:
```
Rescan plugin directory, load new plugins.

**Returns:**
- `Tuple[int, int]` - Number of successfully loaded plugins and number of failed plugins.