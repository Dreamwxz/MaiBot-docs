# MaiBot Plugin Development Documentation

> Welcome to the MaiBot plugin system development documentation! This is the best starting point for your plugin development journey.

## Getting Started

- [📖 Quick Start Guide](quick-start.md) - Quickly create your first plugin

## Component Function Details

- [🧱 Action Component Details](action-components.md) - Master the core Action component
- [💻 Command Component Details](command-components.md) - Learn components that directly respond to commands
- [🔧 Tool Component Details](tool-components.md) - Understand how to extend information acquisition capabilities
- [⚙️ Configuration File System Guide](configuration-guide.md) - Learn to use automatically generated plugin configuration files
- [📄 Manifest System Guide](manifest-guide.md) - Understand plugin metadata management and configuration architecture

## Command vs Action vs Tool Selection Guide

1. When to use Command

- ✅ Users need to explicitly call specific functions
- ✅ Require precise parameter control
- ✅ Management and configuration operations
- ✅ Query and information display

2. When to use Action

- ✅ Enhance MaiMai's intelligent behavior
- ✅ Automatically triggered based on context
- ✅ Emotion and expression display
- ✅ Randomized interactions

3. When to use Tool

- Directly provide information to MaiMai
- Provide more available information when replying
- Simple to use, specialized functionality

## API Browser

### Message Sending and Processing API
- [📤 Send API](api/send-api.md) - Various types of message sending interfaces
- [Message API](api/message-api.md) - Message acquisition, message construction, message query interfaces
- [Chat Flow API](api/chat-api.md) - Chat flow management and query interfaces

### AI and Generation API  
- [LLM API](api/llm-api.md) - Large language model interaction interface, can use built-in LLM to generate content
- [✨ Reply Generator API](api/generator-api.md) - Intelligent reply generation interface, can use built-in stylized generators

### Emoji API
- [😊 Emoji API](api/emoji-api.md) - Emoji selection and management interface

### Relationship System API
- [Person Information API](api/person-api.md) - User information, interfaces for handling people MaiMai knows and relationships

### Data and Configuration API
- [🗄️ Database API](api/database-api.md) - Database operation interfaces
- [⚙️ Configuration API](api/config-api.md) - Configuration reading and user information interfaces

### Plugin and Component Management API
- [🔌 Plugin API](api/plugin-manage-api.md) - Plugin loading and management interfaces
- [🧩 Component API](api/component-manage-api.md) - Component registration and management interfaces

### Logging API
- [📜 Logging API](api/logging-api.md) - Logger instance acquisition interface
### Tool API
- [🔧 Tool API](api/tool-api.md) - Tool acquisition interface

## A Convenient Small Design

We define an `__all__` variable in `__init__.py` that contains all classes and functions that need to be exported.
This way, when importing elsewhere, you can directly use `from src.plugin_system import *` to import all plugin-related classes and functions.
Or you can directly use `from src.plugin_system import BasePlugin, register_plugin, ComponentInfo` and similar ways to import the parts you need.