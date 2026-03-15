# ⚡ Action Component Details

## 📖 What is Action

Action is an intelligent component that provides additional functionality to MaiMai beyond replies, **autonomously selected by MaiMai's decision system**, with random and anthropomorphic invocation characteristics. Action does not directly respond to user commands, but allows MaiMai to intelligently choose appropriate actions based on chat context, making its behavior more natural and realistic.

### Action Characteristics

- 🧠 **Intelligent Activation**: MaiMai intelligently judges whether to use it based on multiple conditions
- 🎲 **Randomness**: Can use random numbers for activation, increasing behavioral unpredictability, closer to real human communication
- 🤖 **Anthropomorphic**: Makes MaiMai's responses more natural and personalized
- 🔄 **Context Awareness**: Makes appropriate reactions based on chat context

---

## 🎯 Basic Structure of Action Component

First, all Actions should inherit from the `BaseAction` class.

Second, each Action component should implement the following basic information:

```python
class ExampleAction(BaseAction):
    action_name = "example_action" # Action's unique identifier
    action_description = "This is an example action for testing and demonstration" # Action description
    activation_type = ActionActivationType.ALWAYS # Default use ALWAYS, this Action is always available 
    associated_types = ["text", "emoji", ...] # Associated types, what types of data this plugin will use
    parallel_action = False # Less likely to be used with other actions
    action_parameters = {"param1": "Parameter 1 description", "param2": "Parameter 2 description", ...}
    # Action usage scenario description - helps LLM judge when to "choose" to use
    action_require = ["Usage scenario description 1", "Usage scenario description 2", ...]

    async def execute(self) -> Tuple[bool, str]:
        """
        Main logic for executing Action
        
        Returns:
            Tuple[bool, str]: (whether successful, execution result description)
        """
        # ---- Action execution logic ----
        return True, "Execution successful"
```

#### associated_types: The message types this Action will send, such as text, emoji, etc.

This part is passed by Adapter to the processor.

Taking MaiBot-Napcat-Adapter as an example, optional items are as follows:

| Type | Description | Format |
| --- | --- | --- |
| text | Text message | str |
| emoji | Emoji message | str: Headless base64 of emoji |
| image | Image message | str: Headless base64 of image |
| reply | Reply message | str: Reply message ID |
| voice | Voice message | str: Headless base64 of wav format voice |
| command | Command message | See Adapter documentation |
| voiceurl | Voice URL message | str: URL of wav format voice |
| music | Music message | str: Music ID of this song on NetEase Cloud Music |
| videourl | Video URL message | str: Video URL |
| file | File message | str: File path |

**Please note that different processors may support different message types. Please pay attention during development.**

#### action_parameters: Parameter description for this Action.

This is a dictionary where keys are parameter names and values are parameter descriptions. This field helps LLM understand how to use this Action, and LLM returns corresponding parameters, finally passed to the Action's **`action_data`** attribute. Its format is exactly the same as what you defined **(unless LLM makes a mistake and returns incorrect content)**.

---

## Action Invocation Decision Mechanism

Action adopts a **two-layer decision mechanism** to optimize performance and decision quality:

**First Layer: Activation Control**

Activation determines whether this Action enters the decision candidate pool. Actions not activated will never be chosen by MaiMai.

**Second Layer: Usage Decision**

After Action is activated, usage conditions determine when MaiMai will **"choose"** to use this Action.

### Decision Parameter Details

#### First Layer: ActivationType Description

