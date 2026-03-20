# Plugin Configuration Schema Development Documentation

This document introduces how to use MaiBot's plugin configuration system to make your plugin support WebUI visual configuration.

## Overview

MaiBot's plugin configuration system allows plugin developers to define configuration items declaratively, and WebUI automatically generates configuration forms based on the configuration Schema.

### Core Components

| Component | Description |
|------|------|
| `ConfigField` | Configuration field definition, describing type, default value, UI control, etc. of a single configuration item |
| `ConfigSection` | Configuration section metadata, describing title, icon, etc. of a group of related configurations |
| `ConfigTab` | Tab definition, used to organize multiple sections into a tab |
| `ConfigLayout` | Page layout definition, supporting automatic layout, tab layout, etc. |

## Quick Start

### Basic Usage

```python
from src.plugin_system.base.base_plugin import BasePlugin
from src.plugin_system.base.config_types import ConfigField

class MyPlugin(BasePlugin):
    plugin_name = "my_plugin"
    enable_plugin = True
    dependencies = []
    python_dependencies = []
    config_file_name = "config.toml"
    
    # Define configuration Schema
    config_schema = {
        "plugin": {
            "name": ConfigField(
                type=str,
                default="my_plugin",
                description="Plugin name",
                required=True
            ),
            "enabled": ConfigField(
                type=bool,
                default=True,
                description="Whether to enable plugin"
            ),
        },
        "api": {
            "key": ConfigField(
                type=str,
                default="",
                description="API key",
                input_type="password",
                placeholder="Please enter your API key"
            ),
            "timeout": ConfigField(
                type=int,
                default=30,
                description="Request timeout (seconds)",
                min=1,
                max=120
            ),
        }
    }
    
    # Optional: Section metadata
    config_section_descriptions = {
        "plugin": "Plugin basic information",
        "api": "API configuration",
    }
```

## ConfigField Details

### Constructor Parameters

#### Basic Fields (Required)

| Parameter | Type | Description |
|------|------|------|
| `type` | `type` | Field type: `str`, `int`, `float`, `bool`, `list`, `dict` |
| `default` | `Any` | Default value |
| `description` | `str` | Field description, also used as default label |

#### Validation Related

| Parameter | Type | Description |
|------|------|------|
| `required` | `bool` | Whether required, default `False` |
| `choices` | `List[Any]` | Optional value list, used for dropdown selection |
| `min` | `float` | Minimum value (numeric types) |
| `max` | `float` | Maximum value (numeric types) |
| `step` | `float` | Step value (numeric types) |
| `pattern` | `str` | Regular validation (string type) |
| `max_length` | `int` | Maximum length (string type) |
| `example` | `str` | Example value, used for generating configuration file comments |

#### UI Display Control

| Parameter | Type | Description |
|------|------|------|
| `label` | `str` | Display label, default uses `description` |
| `placeholder` | `str` | Input box placeholder |
| `hint` | `str` | Hint text below field |
| `icon` | `str` | Field icon name |
| `hidden` | `bool` | Whether hidden in UI, default `False` |
| `disabled` | `bool` | Whether editing disabled, default `False` |
| `order` | `int` | Sorting weight, smaller numbers appear first, default `0` |

#### Input Control Types

| Parameter | Type | Description |
|------|------|------|
| `input_type` | `str` | Force specify control type, automatically inferred if not specified |
| `rows` | `int` | textarea row count, default `3` |

Supported `input_type` values:

| Value | Description |
|------|------|
| `text` | Single-line text input box |
| `password` | Password input box (with show/hide toggle) |
| `textarea` | Multi-line text area |
| `number` | Number input box |
| `switch` | Switch toggle (boolean value) |
| `slider` | Slider (needs to cooperate with min/max) |
| `select` | Dropdown selection (needs to cooperate with choices) |
| `list` | Dynamic list editor (supports drag-and-drop sorting) |
| `color` | Color picker (planned) |
| `code` | Code editor (planned) |
| `file` | File upload (planned) |
| `json` | JSON editor (planned) |

#### Grouping and Conditional Display

| Parameter | Type | Description |
|------|------|------|
| `group` | `str` | Field grouping, further subdivision within section |
| `depends_on` | `str` | Dependent field path, e.g., `"section.field"` |
| `depends_value` | `Any` | Value needed for dependent field |

#### List Type Specific

| Parameter | Type | Description |
|------|------|------|
| `item_type` | `str` | Array element type: `"string"`, `"number"`, `"object"` |
| `item_fields` | `Dict[str, Any]` | When `item_type="object"`, define object field structure |
| `min_items` | `int` | Array minimum element count |
| `max_items` | `int` | Array maximum element count |

