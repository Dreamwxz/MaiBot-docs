# 🚀 Quick Start Guide

This guide will take you from zero to creating a fully functional MaiCore plugin.

## 📖 Overview

This guide will quickly walk you through creating your first MaiCore plugin. We'll create a simple greeting plugin to demonstrate the basic concepts of the plugin system.

All the following code is in our `plugins/hello_world_plugin/` directory.

### A Convenient Small Design

During development, we define an `__all__` variable in `__init__.py` that contains all classes and functions that need to be exported.
This way, when importing elsewhere, you can directly use `from src.plugin_system import *` to import all plugin-related classes and functions.
Or you can directly use `from src.plugin_system import BasePlugin, register_plugin, ComponentInfo` and similar ways to import the parts you need.

### 📂 Preparation

Make sure you have:

1. Cloned the MaiCore project
2. Installed Python dependencies
3. Understand basic Python syntax

## 🏗️ Creating a Plugin

### 1. Create Plugin Directory

Create your plugin directory in the `plugins/` folder at the project root

Here we create a directory named `hello_world_plugin`

### 2. Create `_manifest.json` File

Create a `_manifest.json` file in the plugin directory with the following content:

```json
{
  "manifest_version": 1,
  "name": "Hello World Plugin",
  "version": "1.0.0",
  "description": "A simple Hello World plugin",
  "author": {
    "name": "Your Name"
  }
}
```

For detailed explanation of `_manifest.json`, please refer to [Manifest File Guide](./manifest-guide.md).

### 3. Create the Simplest Plugin

Let's start with the basics! Create the `plugin.py` file:

```python
from typing import List, Tuple, Type
from src.plugin_system import BasePlugin, register_plugin, ComponentInfo

@register_plugin # Register plugin
class HelloWorldPlugin(BasePlugin):
    """Hello World Plugin - Your first MaiCore plugin"""

    # Below are basic plugin information and methods (must be filled)
    plugin_name = "hello_world_plugin"
    enable_plugin = True  # Enable plugin
    dependencies = []  # Plugin dependency list (currently empty)
    python_dependencies = []  # Python dependency list (currently empty)
    config_file_name = "config.toml"  # Configuration file name
    config_schema = {}  # Configuration file schema (currently empty)

    def get_plugin_components(self) -> List[Tuple[ComponentInfo, Type]]: # Get plugin components
        """Return the list of components included in the plugin (currently empty)"""
        return []
```

🎉 Congratulations! You just created the simplest but complete MaiCore plugin!

**Explanation of this code:**

- First, we define a HelloWorldPlugin plugin class in `plugin.py`, inheriting from `BasePlugin`, providing basic functionality.
- By adding the `@register_plugin` decorator to the class, we tell the system "this is a plugin"
- `plugin_name` etc. are basic plugin information that must be filled
- `get_plugin_components()` returns the plugin's functional components. Currently we haven't defined any Action, Command, or EventHandler, so it returns an empty list.

### 4. Test the Basic Plugin

Now you can test this plugin! Start MaiCore:

Run MaiCore directly through the launcher or `python bot.py`

In the logs, you should see information about the plugin being loaded. Although the plugin doesn't have any functionality yet, it's already running successfully!

![1750326700269](/develop/plugin_develop/image/quick-start/1750326700269.png)

### 5. Add First Functionality: Greeting Action

Now we want to add a useful function to the plugin. Let's start with the most fun Action

Action is a type of "action" that allows MaiCore to choose to use based on its own will. In MaiCore, whether it's "replying" or "not replying", or "sending emojis" and "muting", etc., are all implemented through Actions.

You can expand MaiCore's capabilities by writing actions, including sending voice messages, taking screenshots, even operating files, writing code...

Now let's add the first simple function to the plugin. This Action can send a greeting message to the user.

Add the Action component in the `plugin.py` file, complete code as follows:

```python
from typing import List, Tuple, Type
from src.plugin_system import (
    BasePlugin, register_plugin, BaseAction, 
    ComponentInfo, ActionActivationType, ChatMode
)

# ===== Action Component =====

class HelloAction(BaseAction):
    """Greeting Action - Simple greeting action"""

    # === Basic Information (must be filled) ===
    action_name = "hello_greeting"
    action_description = "Send greeting message to user"
    activation_type = ActionActivationType.ALWAYS  # Always activated

    # === Function Description (must be filled) ===
    action_parameters = {"greeting_message": "Greeting message to send"}
    action_require = ["Use when needing to send friendly greetings", "Use when someone greets you", "Use when you meet someone you haven't seen before"]
    associated_types = ["text"]

    async def execute(self) -> Tuple[bool, str]:
        """Execute greeting action - This is the core functionality"""
        # Send greeting message
        greeting_message = self.action_data.get("greeting_message", "")
        base_message = self.get_config("greeting.message", "Hi! Nice to meet you! 😊")
        message = base_message + greeting_message
        await self.send_text(message)

        return True, "Sent greeting message"

@register_plugin
class HelloWorldPlugin(BasePlugin):
    """Hello World Plugin - Your first MaiCore plugin"""

    # Plugin basic information
    plugin_name = "hello_world_plugin"
    enable_plugin = True
    dependencies = []
    python_dependencies = []
    config_file_name = "config.toml"
    config_schema = {}

    def get_plugin_components(self) -> List[Tuple[ComponentInfo, Type]]:
        """Return the list of components included in the plugin"""
        return [
            # Add our greeting Action
            (HelloAction.get_action_info(), HelloAction),
        ]
```

**Explanation of this code:**

- `HelloAction` is the greeting action class we defined, inheriting from `BaseAction`, and implementing core functionality.
- In `HelloWorldPlugin`, we register `HelloAction` as a component of the plugin through the `get_plugin_components()` method, by calling the built-in method `get_action_info()`.
- This way, when the plugin is loaded, the greeting action will also be loaded and can be used in MaiCore.
- The `execute()` function is the core of the Action, defining what to do specifically when the Action is selected by MaiCore
- `self.send_text()` is a convenient method for sending text messages

For detailed explanation of `activation_type`, `action_parameters`, `action_require`, `associated_types`, etc. in Action components, please refer to [Action Component Guide](./action-components.md).

### 6. Test the Greeting Action

Restart MaiCore, then send any message in chat, for example:

```
Hello
```

MaiCore might choose to use your greeting Action, sending a reply:

```
Hi! Nice to meet you! 😊
```

![1750332508760](/develop/plugin_develop/image/quick-start/1750332508760.png)

> **💡 Tip**: MaiCore intelligently decides when to use it. If you don't see the effect immediately, try different messages a few more times.

🎉 Great! Your plugin now has actual functionality!

### 7. Add Second Functionality: Time Query Command

Now let's add a Command component. Command is different from Action, it directly responds to user commands:

Command is the simplest, most direct response, not judged and selected by LLM

```python
# On top of existing code, add Command component
import datetime
from src.plugin_system import BaseCommand
# Import Command base class

class TimeCommand(BaseCommand):
    """Time Query Command - Respond to /time command"""

    command_name = "time"
    command_description = "Query current time"

    # === Command Settings (must be filled) ===
    command_pattern = r"^/time$"  # Exact match for "/time" command

    async def execute(self) -> Tuple[bool, Optional[str], bool]:
        """Execute time query"""
        # Get current time
        time_format: str = "%Y-%m-%d %H:%M:%S"
        now = datetime.datetime.now()
        time_str = now.strftime(time_format)

        # Send time information
        message = f"⏰ Current time: {time_str}"
        await self.send_text(message)

        return True, f"Displayed current time: {time_str}", True

@register_plugin
class HelloWorldPlugin(BasePlugin):
    """Hello World Plugin - Your first MaiCore plugin"""

    # Plugin basic information
    plugin_name = "hello_world_plugin"
    enable_plugin = True
    dependencies = []
    python_dependencies = []
    config_file_name = "config.toml"
    config_schema = {}

    def get_plugin_components(self) -> List[Tuple[ComponentInfo, Type]]:
        return [
            (HelloAction.get_action_info(), HelloAction),
            (TimeCommand.get_command_info(), TimeCommand),
        ]
```

Similarly, we register `TimeCommand` as a component of the plugin through the `get_plugin_components()` method, by calling the built-in method `get_action_info()`.

**Command Component Explanation:**

- `command_pattern` uses regular expressions to match user input
- `^/time$` means exact match for "/time"

For more information about Command components, please refer to [Command Component Guide](./command-components.md).

### 8. Test Time Query Command

Restart MaiCore, send command:

```
/time
```

You should receive a reply:

```
⏰ Current time: 2024-01-01 12:00:00
```

🎉 Great! Now you understand the basic usage of Action and Command components. You can continue to expand the plugin's functionality according to your needs, add more Action and Command components, making your plugin richer and more powerful!

---

## Advanced Tutorial

If you want to make the plugin more flexible and powerful, you can refer to the following advanced tutorials.

### 1. Add Configuration File

Want to add a configuration file for the plugin? Let's configure the `config_schema` property together!

> **🚨 Important: Do not manually create config.toml file!**
>
> We need to define configuration Schema in the plugin code, letting the system automatically generate configuration files.

