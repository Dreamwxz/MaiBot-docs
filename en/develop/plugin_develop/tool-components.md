# 🔧 Tool Component Details

## 📖 What is Tool

Tool is MaiBot's information acquisition capability extension component. If Action components have various functions and can expand what MaiMai can do, then Tool expands the amount of information MaiMai can obtain during a certain process.

### 🎯 Tool Characteristics

- 🔍 **Information Acquisition Enhancement**: Expand MaiMai's ability to obtain external information
- 📊 **Data Enrichment**: Help MaiMai obtain more background information and real-time data
- 🔌 **Plugin Architecture**: Support independent development and registration of new tools
- ⚡ **Auto Discovery**: Tools are automatically recognized and registered by the system

### 🆚 Tool vs Action vs Command Differences

| Feature | Action | Command | Tool |
|-----|-------|---------|------|
| **Main Purpose** | Expand MaiMai behavior capabilities | Respond to user instructions | Expand MaiMai information acquisition |
| **Trigger Method** | MaiMai intelligent decision | User active trigger | LLM calls as needed |
| **Goal** | Let MaiMai do more things | Provide specific functionality | Let MaiMai know more information |
| **Usage Scenario** | Enhance interaction experience | Functional service | Information query and analysis |

## 🏗️ Basic Structure of Tool Component

Each tool must inherit from `BaseTool` base class and implement the following properties and methods:

```python
from src.plugin_system import BaseTool

class MyTool(BaseTool):
    # Tool name, must be unique
    name = "my_tool"
    
    # Tool description, tells LLM the purpose of this tool
    description = "This tool is used to obtain specific types of information"
    
    # Parameter definition, only define parameters
    # For example, to define a parameter table similar to the following openai format, you can define it like this:
    # {
    #     "type": "object",
    #     "properties": {
    #         "query": {
    #             "type": "string",
    #             "description": "Query parameter"
    #         },
    #         "limit": {
    #             "type": "integer", 
    #             "description": "Result quantity limit"
    #         }
    #     },
    #     "required": ["query"]
    # }
    parameters = [
        ("query", "string", "Query parameter", True),  # Required parameter
        ("limit", "integer", "Result quantity limit", False)  # Optional parameter
    ]

    available_for_llm = True  # Whether available for LLM
    
    async def execute(self, function_args: Dict[str, Any]):
        """Execute tool logic"""
        # Implement tool functionality
        result = f"Query result: {function_args.get('query')}"
        
        return {
            "name": self.name,
            "content": result
        }
```

### Property Description

| Property | Type | Description |
|-----|------|------|
| `name` | str | Tool's unique identifier name |
| `description` | str | Tool function description, helps LLM understand purpose |
| `parameters` | list[tuple] | Parameter definition |

The constructed tool definition is:
```python
{"name": cls.name, "description": cls.description, "parameters": cls.parameters}
```

### Method Description

| Method | Parameters | Return Value | Description |
|-----|------|--------|------|
| `execute` | `function_args` | `dict` | Execute tool core logic |

---

## 🎨 Complete Tool Example

Complete a weather query tool

```python
from src.plugin_system import BaseTool
import aiohttp
import json

class WeatherTool(BaseTool):
    """Weather Query Tool - Get real-time weather information for specified city"""
    
    name = "weather_query"
    description = "Query real-time weather information for specified city, including temperature, humidity, weather conditions, etc."
    available_for_llm = True  # Allow LLM to call this tool
    parameters = [
        ("city", "string", "City name to query weather for, e.g.: Beijing, Shanghai, New York", True),
        ("country", "string", "Country code, e.g.: CN, US, optional parameter", False)
    ]
    
    async def execute(self, function_args: dict):
        """Execute weather query"""
        try:
            city = function_args.get("city")
            country = function_args.get("country", "")
            
            # Build query parameters
            location = f"{city},{country}" if country else city
            
            # Call weather API (example)
            weather_data = await self._fetch_weather(location)
            
            # Format result
            result = self._format_weather_data(weather_data)
            
            return {
                "name": self.name,
                "content": result
            }
            
        except Exception as e:
            return {
                "name": self.name,
                "content": f"Weather query failed: {str(e)}"
            }
    
    async def _fetch_weather(self, location: str) -> dict:
        """Get weather data"""
        # This is an example, actual implementation needs to connect to real weather API
        api_url = f"http://api.weather.com/v1/current?q={location}"
        
        async with aiohttp.ClientSession() as session:
            async with session.get(api_url) as response:
                return await response.json()
    
    def _format_weather_data(self, data: dict) -> str:
        """Format weather data"""
        if not data:
            return "No weather data available"
        
        # Extract key information
        city = data.get("location", {}).get("name", "Unknown city")
        temp = data.get("current", {}).get("temp_c", "Unknown")
        condition = data.get("current", {}).get("condition", {}).get("text", "Unknown")
        humidity = data.get("current", {}).get("humidity", "Unknown")
        
        # Format output
        return f"""
🌤️ {city} Real-time Weather
━━━━━━━━━━━━━━━━━━
🌡️ Temperature: {temp}°C
☁️ Weather: {condition}
💧 Humidity: {humidity}%
━━━━━━━━━━━━━━━━━━
        """.strip()
```

---

## 🚨 Notes and Limitations

### Current Limitations

1. **Applicable Scope**: Mainly suitable for information acquisition scenarios
2. **Configuration Requirements**: Must enable tool processor

### Development Suggestions

1. **Function Specificity**: Each tool focuses on single function
2. **Parameter Clarity**: Clearly define tool parameters and purposes
3. **Error Handling**: Complete exception handling and error feedback
4. **Performance Consideration**: Avoid long blocking operations
5. **Information Accuracy**: Ensure accuracy and timeliness of obtained information

## 🎯 Best Practices

### 1. Tool Naming Conventions
#### ✅ Good Naming
```python
name = "weather_query"        # Clearly expresses function
name = "knowledge_search"     # Strong descriptive
name = "stock_price_check"    # Clear functionality
```
#### ❌ Naming to Avoid
```python
name = "tool1"               # Meaningless
name = "wq"                  # Too short
name = "weather_and_news"    # Function too complex
```

### 2. Description Conventions
#### ✅ Good Description
```python
description = "Query real-time weather information for specified city, including temperature, humidity, weather conditions"
```
#### ❌ Description to Avoid
```python
description = "Weather"         # Too simple
description = "Get information"      # Not specific enough
```

### 3. Parameter Design

#### ✅ Reasonable Parameter Design
```python
parameters = [
    ("city", "string", "City name, e.g.: Beijing, Shanghai", True),
    ("unit", "string", "Temperature unit: celsius or fahrenheit", False)
]
```
#### ❌ Parameter Design to Avoid
```python
parameters = [
    ("data", "string", "Data", True)  # Parameter too vague
]
```

### 4. Result Formatting
#### ✅ Good Result Format
```python
def _format_result(self, data):
    return f"""
🔍 Query Result
━━━━━━━━━━━━
📊 Data: {data['value']}
📅 Time: {data['timestamp']}
📝 Description: {data['description']}
━━━━━━━━━━━━
    """.strip()
```
#### ❌ Result Format to Avoid
```python
def _format_result(self, data):
    return str(data)  # Directly return raw data
```