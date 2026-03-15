# 📄 Plugin Manifest System Guide

## Overview

The MaiBot plugin system now requires every plugin to include a `_manifest.json` file. This file describes important metadata such as basic plugin information, dependencies, components, etc.

### 🔄 Configuration Architecture: Manifest and Config Responsibility Separation

To avoid information duplication and improve maintainability, we adopt a **dual-file architecture**:

- **`_manifest.json`** - Plugin's **static metadata**
  - Plugin identity information (name, version, description)
  - Developer information (author, license, repository)
  - System information (compatibility, component list, categories)
  
- **`config.toml`** - Plugin's **runtime configuration**
  - Enable status (`enabled`)
  - Function parameter configuration
  - User-adjustable behavior settings

This separation ensures:
- ✅ Unified management of metadata information
- ✅ Flexible adjustment of runtime configuration  
- ✅ Avoid duplicate maintenance
- ✅ Clearer responsibility division

## 🔧 Manifest File Structure

### Required Fields

The following fields are required and cannot be empty:

```json
{
  "manifest_version": 1,
  "name": "Plugin Display Name",
  "version": "1.0.0",
  "description": "Plugin Function Description",
  "author": {
    "name": "Author Name"
  }
}
```

### Optional Fields

The following fields are all optional and can be added as needed:

```json
{
  "license": "MIT",
  "host_application": {
    "min_version": "1.0.0",
    "max_version": "4.0.0"
  },
  "homepage_url": "https://github.com/your-repo",
  "repository_url": "https://github.com/your-repo",
  "keywords": ["Keyword1", "Keyword2"],
  "categories": ["Category1", "Category2"],
  "default_locale": "zh-CN",
  "locales_path": "_locales",
  "plugin_info": {
    "is_built_in": false,
    "plugin_type": "general",
    "components": [
      {
        "type": "action",
        "name": "Component Name",
        "description": "Component Description"
      }
    ]
  }
}
```

## 🛠️ Management Tools

### Using manifest_tool.py

We provide a command-line tool to help manage manifest files:

```bash
# Scan for plugins missing manifest
python scripts/manifest_tool.py scan src/plugins

# Create minimal manifest file for plugin
python scripts/manifest_tool.py create-minimal src/plugins/my_plugin --name "My Plugin" --author "Author"

# Create complete manifest template for plugin
python scripts/manifest_tool.py create-complete src/plugins/my_plugin --name "My Plugin"

# Validate manifest file
python scripts/manifest_tool.py validate src/plugins/my_plugin
```

### Validation Examples

Validation passed example:
```
✅ Manifest file validation passed
```

Validation failed example:
```
❌ Validation errors:
  - Missing required field: name
  - Author information missing name field or empty
⚠️ Validation warnings:
  - Recommended to fill field: license
  - Recommended to fill field: keywords
```

## 🔄 Migration Guide

### For Existing Plugins

1. **Check for plugins missing manifest**:
   ```bash
   python scripts/manifest_tool.py scan src/plugins
   ```

2. **Create manifest for each plugin**:
   ```bash
   python scripts/manifest_tool.py create-minimal src/plugins/your_plugin
   ```

3. **Edit manifest file**, fill in correct information.

4. **Validate manifest**:
   ```bash
   python scripts/manifest_tool.py validate src/plugins/your_plugin
   ```

### For New Plugins

When creating new plugins, recommended steps:

1. **Create plugin directory and basic files**
2. **Create complete manifest template**:
   ```bash
   python scripts/manifest_tool.py create-complete src/plugins/new_plugin
   ```
3. **Modify manifest file according to actual situation**
4. **Write plugin code**
5. **Validate manifest file**

## 📋 Field Descriptions

### Basic Information
- `manifest_version`: Manifest format version, currently 1
- `name`: Plugin display name (required)
- `version`: Plugin version number (required)
- `description`: Plugin function description (required)
- `author`: Author information (required)
  - `name`: Author name (required)
  - `url`: Author homepage (optional)

### License and URLs
- `license`: Plugin license (optional, recommended to fill)
- `homepage_url`: Plugin homepage (optional)
- `repository_url`: Source code repository address (optional)

