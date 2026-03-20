# MaiBot Napcat Adapter Documentation

## Installation
Installing this adapter is very simple. Just download the adapter files from the [GitHub repository](https://github.com/MaiM-with-u/MaiBot-Napcat-Adapter), install dependencies, and start it with the appropriate environment.
```bash
git clone https://github.com/MaiM-with-u/MaiBot-Napcat-Adapter.git
pip install -r requirements.txt -i https://mirrors.aliyun.com/pypi/simple --upgrade
python main.py
```

## Configuration
The Napcat adapter configuration template file is located at `template/config_template.toml`. You first **need to copy this file, rename it to config.toml, and place it in the root directory**, then modify it according to your needs.

### Deployment Preparation

#### Folder Structure

A typical deployment folder structure is as follows:

```
Maim-with-u
├── MaiBot
│   ├── changelogs
│   ├── config
│   ├── data
│   ├── depends-data
│   ├── src
│   │   └── ...
│   └── template
└── MaiBot-Napcat-Adapter
    ├── config.toml
    └── template
```

#### Initialize Configuration File

You can use the following commands to copy the configuration file:

```bash
# Linux/macOS
cp template/config_template.toml config.toml

# Windows
# Manually copy template/config_template.toml to the root directory and rename it to config.toml
```

### Napcat Settings

1. **Install Napcat**: Please refer to the [Napcat official documentation](https://napneko.github.io/guide/boot/Shell) to install the Shell version or Framework version.

2. **Configure Websocket Client**:
   - Create a new `websocket client` in Napcat
   - Set the reverse proxy URL (e.g., `ws://localhost:8095/`)
   
   > [!IMPORTANT]
   > Configuration example:
   > ![](/images/napcat_websockets_client.png)

### Key Field Configuration

Before editing `config.toml`, you need to understand the correspondence of the following key fields:

- `[Napcat_Server]` `port` should match the port in the Napcat reverse proxy URL (default 8095)
- `[MaiBot_Server]` `port` should match the `PORT` in MaiBot's `.env` file (default 8000)
- `[Napcat_Server]` `heartbeat` should match the heartbeat interval in Napcat's reverse proxy settings
  - **Note**: The interval in Napcat is in milliseconds, while in the configuration file it's in seconds (e.g., if Napcat is set to 30000 milliseconds, enter 30)

**MaiBot .env file example**:
```ini
HOST=127.0.0.1
PORT=8000
```

The specific configuration items are explained below:

```toml
[Nickname] # Currently unused
nickname = ""

[Napcat_Server] # Napcat connection ws service settings
host = "localhost" # Host address set in Napcat
port = 8095        # Port set in Napcat
heartbeat = 30     # Same as Napcat heartbeat setting (in seconds)

[MaiBot_Server] # Connection to MaiBot ws service settings
platform_name = "qq" # Name identifying the adapter (required)
host = "localhost"   # Host address set in MaiBot's .env file, i.e., the HOST field
port = 8000          # Port set in MaiBot's .env file, i.e., the PORT field

[Chat] # Whitelist/blacklist functionality
group_list_type = "whitelist" # Group list type, options: whitelist, blacklist
group_list = [] # Group list
# When group_list_type is whitelist, only groups in the group list can chat
# When group_list_type is blacklist, any groups in the group list cannot chat
private_list_type = "whitelist" # Private chat list type, options: whitelist, blacklist
private_list = [] # Private chat list
# When private_list_type is whitelist, only users in the private chat list can chat
# When private_list_type is blacklist, any users in the private chat list cannot chat
ban_user_id = [] # Global ban list (users in the global ban list cannot chat at all)
ban_qq_bot = false # Whether to block QQ official bots
enable_poke = true # Whether to enable poke functionality

[Voice] # Voice sending settings
use_tts = false # Whether to use TTS voice (make sure you have configured TTS and have the corresponding adapter)

[Debug]
level = "INFO" # Log level (DEBUG, INFO, WARNING, ERROR)
```

Let me explain each item in the configuration file in detail:

### Napcat_Server
- `host`: Host address set in Napcat, usually `localhost`.
- `port`: Port set in Napcat, usually `8095`.
- `heartbeat`: Same as Napcat heartbeat setting (in seconds), usually `30`.

The combination of `host` and `port` in this section forms the ws service address set in Napcat. I recommend using the default values.

### MaiBot_Server
- `platform_name`: Name identifying the adapter, usually `qq`.
- `host`: Host address set in MaiBot's `.env` file, i.e., the `HOST` field.
- `port`: Port set in MaiBot's `.env` file, i.e., the `PORT` field.

This `platform_name` is used to identify the adapter. Since the Napcat adapter is specifically designed for QQ, I recommend using `qq`, but you can also use other names.

### Chat
- `group_list_type`: Group list type, options: `whitelist`, `blacklist`.
- `group_list`: Group list.

When `group_list_type` is `whitelist`, only groups in the group list can chat. This means only groups explicitly listed in `group_list` will be allowed to interact with the bot, all other unlisted groups will be prohibited.

When `group_list_type` is `blacklist`, any groups in the group list cannot chat. This means groups listed in `group_list` will be prohibited from interacting with the bot, while unlisted groups can use the chat function normally.

- `private_list_type`: Private chat list type, options: `whitelist`, `blacklist`.
- `private_list`: Private chat list.

This setting is similar to `group_list`, but for private chats.

- `ban_user_id`: Global ban list (users in the global ban list cannot chat at all).

This setting is a global ban list. Any user in this list cannot chat with the bot at all, regardless of whether they try to interact with the bot in group chats.

- `ban_qq_bot`: Whether to block QQ official bots.
If you don't want QQ official bots to interact with MaiBot, set this option to `true`.

- `enable_poke`: Whether to enable poke functionality.

Literal meaning~

### Voice
- `use_tts`: Whether to use TTS voice.

If you want to use TTS voice functionality, make sure you have configured TTS and have the corresponding adapter. Then set this option to `true`.

### Debug
- `level`: Log level (`DEBUG`, `INFO`, `WARNING`, `ERROR`).
- `level`: Log level, I recommend using `INFO` to avoid excessive log information interference.