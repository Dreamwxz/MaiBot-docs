# MaiBot TTS Voice Configuration Guide ✨

This document is a configuration guide for the MaiBot TTS Adapter prepared by Mo Bailing~.

This Adapter is used to convert MaiBot's text messages into voice, which is then sent to QQ through Napcat.

Therefore, this adapter requires **both the Napcat adapter and the MaiBot TTS adapter to be installed!**

::: tip
However, if other platform message adapters you use declare compatibility with this adapter, then the Napcat adapter may not need to be installed.
:::

::: tip
If you encounter difficulties during configuration and don't have a strong need for voice functionality, using text chat directly is also a great choice~ Bailing believes everyone can find the most suitable way for themselves! ❤️
:::

## Installation Steps
Installing this adapter is also very simple, just like installing the Napcat adapter. Simply download the adapter files from the [GitHub repository](https://github.com/MaiM-with-u/maimbot_tts_adapter), install dependencies, do some configuration, and then start it~
```bash
git clone https://github.com/MaiM-with-u/maimbot_tts_adapter.git
cd maimbot_tts_adapter
pip install -r requirements.txt -i https://mirrors.aliyun.com/pypi/simple --upgrade
python main.py
```

## Configuration File Details

Compared to the Napcat adapter, the TTS adapter's configuration file is a bit more complex, but Bailing believes everyone can understand it and successfully overcome all difficulties to make your little robot speak!

All configuration sample files for the TTS adapter are in the `template_configs` folder. You can choose different voice service providers according to your preferences and needs.

First, we need to copy a configuration file template from `template_configs/base_template.toml` to `configs/base.toml`.

First, let me introduce the basic configuration file `base.toml`.
```toml
[server]
host = "127.0.0.1"
port = 8070

[routes]
qq = "http://127.0.0.1:8000/ws"
[probability]
voice_probability = 0.2 # Probability of using voice

[enabled_tts] # Enabled TTS modules, please match the directory names of each plugin
enabled = ["GPT_Sovits"]

[tts_base_config]
stream_mode = false  # Whether to enable streaming output
post_process = false # Whether to enable post-processing (currently ineffective)

[debug]
logging_level = "INFO" # Log level
```

Before starting detailed configuration explanations, let me use a diagram to show the data flow between the Napcat adapter, TTS adapter, and MaiBot core:

**Original:**

```mermaid
graph LR
    A[Napcat Adapter] <--> B[MaiBot Core]
```

**After enabling TTS:**

```mermaid
graph LR
    A[Napcat Adapter] <--> B[TTS Adapter]
    B <--> C[MaiBot Core]
```

I believe everyone has noticed that now the Napcat Adapter's server no longer points directly to the MaiBot core, but to our TTS adapter! Therefore, we need to slightly modify a few small places in the **Napcat Adapter**'s `config.toml` file:

```toml
[MaiBot_Server]
platform_name = "qq"
host = "localhost"
port = 8070
[Voice]
use_tts = true
```

Here, `host` should correspond to the `host` field in our `base.toml`, and `port` should also correspond to the `port` field in `base.toml`.

Next, let me continue explaining the other configuration items in the `base.toml` file.

### `routes` Route Configuration
In this configuration, the key name represents the name of the adapter platform being used. I recommend using `qq` directly because the Napcat Adapter's default platform name is `qq`. Of course, if you're using other adapters, you can modify it here to the name of the adapter you're using.

For the URL value, you just need to combine the values of the `HOST` and `PORT` fields from the MaiBot main program's `.env` file (for example, if HOST is `127.0.0.1` and PORT is `8000`, then combined it's `http://127.0.0.1:8000`), then add `/ws` at the end of this address to form the URL in the example.

### `probability` Voice Probability
This field is used to set the probability of voice, calculated as a percentage.

### `enabled_tts` Enabled TTS Modules
This field is used to specify which TTS module to enable. When using it, fill in the folder name corresponding to the TTS module you want to use here. This item is case-sensitive.
::: info Bailing's Tips
The built-in supported voice modules are:
1. [GPT_Sovits](./gpt_sovits) 
2. [Qwen_omni](./qwen_omni)
3. [Doubao_TTS](./doubao_tts)

When filling in content, please choose from the list above and pay attention to case sensitivity~
:::
Although theoretically several module names can be filled in here, the current version of this adapter only supports enabling one voice module at a time. Just choose your favorite one from the list and fill it in.

After selecting and filling in, you can click the links in the list above to directly jump to the detailed configuration documentation for the corresponding module and continue with subsequent setup work.

### `tts_base_config` TTS Basic Configuration
This field is used to set the basic configuration of the voice module. I require everyone to keep it at default.

- `stream_mode`: Whether to enable streaming output.
- `post_process`: Whether to enable post-processing (currently ineffective).

I require you not to change these two configurations unless you know what you're doing. For actual effects, please refer to the code.

### `debug` Debug Configuration
This field is currently only used to identify the log level.

- `logging_level`: Log level, default is `INFO`.

::: tip Bailing's Troubleshooting Tips
If you encounter problems during use, you can check the Troubleshooting section
:::