### Categories and Tags
- `keywords`: Keyword array (optional, recommended to fill)
- `categories`: Category array (optional, recommended to fill)
  
#### 🏷️ Categories Classification System Explanation

The plugin system uses a predefined classification system to organize and manage plugins. The `categories` field must use one or more of the following **English category identifiers**:

| English Category Identifier | Chinese Display Name | Description |
|---------------|-------------|------|
| `Group Management` | 群组管理 | Group-related management function plugins |
| `Entertainment & Interaction` | 娱乐互动 | Entertainment and interaction plugins |
| `Utility Tools` | 实用工具 | Utility tool plugins |
| `Content Generation` | 内容生成 | Content generation related plugins |
| `Multimedia` | 多媒体 | Multimedia processing plugins |
| `External Integration` | 外部集成 | External service integration plugins |
| `Data Analysis & Insights` | 数据分析与洞察 | Data analysis related plugins |
| `Other` | 其他 | Other uncategorized plugins |

**Example usage**:
```json
{
  "categories": ["Entertainment & Interaction"],
  // Or multiple categories
  "categories": ["Utility Tools", "External Integration"]
}
```

⚠️ **Important reminder**:
- Must use **English category identifiers** from the table, otherwise plugins cannot be correctly classified
- If unsure about category, recommend using `Other`
- The system automatically maps English categories to Chinese display names

### Compatibility
- `host_application`: Host application compatibility (required)
  - `min_version`: Minimum compatible version (required)
  - `max_version`: Maximum compatible version (optional, but recommended to fill, defaults to latest version if not filled)

⚠️ When not filling in the maximum compatible version, the plugin will default to support all versions above the minimum version. **(Since we have done extensive refactoring of the plugin system across different versions, this situation is almost impossible.)**

### Internationalization
- `default_locale`: Default language (optional)
- `locales_path`: Language file directory (optional)

### Plugin Specific Information
- `plugin_info`: Plugin detailed information (optional)
  - `is_built_in`: Whether it's a built-in plugin
  - `plugin_type`: Plugin type
  - `components`: Component list

## Example File

Below is an example `manifest.json` file following the above good practices.

```json
{
  "manifest_version": 1, # File version
  "name": "Example Plugin", # Plugin name
  "version": "1.0.0", # Plugin version
  "description": "This is an example plugin", # Plugin introduction
  "author": { # Plugin author information
    "name": "MaiBot Team", # Plugin author name
    "url": "https://github.com/MaiM-with-u", # Plugin author homepage
  },
  "license": "MIT", # Plugin license version

  "host_application": {
    "min_version": "0.7.0", # Minimum MaiMai version plugin adapts to
    "max_version": "0.8.0" # (Optional) Maximum MaiMai version plugin adapts to
  },
  "homepage_url": "https://github.com/MaiM-with-u", # (Optional) Plugin homepage
  "repository_url": "https://github.com/MaiM-with-u/plugin-repo", # (Optional) Plugin repository address
  "keywords": ["Example"], # Plugin keywords
  "categories": ["Other"], # Plugin categories
  
  "default_locale": "CN", # Plugin default language
  "locales_path": "_locales" # (Optional) Plugin language folder
}
```

## ⚠️ Notes

1. **Mandatory requirement**: All plugins must include `_manifest.json` file, otherwise cannot be loaded
2. **Encoding format**: Manifest file must use UTF-8 encoding
3. **JSON format**: File must be valid JSON format
4. **Required fields**: `manifest_version`, `name`, `version`, `description`, `author.name` are required
5. **Version compatibility**: Currently only supports `manifest_version = 1`

## 🔍 Frequently Asked Questions

### Q: Can optional fields be left unfilled?
A: Yes. All fields marked as "optional" can be left unfilled, but it's recommended to at least fill `license` and `keywords`.

### Q: What if manifest validation fails?
A: Fix corresponding issues according to validator's error prompts. Errors will cause plugin loading to fail, warnings won't.

## 📚 Reference Examples

Check built-in plugin manifest files as reference:
- `src/plugins/built_in/core_actions/_manifest.json`
- `src/plugins/built_in/tts_plugin/_manifest.json`
- `src/plugins/hello_world_plugin/_manifest.json`