**List Type Examples:**

```python
# Simple string list
"keywords": ConfigField(
    type=list,
    default=["Keyword1", "Keyword2"],
    description="Trigger keywords",
    item_type="string",
    placeholder="Enter keyword",
    max_items=20,
    hint="Supports up to 20 keywords"
),

# Number list
"retry_delays": ConfigField(
    type=list,
    default=[1, 2, 5, 10],
    description="Retry delays (seconds)",
    item_type="number",
    min_items=1,
    max_items=5,
),

# Object list (complex structure)
"api_endpoints": ConfigField(
    type=list,
    default=[{"name": "Main server", "url": "https://api.example.com"}],
    description="API endpoint list",
    item_type="object",
    item_fields={
        "name": {"type": "string", "label": "Name", "placeholder": "Endpoint name"},
        "url": {"type": "string", "label": "URL", "placeholder": "https://..."},
        "priority": {"type": "number", "label": "Priority", "default": 0},
    },
    min_items=1,
    max_items=5,
),
```

### Control Auto-inference Rules

If `input_type` not specified, system automatically selects control based on following rules:

| Condition | Control Type |
|------|----------|
| `type=bool` | Switch toggle |
| `type=int/float` + has `min` and `max` | Slider |
| `type=int/float` | NumberInput number input box |
| `type=str` + has `choices` | Select dropdown selection |
| `type=str` + `input_type="password"` | Password box |
| `type=str` + `input_type="textarea"` | Textarea text area |
| `type=list` | DynamicList dynamic list |
| `type=dict` | JSON editor |
| `type=str` (default) | Input text box |

### Examples

```python
config_schema = {
    "api": {
        # Password input box
        "api_key": ConfigField(
            type=str,
            default="",
            description="API key",
            input_type="password",
            placeholder="sk-xxxxxxxx",
            required=True,
            hint="Obtain from service provider console"
        ),
        
        # Slider with range restriction
        "temperature": ConfigField(
            type=float,
            default=0.7,
            description="Generation temperature",
            min=0.0,
            max=2.0,
            step=0.1,
            hint="Higher values make output more random"
        ),
        
        # Dropdown selection
        "model": ConfigField(
            type=str,
            default="gpt-4",
            description="Model to use",
            choices=["gpt-3.5-turbo", "gpt-4", "gpt-4-turbo"],
            icon="cpu"
        ),
        
        # Multi-line text
        "system_prompt": ConfigField(
            type=str,
            default="",
            description="System prompt",
            input_type="textarea",
            rows=5,
            placeholder="Enter system prompt..."
        ),
        
        # Conditional display: only shows when debug is True
        "debug_log_path": ConfigField(
            type=str,
            default="./debug.log",
            description="Debug log path",
            depends_on="debug.enabled",
            depends_value=True
        ),
    }
}
```

## ConfigSection Details

Used to add metadata to configuration sections, such as title, icon, whether collapsed by default, etc.

### Method 1: Simple String

```python
config_section_descriptions = {
    "plugin": "Plugin basic information",
    "api": "API configuration",
}
```

### Method 2: Using ConfigSection

```python
from src.plugin_system.base.config_types import ConfigSection

config_section_descriptions = {
    "plugin": ConfigSection(
        title="Plugin basic information",
        description="Configure plugin basic properties",
        icon="settings",
        order=1
    ),
    "api": ConfigSection(
        title="API configuration",
        description="External API connection parameters",
        icon="cloud",
        order=2
    ),
    "advanced": ConfigSection(
        title="Advanced settings",
        icon="code",
        collapsed=True,  # Collapsed by default
        order=99
    ),
}
```

### Method 3: Using Convenience Function

```python
from src.plugin_system.base.config_types import section_meta

config_section_descriptions = {
    "plugin": section_meta("Plugin basic information", icon="settings", order=1),
    "api": section_meta("API configuration", icon="cloud", order=2),
    "advanced": section_meta("Advanced settings", collapsed=True, order=99),
}
```

### ConfigSection Parameters

| Parameter | Type | Default Value | Description |
|------|------|--------|------|
| `title` | `str` | Required | Display title |
| `description` | `str` | `None` | Detailed description |
| `icon` | `str` | `None` | Icon name |
| `collapsed` | `bool` | `False` | Whether collapsed by default |
| `order` | `int` | `0` | Sorting weight |

## Custom Layout

By default, all sections are displayed as collapsible panels. If more complex layout needed, can use `config_layout`.

