# Maim_Message Message Class
This is the base message class for Maim_Message, serving as the fundamental data structure for messages sent and received by Adapters.

## class Seg:
This is the recursive basic message unit, used to represent different parts and types of messages.

```python
@dataclass
class Seg:
    type: str
    data: Union[str, List["Seg"]]

    @classmethod
    def from_dict(cls, data: Dict) -> "Seg":

    def to_dict(self) -> Dict:
```

`type`: Marks the Seg message segment type. MaiBot Core currently accepts types: `[text, image, emoji, seglist]`

`data`: Specific message content, differentiated by `type`.

<div class="indent">

When `type="text"`, `data` is a string.

When `type="image"`, `data` is the **headerless base64 encoding** of the image.

When `type="emoji"`, `data` is the **headerless base64 encoding** of the emoji image.

When `type="voice"`, `data` is the headerless base64 encoding of uncompressed WAV format audio.

When `type="reply"`, `data` is the message_id of the replied object.

When `type="seglist"`, `data` is a list of Seg objects.

When `type="command"`, `data` is a dictionary containing the command name and parameters. Detailed parameter list is in [Seg Command List](/en/develop/maim_message/command_args)

</div>

`from_dict`: Converts a dictionary to a Seg object. Used for creating Seg objects from dictionary format data. You don't need to call this manually for parsing.

`to_dict`: Converts a Seg object to a dictionary. Used for transmitting or storing Seg objects in dictionary format. You don't need to call this manually.

## class GroupInfo:
This is group message metadata, used to represent group name, group ID, and the platform where the group is located.

```python
@dataclass
class GroupInfo:
    platform: str
    group_id: str
    group_name: Optional[str] = None  # Group name

    def to_dict(self) -> Dict:

    @classmethod
    def from_dict(cls, data: Dict) -> "GroupInfo":
```

`platform`: Marks the platform where the group is located.

`group_id`: Marks the unique ID of the group.

`group_name`: Marks the name of the group (optional).

`from_dict`: Converts a dictionary to a GroupInfo object. You don't need to call this manually for parsing.

`to_dict`: Converts a GroupInfo object to a dictionary. You don't need to call this manually.

## class UserInfo:
This is user message metadata, used to represent user platform, user ID, user nickname/username, and the user's group nickname.

```python
@dataclass
class UserInfo:
    platform: str
    user_id: str
    user_nickname: Optional[str] = None  # User nickname
    user_cardname: Optional[str] = None  # User group nickname

    def to_dict(self) -> Dict:

    @classmethod
    def from_dict(cls, data: Dict) -> "UserInfo":
```

`platform`: Marks the platform where the user is located.

`user_id`: Marks the unique ID of the user.

`user_nickname`: User's nickname/username (optional).

`user_cardname`: User's nickname within the group (optional).

`to_dict`: Converts a UserInfo object to a dictionary. You don't need to call this manually.

`from_dict`: Converts a dictionary to a UserInfo object. You don't need to call this manually for parsing.

## class FormatInfo:
This is format information metadata, used to represent formats that Adapters can send and accept.

```python
@dataclass
class FormatInfo:
    content_format: List["str"] = None
    accept_format: Optional[List["str"]] = None

    def to_dict(self) -> Dict:

    @classmethod
    def from_dict(cls, data: Dict) -> "FormatInfo":
```

`content_format`: Marks the content types contained in this message, **required**, defaults to empty. Tells MaiBot Core the types of content this message contains.

`accept_format`: Marks the format information that the Adapter can accept, **required**, defaults to empty. Tells MaiBot Core the format information that can be sent.

**The format information above should match the valid type types of Seg when filled in.**

`to_dict`: Converts a FormatInfo object to a dictionary. You don't need to call this manually.

`from_dict`: Converts a dictionary to a FormatInfo object. You don't need to call this manually for parsing.

## class TemplateInfo:
This is template information metadata.

```python
@dataclass
class TemplateInfo:
    template_items: Optional[Dict[str, str]] = None
    template_name: Optional[Dict[str, str]] = None
    template_default: bool = True

    def to_dict(self) -> Dict:

    @classmethod
    def from_dict(cls, data: Dict) -> "TemplateInfo":
```

`template_items`: Marks the content of the template, defaults to empty.

`template_name`: Marks the name of the template, defaults to empty.

`template_default`: Marks whether it's the default template, defaults to True.

`to_dict`: Converts a TemplateInfo object to a dictionary. You don't need to call this manually.

`from_dict`: Converts a dictionary to a TemplateInfo object. You don't need to call this manually for parsing.

## class BaseMessageInfo
Complete metadata information for messages.

```python
@dataclass
class BaseMessageInfo:
    platform: str
    message_id: str
    time: float
    group_info: Optional[GroupInfo] = None
    user_info: UserInfo
    format_info: FormatInfo
    template_info: Optional[TemplateInfo] = None
    additional_config: Optional[dict] = None

    def to_dict(self) -> Dict:

    @classmethod
    def from_dict(cls, data: Dict) -> "BaseMessageInfo":
```

`platform`: Marks the platform where the message is located.

`message_id`: Marks the unique ID of the message.

`time`: Marks the timestamp of the message.

`group_info`: Marks the group information of the message. Set to empty for private messages, should be a GroupInfo object for group messages.

`user_info`: Marks the user information of the message.

`format_info`: Marks the format information of the message, indicating formats that Adapters can send and accept.

`template_info`: Marks the template information of the message (optional).

`additional_config`: Marks additional configuration for the message (optional). Configuration should be filled based on downstream acceptable configurations.

| Currently available configuration items | Optional values | Description |
| --- | --- | ---|
| `maimcore_reply_probability_gain`* | Any numerical value | Reply probability gain (100% is 1) |
| `allow_tts` | `True`/`False` | Whether TTS voice is allowed** |
| `original_text`*** | Any string | Original text data carried in voice data returned from TTS Adapter |

>*: Temporarily disabled due to code development
>
>**: Requires self-configuration of corresponding TTS engine, along with a corresponding TTS Adapter
>
>***: This item is returned by [maimbot_tts_adapter](https://github.com/tcmofashi/maimbot_tts_adapter), not supported by MaiBot Core itself

`to_dict`: Converts a BaseMessageInfo object to a dictionary. You don't need to call this manually.

`from_dict`: Converts a dictionary to a BaseMessageInfo object. You don't need to call this manually for parsing.

## class MessageBase
Complete message object for sending and receiving.

```python
@dataclass
class MessageBase:
    message_info: BaseMessageInfo
    message_segment: Seg
    raw_message: Optional[str] = None  # Original message containing unparsed cq codes

    def to_dict(self) -> Dict:

    @classmethod
    def from_dict(cls, data: Dict) -> "MessageBase":
```

`message_info`: Marks the metadata information of the message, a BaseMessageInfo object.

`message_segment`: Marks the content of the message, a Seg object.

`raw_message`: Marks the original content of the message, containing unparsed cq codes (optional).

`to_dict`: Converts a MessageBase object to a dictionary. You don't need to call this manually.

`from_dict`: Converts a dictionary to a MessageBase object. **Please call this after receiving a message to convert the dictionary to a MessageBase object.**