| Activation Type | Description | Usage Scenarios |
| ----------- | ---------------------------------------- | ---------------------- |
| [`NEVER`](#never-activation)     | Never activated, Action invisible to MaiMai | Temporarily disable an Action |
| [`ALWAYS`](#always-activation)    | Always activated, Action always in MaiMai's candidate pool | Core functionality, like reply, no reply |
| `RANDOM`    | Activation determined based on random probability | Functions that increase behavioral randomness |
| `KEYWORD`   | Activated when specific keywords detected | Functions with clear trigger conditions |

#### Activation Type Brief Description

- `NEVER`: Never activated (action always disabled)
    ```python
    class DisabledAction(BaseAction):
        activation_type = ActionActivationType.NEVER
    ```

- `ALWAYS`: Always activated (always available)
    ```python
    class AlwaysActivatedAction(BaseAction):
        activation_type = ActionActivationType.ALWAYS
    ```

- `RANDOM`: Random probability activation, need to specify probability
    ```python
    class SurpriseAction(BaseAction):
        activation_type = ActionActivationType.RANDOM
        random_activation_probability = 0.1  # 10% probability
    ```

- `KEYWORD`: Activated when keywords matched, need to set keyword list and whether case-sensitive
    ```python
    class GreetingAction(BaseAction):
        activation_type = ActionActivationType.KEYWORD
        activation_keywords = ["Hello", "hello", "hi", "hey"]
        keyword_case_sensitive = False
    ```

More KEYWORD type examples can be found in `ByeAction` under `plugins/hello_world_plugin`.

#### Second Layer: Usage Decision

**After Action is activated, usage conditions determine when MaiMai will "choose" to use this Action**.

This layer is comprehensively determined by the following factors:

- `action_require`: Usage scenario description, helps LLM judge when to choose
- `action_parameters`: Required parameters, affect Action executability
- Current chat context and MaiMai's decision logic

---

### Decision Process Example

```python
class EmojiAction(BaseAction):
    # First layer: Activation control
    activation_type = ActionActivationType.RANDOM  # Random activation
    random_activation_probability = 0.1  # 10% probability activation

    # Second layer: Usage decision
    action_require = [
        "Can choose to use when expressing emotions",
        "Increase chat fun",
        "Don't send multiple emojis consecutively"
    ]
```

**Decision Process**:

1. **First layer activation judgment**:

    - Use random number for decision, with 10% probability, MaiMai "knows" can use this Action
2. **Second layer usage decision**:

   - Even if Action is activated, MaiMai will also judge whether to actually choose to use it based on conditions in `action_require`
   - For example: If emojis were just sent, according to "Don't send multiple emojis consecutively" requirement, MaiMai might not choose this Action

---

## Action Built-in Properties Description

```python
class BaseAction:
    def __init__(self):
        # Message related properties
        self.log_prefix: str          # Log prefix
        self.group_id: str            # Group ID
        self.group_name: str          # Group name
        self.user_id: str             # User ID
        self.user_nickname: str       # User nickname
        self.platform: str            # Platform type (qq, telegram, etc.)
        self.chat_id: str             # Chat ID
        self.chat_stream: ChatStream  # Chat stream object
        self.is_group: bool           # Whether group chat

        # Message body
        self.action_message: dict     # Message data

        # Action related properties
        self.action_data: dict        # Data during Action execution
        self.thinking_id: str         # Thinking ID
```

action_message is a dictionary containing key-value pairs as follows (omitting unnecessary key-value pairs)

```python
{
    "message_id": "1234567890",  # Message id, str
    "time": 1627545600.0,  # Timestamp, float
    "chat_id": "abcdef123456",  # Chat ID, str
    "reply_to": None,  # Reply message id, str or None
    "interest_value": 0.85,  # Interest value, float
    "is_mentioned": True,  # Whether mentioned, bool
    "chat_info_last_active_time": 1627548600.0,  # Last active time, float
    "processed_plain_text": None,  # Processed text, str or None
    "additional_config": None,  # additional_config passed by Adapter, dict or None
    "is_emoji": False,  # Whether emoji, bool
    "is_picid": False,  # Whether picture ID, bool
    "is_command": False  # Whether command, bool
}
```

Please query the database for the format of some values.

---

## Action Built-in Methods Description

```python
class BaseAction:
    def get_config(self, key: str, default=None):
        """Get plugin configuration value, using nested key access"""
    
    async def wait_for_new_message(self, timeout: int = 1200) -> Tuple[bool, str]:
        """Wait for new message or timeout"""

    async def send_text(self, content: str, reply_to: str = "", reply_to_platform_id: str = "", typing: bool = False) -> bool:
        """Send text message"""

    async def send_emoji(self, emoji_base64: str) -> bool:
        """Send emoji"""

    async def send_image(self, image_base64: str) -> bool:
        """Send image"""

    async def send_custom(self, message_type: str, content: str, typing: bool = False, reply_to: str = "") -> bool:
        """Send custom type message"""

    async def store_action_info(self, action_build_into_prompt: bool = False, action_prompt_display: str = "", action_done: bool = True) -> None:
        """Store action information to database"""

    async def send_command(self, command_name: str, args: Optional[dict] = None, display_message: str = "", storage_message: bool = True) -> bool:
        """Send command message"""
```

For specific parameters and usage, see the definition of `BaseAction` base class.