### Tab Layout

```python
from src.plugin_system.base.config_types import ConfigLayout, ConfigTab

class MyPlugin(BasePlugin):
    # ... other properties ...
    
    config_schema = {
        "plugin": { ... },
        "api": { ... },
        "model": { ... },
        "debug": { ... },
        "logging": { ... },
    }
    
    config_layout = ConfigLayout(
        type="tabs",
        tabs=[
            ConfigTab(
                id="basic",
                title="Basic settings",
                sections=["plugin", "api"],
                icon="settings"
            ),
            ConfigTab(
                id="model",
                title="Model configuration",
                sections=["model"],
                icon="cpu"
            ),
            ConfigTab(
                id="dev",
                title="Developer",
                sections=["debug", "logging"],
                icon="terminal",
                badge="Dev"  # Display badge
            ),
        ]
    )
```

### ConfigTab Parameters

| Parameter | Type | Default Value | Description |
|------|------|--------|------|
| `id` | `str` | Required | Tab ID |
| `title` | `str` | Required | Display title |
| `sections` | `List[str]` | `[]` | Included section name list |
| `icon` | `str` | `None` | Icon name |
| `order` | `int` | `0` | Sorting weight |
| `badge` | `str` | `None` | Badge text |

### ConfigLayout Parameters

| Parameter | Type | Default Value | Description |
|------|------|--------|------|
| `type` | `str` | `"auto"` | Layout type: `auto`, `tabs`, `pages` |
| `tabs` | `List[ConfigTab]` | `[]` | Tab list |

Layout type description:

| Type | Description |
|------|------|
| `auto` | Automatic layout, sections displayed as collapsible panels |
| `tabs` | Tab layout, top tab switching |
| `pages` | Page layout, left navigation + right content (planned) |

## Complete Example

```python
from src.plugin_system.base.base_plugin import BasePlugin
from src.plugin_system.base.config_types import (
    ConfigField,
    ConfigSection,
    ConfigLayout,
    ConfigTab,
)
from src.plugin_system.apis.plugin_register_api import register_plugin


@register_plugin
class TTSPlugin(BasePlugin):
    """TTS Voice Synthesis Plugin"""
    
    plugin_name = "tts_plugin"
    enable_plugin = True
    dependencies = []
    python_dependencies = []
    config_file_name = "config.toml"
    
    # Configuration Schema
    config_schema = {
        "plugin": {
            "name": ConfigField(
                type=str,
                default="tts_plugin",
                description="Plugin name",
                required=True
            ),
            "version": ConfigField(
                type=str,
                default="1.0.0",
                description="Plugin version"
            ),
            "enabled": ConfigField(
                type=bool,
                default=True,
                description="Whether to enable plugin"
            ),
        },
        "tts": {
            "provider": ConfigField(
                type=str,
                default="azure",
                description="TTS service provider",
                choices=["azure", "google", "local"],
                order=1
            ),
            "api_key": ConfigField(
                type=str,
                default="",
                description="API key",
                input_type="password",
                required=True,
                order=2
            ),
            "voice": ConfigField(
                type=str,
                default="zh-CN-XiaoxiaoNeural",
                description="Voice role",
                placeholder="e.g.: zh-CN-XiaoxiaoNeural",
                order=3
            ),
            "speed": ConfigField(
                type=float,
                default=1.0,
                description="Speech speed",
                min=0.5,
                max=2.0,
                step=0.1,
                order=4
            ),
        },
        "cache": {
            "enabled": ConfigField(
                type=bool,
                default=True,
                description="Enable cache"
            ),
            "max_size": ConfigField(
                type=int,
                default=100,
                description="Maximum cache count",
                min=10,
                max=1000,
                depends_on="cache.enabled",
                depends_value=True
            ),
            "ttl": ConfigField(
                type=int,
                default=3600,
                description="Cache expiration time (seconds)",
                min=60,
                depends_on="cache.enabled",
                depends_value=True
            ),
        },
        "debug": {
            "enabled": ConfigField(
                type=bool,
                default=False,
                description="Debug mode"
            ),
            "log_level": ConfigField(
                type=str,
                default="INFO",
                description="Log level",
                choices=["DEBUG", "INFO", "WARNING", "ERROR"]
            ),
        },
    }
    
    # Section metadata
    config_section_descriptions = {
        "plugin": ConfigSection(
            title="Plugin information",
            icon="info",
            order=1
        ),
        "tts": ConfigSection(
            title="TTS configuration",
            description="Voice synthesis related settings",
            icon="volume-2",
            order=2
        ),
        "cache": ConfigSection(
            title="Cache settings",
            icon="database",
            order=3
        ),
        "debug": ConfigSection(
            title="Debug options",
            icon="bug",
            collapsed=True,
            order=99
        ),
    }
    
    # Custom layout (optional)
    config_layout = ConfigLayout(
        type="tabs",
        tabs=[
            ConfigTab(id="main", title="Main settings", sections=["plugin", "tts"]),
            ConfigTab(id="advanced", title="Advanced", sections=["cache", "debug"]),
        ]
    )
    
    # ... plugin other implementations ...
```

