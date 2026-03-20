# Component Management API

The Component Management API module provides querying and management functionality for plugin components, enabling plugins to obtain and use component-related information.

## Import Method
```python
from src.plugin_system.apis import component_manage_api
# or
from src.plugin_system import component_manage_api
```

## Function Overview

The Component Management API mainly provides the following functions:
- **Plugin Information Query** - Get information about all plugins or specified plugins.
- **Component Query** - Query component information by name or type.
- **Component Management** - Enable or disable components, supporting global and local operations.

## Main Functions

### 1. Get All Plugin Information
```python
def get_all_plugin_info() -> Dict[str, PluginInfo]:
```
Get information about all plugins.

**Returns:**
- `Dict[str, PluginInfo]` - Dictionary containing all plugin information, keys are plugin names, values are `PluginInfo` objects.

### 2. Get Specified Plugin Information
```python
def get_plugin_info(plugin_name: str) -> Optional[PluginInfo]:
```
Get information about specified plugin.

**Args:**
- `plugin_name` (str): Plugin name.

**Returns:**
- `Optional[PluginInfo]`: Plugin information object, returns `None` if plugin doesn't exist.

### 3. Get Specified Component Information
```python
def get_component_info(component_name: str, component_type: ComponentType) -> Optional[Union[CommandInfo, ActionInfo, EventHandlerInfo]]:
```
Get information about specified component.

**Args:**
- `component_name` (str): Component name.
- `component_type` (ComponentType): Component type.

**Returns:**
- `Optional[Union[CommandInfo, ActionInfo, EventHandlerInfo]]`: Component information object, returns `None` if component doesn't exist.

### 4. Get All Component Information of Specified Type
```python
def get_components_info_by_type(component_type: ComponentType) -> Dict[str, Union[CommandInfo, ActionInfo, EventHandlerInfo]]:
```
Get all component information of specified type.

**Args:**
- `component_type` (ComponentType): Component type.

**Returns:**
- `Dict[str, Union[CommandInfo, ActionInfo, EventHandlerInfo]]`: Dictionary containing component information of specified type, keys are component names, values are corresponding component information objects.

### 5. Get All Enabled Component Information of Specified Type
```python
def get_enabled_components_info_by_type(component_type: ComponentType) -> Dict[str, Union[CommandInfo, ActionInfo, EventHandlerInfo]]:
```
Get all enabled component information of specified type.

**Args:**
- `component_type` (ComponentType): Component type.

**Returns:**
- `Dict[str, Union[CommandInfo, ActionInfo, EventHandlerInfo]]`: Dictionary containing enabled component information of specified type, keys are component names, values are corresponding component information objects.

### 6. Get Registered Action Information
```python
def get_registered_action_info(action_name: str) -> Optional[ActionInfo]:
```
Get registered information for specified Action.

**Args:**
- `action_name` (str): Action name.

**Returns:**
- `Optional[ActionInfo]` - Action information object, returns `None` if Action doesn't exist.

### 7. Get Registered Command Information
```python
def get_registered_command_info(command_name: str) -> Optional[CommandInfo]:
```
Get registered information for specified Command.

**Args:**
- `command_name` (str): Command name.

**Returns:**
- `Optional[CommandInfo]` - Command information object, returns `None` if Command doesn't exist.

### 8. Get Registered Tool Information
```python
def get_registered_tool_info(tool_name: str) -> Optional[ToolInfo]:
```
Get registered information for specified Tool.

**Args:**
- `tool_name` (str): Tool name.

**Returns:**
- `Optional[ToolInfo]` - Tool information object, returns `None` if Tool doesn't exist.

### 9. Get Registered EventHandler Information
```python
def get_registered_event_handler_info(event_handler_name: str) -> Optional[EventHandlerInfo]:
```
Get registered information for specified EventHandler.

**Args:**
- `event_handler_name` (str): EventHandler name.

**Returns:**
- `Optional[EventHandlerInfo]` - EventHandler information object, returns `None` if EventHandler doesn't exist.

### 10. Globally Enable Specified Component
```python
def globally_enable_component(component_name: str, component_type: ComponentType) -> bool:
```
Globally enable specified component.

**Args:**
- `component_name` (str): Component name.
- `component_type` (ComponentType): Component type.

**Returns:**
- `bool` - Returns `True` if enabling successful, otherwise `False`.

### 11. Globally Disable Specified Component
```python
async def globally_disable_component(component_name: str, component_type: ComponentType) -> bool:
```
Globally disable specified component.

**This function is asynchronous, ensure calling in asynchronous environment.**

**Args:**
- `component_name` (str): Component name.
- `component_type` (ComponentType): Component type.

**Returns:**
- `bool` - Returns `True` if disabling successful, otherwise `False`.

### 12. Locally Enable Specified Component
```python
def locally_enable_component(component_name: str, component_type: ComponentType, stream_id: str) -> bool:
```
Locally enable specified component.

**Args:**
- `component_name` (str): Component name.
- `component_type` (ComponentType): Component type.
- `stream_id` (str): Message stream ID.

**Returns:**
- `bool` - Returns `True` if enabling successful, otherwise `False`.

### 13. Locally Disable Specified Component
```python
def locally_disable_component(component_name: str, component_type: ComponentType, stream_id: str) -> bool:
```
Locally disable specified component.

**Args:**
- `component_name` (str): Component name.
- `component_type` (ComponentType): Component type.
- `stream_id` (str): Message stream ID.

**Returns:**
- `bool` - Returns `True` if disabling successful, otherwise `False`.

### 14. Get Locally Disabled Component List for Specified Message Stream
```python
def get_locally_disabled_components(stream_id: str, component_type: ComponentType) -> list[str]:
```
Get locally disabled component list for specified message stream.

**Args:**
- `stream_id` (str): Message stream ID.
- `component_type` (ComponentType): Component type.

**Returns:**
- `list[str]` - Disabled component name list.