First, define configuration Schema in the plugin class:

```python
from src.plugin_system import ConfigField

@register_plugin
class HelloWorldPlugin(BasePlugin):
    """Hello World Plugin - Your first MaiCore plugin"""

    # Plugin basic information
    plugin_name: str = "hello_world_plugin"  # Internal identifier
    enable_plugin: bool = True
    dependencies: List[str] = []  # Plugin dependency list
    python_dependencies: List[str] = []  # Python package dependency list
    config_file_name: str = "config.toml"  # Configuration file name

    # Configuration Schema definition
    config_schema: dict = {
        "plugin": {
            "name": ConfigField(type=str, default="hello_world_plugin", description="Plugin name"),
            "version": ConfigField(type=str, default="1.0.0", description="Plugin version"),
            "enabled": ConfigField(type=bool, default=False, description="Whether to enable plugin"),
        },
        "greeting": {
            "message": ConfigField(type=str, default="Hi! Nice to meet you! 😊", description="Default greeting message"),
            "enable_emoji": ConfigField(type=bool, default=True, description="Whether to enable emoji"),
        },
        "time": {"format": ConfigField(type=str, default="%Y-%m-%d %H:%M:%S", description="Time display format")},
    }

    def get_plugin_components(self) -> List[Tuple[ComponentInfo, Type]]:
        return [
            (HelloAction.get_action_info(), HelloAction),
            (TimeCommand.get_command_info(), TimeCommand),
        ]
```

This will generate a `config.toml` file like this:

```toml
# hello_world_plugin - Automatically generated configuration file
# My first MaiCore plugin, containing basic examples like greeting functionality and time query

# Plugin basic information
[plugin]

# Plugin name
name = "hello_world_plugin"

# Plugin version
version = "1.0.0"

# Whether to enable plugin
enabled = false


# Greeting functionality configuration
[greeting]

# Default greeting message
message = "Hi! Nice to meet you! 😊"

# Whether to enable emoji
enable_emoji = true


# Time query configuration
[time]

# Time display format
format = "%Y-%m-%d %H:%M:%S"
```

Then modify the Action and Command code to read configuration through the `get_config()` method (configuration keys are namespace-style):

```python
class HelloAction(BaseAction):
    """Greeting Action - Simple greeting action"""

    # === Basic Information (must be filled) ===
    action_name = "hello_greeting"
    action_description = "Send greeting message to user"
    activation_type = ActionActivationType.ALWAYS  # Always activated

    # === Function Description (must be filled) ===
    action_parameters = {"greeting_message": "Greeting message to send"}
    action_require = ["Use when needing to send friendly greetings", "Use when someone greets you", "Use when you meet someone you haven't seen before"]
    associated_types = ["text"]

    async def execute(self) -> Tuple[bool, str]:
        """Execute greeting action - This is the core functionality"""
        # Send greeting message
        greeting_message = self.action_data.get("greeting_message", "")
        base_message = self.get_config("greeting.message", "Hi! Nice to meet you! 😊")
        message = base_message + greeting_message
        await self.send_text(message)

        return True, "Sent greeting message"

class TimeCommand(BaseCommand):
    """Time Query Command - Respond to /time command"""

    command_name = "time"
    command_description = "Query current time"

    # === Command Settings (must be filled) ===
    command_pattern = r"^/time$"  # Exact match for "/time" command

    async def execute(self) -> Tuple[bool, str, bool]:
        """Execute time query"""
        import datetime

        # Get current time
        time_format: str = self.get_config("time.format", "%Y-%m-%d %H:%M:%S")  # type: ignore
        now = datetime.datetime.now()
        time_str = now.strftime(time_format)

        # Send time information
        message = f"⏰ Current time: {time_str}"
        await self.send_text(message)

        return True, f"Displayed current time: {time_str}", True
```

**Configuration System Workflow:**

1. **Define Schema**: Define configuration structure in plugin code
2. **Auto-generate**: When starting the plugin, the system automatically generates `config.toml` file
3. **User Modification**: Users can modify the generated configuration file
4. **Code Reading**: Use `self.get_config()` to read configuration values

**Absolutely do not manually create `config.toml` file!**

For more detailed configuration system introduction, please refer to [Configuration Guide](./configuration-guide.md).

### 2. Create Documentation

You can create a `README.md` file describing the plugin's functionality and usage methods.

### 3. Publish to Plugin Market

If you want more people to use your plugin, you can publish it to MaiCore's plugin market.

For this part, please refer to the documentation of [plugin-repo](https://github.com/Maim-with-u/plugin-repo).

---

🎉 Congratulations! You have successfully created your own plugin!