## Reading Configuration in Plugin

```python
class MyPlugin(BasePlugin):
    def some_method(self):
        # Read single configuration item
        api_key = self.get_config("api.key")
        timeout = self.get_config("api.timeout", default=30)
        
        # Read entire section
        api_config = self.config.get("api", {})
        
        # Check if enabled
        if self.get_config("plugin.enabled", True):
            # Execute logic
            pass
```

## WebUI API

WebUI provides following APIs for configuration management:

| Endpoint | Method | Description |
|------|------|------|
| `/api/webui/plugins/config/{plugin_id}/schema` | GET | Get plugin configuration Schema |
| `/api/webui/plugins/config/{plugin_id}` | GET | Get current configuration values |
| `/api/webui/plugins/config/{plugin_id}` | PUT | Update configuration |
| `/api/webui/plugins/config/{plugin_id}/reset` | POST | Reset to default configuration |
| `/api/webui/plugins/config/{plugin_id}/toggle` | POST | Toggle enable/disable status |

## Best Practices

### 1. Reasonably Organize Sections

Put related configuration items in same section, use meaningful section names.

```python
# Good practice
config_schema = {
    "api": { ... },      # API related
    "model": { ... },    # Model related
    "cache": { ... },    # Cache related
}

# Avoid
config_schema = {
    "settings": { ... },  # Too general
    "misc": { ... },      # Not clear
}
```

### 2. Provide Meaningful Descriptions and Hints

```python
ConfigField(
    type=str,
    default="",
    description="OpenAI API key",  # Clear description
    hint="Obtain from platform.openai.com",  # Helpful hint
    placeholder="sk-..."  # Format example
)
```

### 3. Use Appropriate Control Types

```python
# Sensitive information use password box
"api_key": ConfigField(type=str, input_type="password", ...)

# Limited options use dropdown
"model": ConfigField(type=str, choices=["gpt-3.5", "gpt-4"], ...)

# Range values use slider
"temperature": ConfigField(type=float, min=0, max=2, ...)

# Long text use textarea
"prompt": ConfigField(type=str, input_type="textarea", rows=5, ...)
```

### 4. Set Reasonable Default Values

```python
ConfigField(
    type=int,
    default=30,  # Reasonable default value
    min=1,
    max=120,
    description="Timeout (seconds)"
)
```

### 5. Use Conditional Display to Reduce Interface Complexity

```python
"debug_enabled": ConfigField(type=bool, default=False, ...),
"debug_log_path": ConfigField(
    type=str,
    depends_on="debug.debug_enabled",
    depends_value=True,
    ...
)
```

### 6. Use order to Control Display Order

```python
"important_field": ConfigField(..., order=1),
"less_important": ConfigField(..., order=10),
"rarely_used": ConfigField(..., order=99),
```

## Migration Guide

If your plugin uses old version `config_schema`, upgrading is very simple:

### Old Version

```python
config_schema = {
    "api": {
        "key": ConfigField(type=str, default="", description="API key"),
    }
}
```

### New Version (Fully Compatible)

Old code can continue working without modification. If need to use new features, just add new parameters:

```python
config_schema = {
    "api": {
        "key": ConfigField(
            type=str,
            default="",
            description="API key",
            # New parameters
            input_type="password",
            placeholder="Please enter API key",
            hint="Obtain from console",
            required=True,
            order=1
        ),
    }
}
```

## Frequently Asked Questions

### Q: When do configuration modifications take effect?

A: After configuration saved to TOML file, need to reload plugin to take effect. WebUI will display prompt information.

### Q: How to validate configuration values?

A: Currently supports basic validation (required, range, regex), complex validation logic needs to be implemented in plugin code.

### Q: Which icons are supported?

A: Icons use Lucide Icons, reference https://lucide.dev/icons

### Q: How to handle sensitive configuration?

A: Using `input_type="password"` will hide input content in UI, but still plain text storage in configuration file. If need encrypted storage, need to implement yourself.