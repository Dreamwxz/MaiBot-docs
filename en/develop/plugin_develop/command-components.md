# 💻 Command Component Details

## 📖 What is Command

Command is a component that directly responds to user explicit instructions. Unlike Action, Command is **passively triggered**, executing immediately when users input specific format commands.

Command matches user input through regular expressions, providing deterministic functional services.

### 🎯 Command Characteristics

- 🎯 **Deterministic Execution**: Execute immediately when command matched, no randomness
- ⚡ **Instant Response**: User actively triggers, quick response
- 🔍 **Regex Matching**: Precisely match user input through regular expressions
- 🛑 **Interception Control**: Can control whether to prevent message from continuing processing
- 📝 **Parameter Parsing**: Support extracting parameters from user input

---

## 🛠️ Basic Structure of Command Component

First, Command component needs to inherit from `BaseCommand` class and implement necessary methods.

```python
class ExampleCommand(BaseCommand):
    command_name = "example" # Command name, as unique identifier
    command_description = "This is an example command" # Command description
    command_pattern = r"" # Regular expression for command matching

    async def execute(self) -> Tuple[bool, Optional[str], bool]:
        """
        Main logic for executing Command

        Returns:
            Tuple[bool, str, bool]: 
                - First bool indicates whether execution successful
                - Second str is execution result message
                - Third bool indicates whether need to prevent message from continuing processing
        """
        # ---- Command execution logic ----
        return True, "Execution successful", False
```

**`command_pattern`**: The regular expression this Command matches, used for precisely matching user input.

Please note: If you want to get parameters from the command, please use named capture groups in the regular expression, for example `(?P<param_name>pattern)`.

This way when matching, the internal implementation can use `re.match.groupdict()` method to get all capture group parameters, and store them in dictionary form in `self.matched_groups`.

### Matching Example

Suppose we have a command `/example param1=value1 param2=value2`, the corresponding regular expression could be:

```python
class ExampleCommand(BaseCommand):
    command_name = "example"
    command_description = "This is an example command"
    command_pattern = r"/example (?P<param1>\w+) (?P<param2>\w+)"

    async def execute(self) -> Tuple[bool, Optional[str], bool]:
        # Get matched parameters
        param1 = self.matched_groups.get("param1")
        param2 = self.matched_groups.get("param2")
        
        # Execution logic
        return True, f"Parameter 1: {param1}, Parameter 2: {param2}", False
```

---

## Command Built-in Methods Description

```python
class BaseCommand:
    def get_config(self, key: str, default=None):
        """Get plugin configuration value, using nested key access"""

    async def send_text(self, content: str, reply_to: str = "") -> bool:
        """Send reply message"""

    async def send_type(self, message_type: str, content: str, display_message: str = "", typing: bool = False, reply_to: str = "") -> bool:
        """Send specified type reply message to current chat environment"""

    async def send_command(self, command_name: str, args: Optional[dict] = None, display_message: str = "", storage_message: bool = True) -> bool:
        """Send command message"""

    async def send_emoji(self, emoji_base64: str) -> bool:
        """Send emoji"""

    async def send_image(self, image_base64: str) -> bool:
        """Send image"""
```

For specific parameters and usage, see the definition of `BaseCommand` base class.