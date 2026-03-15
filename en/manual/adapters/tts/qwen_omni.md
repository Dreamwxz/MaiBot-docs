# Voice Module - Qwen Omni TTS Configuration Guide ✨

This module interfaces with the TTS voice synthesis service provided by the Qwen multimodal model!

First, we need to copy a configuration file template from `template_configs/Qwen_omni_template.toml` to `configs/Qwen_omni.toml`.

```toml
api_key = "your_api_key" # API key
base_url = "https://dashscope.aliyuncs.com/compatible-mode/v1" # API address
model_name = "qwen-omni-turbo" # Model name
voice_character = "Chelsie" # Voice character
media_format = "wav" # Audio format, please use wav format
```

## Configuration Item Details

### `api_key` API Key
This is where you fill in the API key for the Qwen service. You need to go to Alibaba Cloud's [DashScope Console](https://dashscope.console.aliyun.com/) to create and obtain your API Key.

:::warning Bailing's Warning
API Key is your important credential, please keep it safe and don't leak it to others!
:::

### `base_url` API Address
This is the API interface address for the Qwen service. Usually no modification is needed, just keep it as default.

### `model_name` Model Name
This specifies the model name to use. You can choose different models according to your needs. Note, please be sure to choose a multimodal model that supports voice output! For example, `qwen-omni-turbo` is a good choice.

#### `voice_character` Voice Character
Here you can configure the voice character you want. Qwen provides multiple different voice character options, you can choose according to your preference.
::: tip Bailing's Tip (2025/05/22 version)
Currently, the [official documentation](https://help.aliyun.com/zh/model-studio/qwen-omni) lists supported voice characters as:
- "Cherry" **（not supported by open source version models）**
- "Serena" **（not supported by open source version models）**
- "Ethan"
- "Chelsie"
:::

### `media_format` Audio Format
This configures the audio format to use.

Currently, the official only supports `wav` format, so please modify according to the official documentation~.