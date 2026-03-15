# Emoji API

The Emoji API module provides emoji acquisition, querying, and management functionality, allowing plugins to intelligently select and use emojis.

## Import Method

```python
from src.plugin_system.apis import emoji_api
# or
from src.plugin_system import emoji_api
```

## Two-Step Recognition Optimization

Starting from the new version, the emoji recognition system adopts a **two-step recognition + intelligent caching** optimization solution:

### **Recognition Process When Receiving Emojis**
1. **Step One**: VLM visual analysis - generates detailed description
2. **Step Two**: LLM emotion analysis - extracts core emotion tags based on detailed description
3. **Caching Mechanism**: Caches emotion tags to database, saves detailed description to Images table

### **Optimization When Registering Emojis**
- **Intelligent Reuse**: Prioritizes obtaining existing detailed descriptions from Images table
- **Avoid Duplication**: Skips VLM calls if emoji was previously received
- **Performance Improvement**: Reduces unnecessary AI calls, lowers latency and cost

### **Caching Strategy**
- **ImageDescriptions table**: Caches final emotion tags (for quick display)
- **Images table**: Saves detailed descriptions (for reuse during registration)
- **Double Check**: Prevents duplicate generation in concurrent situations

## Main Functions

### 1. Emoji Acquisition
```python
async def get_by_description(description: str) -> Optional[Tuple[str, str, str]]:
```
Select emoji based on scene description

**Args:**
- `description`: Emoji description text, such as "happy", "sad", "angry", etc.

**Returns:**
- `Optional[Tuple[str, str, str]]`: A tuple: (emoji base64 encoding, description, emotion tag), returns None if no matching emoji found

#### Example
```python
emoji_result = await emoji_api.get_by_description("laughing")
if emoji_result:
    emoji_base64, description, matched_scene = emoji_result
    print(f"Acquired emoji: {description}, scene: {matched_scene}")
    # Can use emoji_base64 for sending emoji
```

### 2. Random Emoji Acquisition
```python
async def get_random(count: Optional[int] = 1) -> List[Tuple[str, str, str]]:
```
Randomly acquire specified number of emojis

**Args:**
- `count`: Number of emojis to acquire, default is 1

**Returns:**
- `List[Tuple[str, str, str]]`: A list containing multiple emojis, each element is a tuple: (emoji base64 encoding, description, emotion tag), returns empty list if not found or error occurs

### 3. Get Emoji by Emotion
```python
async def get_by_emotion(emotion: str) -> Optional[Tuple[str, str, str]]:
```
Get emoji based on emotion tag

**Args:**
- `emotion`: Emotion tag, such as "happy", "sad", "angry", etc.

**Returns:**
- `Optional[Tuple[str, str, str]]`: A tuple: (emoji base64 encoding, description, emotion tag), returns None if not found

### 4. Get Emoji Count
```python
def get_count() -> int:
```
Get current available emoji count

### 5. Get Emoji System Information
```python
def get_info() -> Dict[str, Any]:
```
Get basic information about the emoji system

**Returns:**
- `Dict[str, Any]`: Dictionary containing emoji count, description, and other information, includes the following keys:
    - `current_count`: Current emoji count
    - `max_count`: Maximum emoji count
    - `available_emojis`: Current available emoji count

### 6. Get All Available Emotion Tags
```python
def get_emotions() -> List[str]:
```
Get all available emotion tags **(already deduplicated)**

### 7. Get All Emoji Descriptions
```python
def get_descriptions() -> List[str]:
```
Get all emoji description list

## Scene Description Explanation

### Common Scene Descriptions
The emoji system supports various specific scene descriptions, examples include:

- **Happy Scenes**: Happy laughter, satisfied smile, excited dancing
- **Helpless Scenes**: Expressing helplessness and frustration, slight sarcasm, speechless shaking head
- **Angry Scenes**: Anger and dissatisfaction, angry glare, furious rage
- **Surprised Scenes**: Shocked expression, unexpected discovery, confused thinking
- **Cute Scenes**: Cute expression, coquettish action, shy appearance

### Emotion Keyword Examples
Emotion keywords supported by the system include:
- Laughing, smiling, excited, dancing
- Helpless, frustrated, sarcastic, speechless, shaking head
- Angry, dissatisfied, angry, glaring, raging
- Shocked, surprised, confused, thinking
- Cute, coquettish, shy, adorable

### Matching Mechanism
- **Exact Match**: Prioritizes matching complete scene descriptions, such as "happy laughter"
- **Keyword Match**: If no exact match, performs fuzzy matching based on keywords
- **Semantic Match**: System understands semantic meaning of scenes for intelligent matching

## Notes

1. **Asynchronous Functions**: Some functions are asynchronous and require `await`
2. **Return Format**: Emojis returned as base64 encoding, can be directly used for sending
3. **Error Handling**: All functions have error handling, returns None, empty list, or default value on failure
4. **Usage Statistics**: System records emoji usage counts
5. **File Dependency**: Emojis depend on local files, ensure emoji files exist
6. **Encoding Format**: Returns base64 encoded image data, can be directly used for network transmission
7. **Scene Understanding**: System understands specific scene descriptions, more accurate than simple emotion classification